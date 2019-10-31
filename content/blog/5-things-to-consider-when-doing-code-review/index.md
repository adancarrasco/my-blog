---
title: 5 things to consider when doing code review
date: "2019-10-31T10:35:03.284Z"
description: "What to consider when doing code review"
---

When you working in a project you should have the best practice of creating Pull Requests, if you are a Junior developer you start receiving some feedback and after some time you start doing code review to your peers and later to other Teams members. In this article we are going to review 5 things that we should consider while doing Code Reviews.

### The structure of the Pull Request

As a complement of the code, a PR should have some descriptive text about what the code is intended to do, this can be either fix a bug, a feature, part of a feature etc. So, how can you do that or look at it? There are many ways, some of them are having templates like for example:

#### Steps to reproduce:

- Go to localhost
- Login as user
- Open the User profile
- Click upload a photo
- Upload a photo

#### Where to look in the code:

I modified the UploadPhoto component adding the hability to upload photos asynchronously

#### Relevant tickets:

Here we can put the JIRA ticket. Some times the JIRA ticket has more information related to the feature/bug/improvement, but if not we should pay attention that the PR has that information

#### Screenshots:

An screenshot of before and after is always helpful if our change is implementing a new feature or also fixing a bug.

#### Checklist:

- [ ] I added the necessary documentation
- [ ] I added tests to prove that my fix is effective or my feature works.
- [ ] Etc

### Complexity

If you see any code that is not being properly written, such as complex logic or big methods you should suggest a refactor. Sometimes is hard for Junior Developers to thing on how they can refactor the things; it's good to put some examples about what you are referring to either by putting some pices of the code you are suggesting to refactor or to point to any other places of the project where the pattern that you are suggesting is used.

### Tests

Does the changes for the feature/bug/partial feature include tests? If not, you should ask for them, if it does, are the tests good enough to test the described functionality and varations described in the JIRA ticket or description of the feature?

### Descriptive from the name to the style

#### Naming

Pay special attention to any naming, this can include, variables, methods, classes, etc. Remember that good named code is almost as good as good documented code, a code that explains by itself what is doing is the best code to be maintained.

#### Comments

If there's any complex logic including business rules is good to add a comment on why the decision to put those values, do those calculations. Most of the cases the code should be clear, short and good enough to explain the functionality by just reading it, however there are certain situations where we need to add a couple of lines describing any unclear decision.

#### Style

Does the code follow the style defined in the project? There are some rules regarding style that your project should follow, this is also to keep it consistent and clean. For example if you are doing JavaScript code, depending on your codebase you can choose either to use semi colons or not, so whatever decision you make within your team should be followed and applied to all the code in the project; this can also apply for spaces vs tabs when indenting code, also for curly braces, etc.

### Functionality

Once the code is in an acceptable level, is part of the Review, to should ensure that the feature/fix is actually working as expected. Depending on the organization can be the person doing the CR the one who tests or also can be a peer of the person who created the PR, the point is that someone (not the creator) should test the feature and validate that it's working as described.

This summarizes what you should look when reviewing Pull Request. Do you have anything else in mind? Are there any other points that you also consider? Feel free to contribute.

Thanks for reading!
