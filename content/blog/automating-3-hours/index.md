---
title: Automating 3 hours of repetitive work to less than 1 minute
date: "2019-07-30T22:12:03.284Z"
description: "Everyday we do repetitive work in some areas of our lives, some things can be automated, this has been always the case lucky us: this is always an area of improvement."
---

Everyday we do repetitive work in some areas of our lives, some things can be automated, this has been always the case lucky us: this is always an area of improvement. If you detect something you do every day and you've found the way to automate it you can give it a try and do some automation, of course it can only pay back if the automated work takes less time than doing the repetitive task itself. An trendy example of it can be the automated cars, which are trying to automate something in particular: the commute we do every day; that repetitive work is on its way to be automated, there are other things that are automated as well, like processes in manufacturing which previously was done by workers doing repetitive manual work. In Software Development most of the cases is worth the time invested in automation to improve our workflow, for example: doing automation on testing, on deploying, on checking our code for correct spelling, correct spacing etc, most of the common usages are already automated and for those specific we need to act smart and automate them, decrease the repetitive work as much as possible, well, this was the case in my current job…

###The problem

Right now we have a big project, decouple a monolith to micro services for the backend and React in the frontend. One big limitation we have is that we cannot stop the development (adding features fixing bugs) and only focus on the new App, so we are incrementally adding React on top of what we currently have, doing separation from the monolith to a FE layer, in the monolith we have assets, the old way: A file that have all the bundles and creates modules for it, so this is the place to put our code to add React. We wanted to take some of the advantages of using React and isolating the project from the monolith, (having a mock server) this should speed up the development since we don't need to run the whole thing to be able to make progress on the migration. Works great isolated, the problem was when we needed to integrate the React changes into the monolith. We first test in the React project then we integrate it in the monolith, this lead us to repetitive work:

1. Build the files in the React Projects
1. Copy those files to the monolith source code
1. Update the references to these files in the resources file (because our project version has changed and our Webpack bundle files have changed too)
1. We are ready to refresh the browser and see our React changes on top of the monolith

   These tasks were done several times before we could say it was ready to be merged and create the PR (which is another step we repeatedly did)

1. Create Pull Request with React changes on top of the monolith with the package version updated
1. Update the Git Tag

All those steps took us some time, time of manual work that could be automated.

###The automation part
How the automation was done?
I started working on a shell script which basically just copied the files from the build folder to the resources in the monolith, it wasn't ambitious enough, a friend of mine suggested me to use Python (I wasn't that convinced at first but I decided to give it a try, ended up loving it), so the automation was continued in Python.

One problem, the automation script was written in Python but the scripts we run in React are in JS (Node) so first I needed to investigate how to run Python from Node and this is possible using [python-shell package](https://www.npmjs.com/package/python-shell). Solved!

####JavaScript
Was just matter of defining a run script in our package.json file:

```JavaScript
    "deploy-to-monolith": "node deployToMonolith.js"
```

That's it, now we have to put some JS code in action to run our .py script files:
deployToMonolith.js

```JavaScript
const { PythonShell } = require("python-shell")
const REACT_PROJECT_FOLDER_NAME = "myReactApp"
const scriptPath = "./monolith-integration"
const {
  npm_config_bumpPackageVersion,
  npm_config_createPR,
  npm_config_jiraTicketId,
  npm_config_branchShortDesc = "deployToMonolith",
} = process.env
// Follow instructions in the readme.md to know when to use parameters and when not to
// NOTE: PR should only be created when the version is bumped
const deployToMonolith = () => {
  if (npm_config_bumpPackageVersion) {
    let version = ""
    const bumpPackageVersion = new PythonShell("bumpPackageVersion.py", {
      scriptPath,
    })
    bumpPackageVersion.on("message", message => {
      if (message) {
        version = message
      }
      console.log(message)
    })
    bumpPackageVersion.end(error => {
      console.log("Bump package version finished")
      if (error) throw error
      const pushToReact = new PythonShell("pushTagToReact.py", {
        scriptPath,
        args: [npm_config_jiraTicketId, version],
      })
      pushToReact.end(error => {
        if (error) throw error
        console.log(`git tag ${version} pushed`)
        copyDistToMonolith(createPR, version)
      })
    })
  } else {
    // No need to bump package.json version but files need to be updated
    copyDistToMonolith()
  }
}
// Run this while TESTING integrations in Monolith run: `npm run deployToMonolith`
const copyDistToMonolith = (callback = () => {}, args) => {
  console.log("Building project...")
  PythonShell.run("buildProject.py", { scriptPath }, () => {
    console.log("Build project finished")
    console.log("Updating bundle files...")
    PythonShell.run(
      "copyDistToMonolith.py",
      { scriptPath, args: [REACT_PROJECT_FOLDER_NAME] },
      error => {
        console.log("Bundle and resources files in monolith updated")
        if (error) throw error
        callback(args)
      }
    )
  })
}
const createPR = (version = "") => {
  if (npm_config_createPR && npm_config_jiraTicketId) {
    const createPR = new PythonShell("createPR.py", {
      scriptPath,
      args: [
        npm_config_jiraTicketId,
        npm_config_branchShortDesc,
        version,
        REACT_PROJECT_FOLDER_NAME,
      ],
    })
    console.log("Creating PR...")
    createPR.on("message", message => {
      console.log(message)
    })
  }
}
deployToMonolith()
```

####Python

bumpPackageVersion.py

```Python
import sys, fileinput, os
import re

package_version = "  \"version\""
package_version_template = package_version + ": \"{0}\",\n"

def update_package_version () :
  for line in fileinput.input("package.json", inplace=True):
    if line.startswith(package_version) :
      new_version = get_new_version(line)
      line = package_version_template.format(new_version)
    sys.stdout.write(line)
  print("v" + new_version)

def get_new_version (line) :
  current_version = re.search('\d+\.\d+\.\d+', line)[0]
  split_version = current_version.split(".")
  # Currently this updates only the patch 0.0.X
  split_version[2] = str(int(split_version[2], 10) + 1)
  return ".".join(split_version)

update_package_version()
```

pushTagToReact.py

```Python
import os, sys, re, subprocess

jira_ticket_id = sys.argv[1]
version = sys.argv[2]

os.system("git add package.json")
os.system("git commit -m \"{0} - Bump version to {1} \"".format(jira_ticket_id, version))
os.system("git tag -a {0} -m \"Bump to {0}\"".format(version))
os.system("git push origin " + version)
os.system("git push origin " + re.search('.*heads\/(.+)\\\\', str(subprocess.run(["git symbolic-ref HEAD"], shell = True, capture_output = True))).group(1))
```

buildProject.py

```Python
import os
os.system("npm run build")
```

copyDistToMonolith.py

```Python
# This is in .gitignore to prevent conflicts
import os
import shutil
import fileinput, argparse, sys

indent = "        "
project_name = sys.argv[1]
path_to_react_project = "/js/appReact/" + project_name + "/"
#this should be updated with whatever your project path is
resource_line_template = "resource url:'" + path_to_react_project + "{0}'\n"
# Copy the files from dist to appReact folder in monolith project; update the path with your
path_to_monolith = "../monolith" #from the React App
full_path_to_react_project = path_to_monolith + "/" + path_to_react_project

def remove_old_files () :
  for the_file in os.listdir(full_path_to_react_project):
      file_path = os.path.join(full_path_to_react_project, the_file)
      try:
          if os.path.isfile(file_path):
              os.unlink(file_path)
      except Exception as e:
          print(e)

def copy_files_to_monolith () :
  files_to_replace = []
  for root, dirs, files in os.walk("./dist"):
    for file in files:
      path_file = os.path.join(root,file)
      if len(file.split(".")) == 3 :
        files_to_replace.append(file)
      shutil.copy2(path_file, full_path_to_react_project)

  # we don't need the index.html
  os.remove(full_path_to_react_project + "index.html")
  print(files_to_replace)
  return files_to_replace

def update_resources_file (files_to_replace) :
  count = 0
  for line in fileinput.input(path_to_monolith + "/monolith-app/conf/resourcesFile.java", inplace=True):
      if path_to_react_project in line:
        line = resource_line_template.format(files_to_replace[count])
        count += 1
      sys.stdout.write(line)

remove_old_files()
files_to_replace = copy_files_to_monolith()
update_resources_file(files_to_replace)
```

createPR.py

```Python
import os, sys, subprocess, re
import webbrowser

jira_ticket_id = sys.argv[1]

branch_short_desc = sys.argv[2] or ""

version = sys.argv[3] or "error in version"
project_name = sys.argv[4] or "error in project name"
branch_name = "feature_{0}_{1}".format(jira_ticket_id, branch_short_desc)

os.chdir("../monolith")
os.system("git checkout develop && git pull origin develop")
os.system("git checkout -b {}".format(branch_name))
os.system("git add monolith-app/conf/resourcesFile.java")
os.system("git add pathToReactChanges/appReact/{}/".format(sys.argv[4]))
os.system("git commit -m \"{0} - Deploy {1} {2} to monolith\"".format(jira_ticket_id, project_name, version))

batcmd = "git push origin " + branch_name
pr_url = re.search('(https:\/\/\S+)', str(subprocess.run([batcmd], shell = True, capture_output = True)))[0]
print(pr_url)

webbrowser.open(pr_url)
```

That's how we ended up from doing 5 to 10 minutes of work every time we wanted to test our React changes implemented in the monolith to only 10 seconds waiting the code to do the work for us.
###Conclusion

There are moments in Software Development of repetitive work that we do every day, take the risk to automate it, it'll take you more time than the repetitive work at first but it'll definitively pay off in the short term I can assure.

Feel free to ask me if you have any questions, and remember, any repetitive work most of the time worths the time to be automated.
