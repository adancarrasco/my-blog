---
title: Setup Firebase in React Native in 2020
date: "2020-01-26T23:05:03.284Z"
description: "How to setup Firebase Database in React Native in 2020"
---

This weekend I spent some time configuring a new React Native project to work with [Firebase](https://firebase.google.com/).

### What is Firebase?

Firebase is a Google service which provides some services such as:

- Hosting
- Real Time Database
- Authentication
- Cloud Functions
- AB Testing
- Etc

This service is for free with some limitations, after the limitations you pay for what you use. However, this is really good for starting projects and pitch new ideas, one big advantaje is that you don't need to write a single line for you BackEnd, unless you want to use the Google Cloud Functions but in this case, this is Serverless.

For the project that I was working on I needed a Database online where I could read some data, in this case I didn't need to update any data (for now), so I decided to use Firebase, I have used it before and the experience that I had was really good. I really enjoyed working with Firebase.

First of all you need to link your Google Account to your Firebase Account. After that you just create a project. You can follow up the steps [here](https://youtu.be/iosNuIdQoy8?list=PLl-K7zZEsYLmOF_07IayrTntevxtbUxDL)

Once you have your account make sure you select the _Real Time Database_ and start creating a dummy JSON, otherwise you won't be able to get the data with the code samples that I will share later.

In this example I have a collection of users, in this case we only need one user to see how the data will be stored.

```JSON
"users": {
  "hashId": {
    "birthdate": 12343255324,
    "deviceTokens": {
      "hashId": "jfskdl"
    },
    "email": "adan.carrasco@email.com",
    "facebookId": 123423123,
    "name": "Adan",
    "registerDate": 12343245234
  }
}
```

### React Native

Now we have everything setup in the Firebase side, we go to the [React Native](https://facebook.github.io/react-native/docs/getting-started) getting started Site. I recommend you to follow the steps from `react-native cli`, it's a walk through to setup what you need in an easy way.

### Common issues I had (follow this list before going to install Firebase dependencies)

1. Use `npm` instead of `yarn`
2. Forget to run `yarn start` before doing `react-native run-ios`
3. Forget to run `pod install` in the `ios` folder before `react-native run-ios`
4. Forget to install all the requirements for Android before running `react-native run-android`. The requirements are in the getting started page.
5. Install and go ahead adding packages for `iOS` when `Android` hasn't started (making parallel progress makes the life easier :) )
6. Forget to update my Android Studio before trying anything with `Android`

Now we are good to go an install the Firebase dependencies.

### Installing Firebase dependencies

It's as simple as going to the [React Native Firebase package](https://invertase.io/oss/react-native-firebase/quick-start/ios-firebase-credentials) maintined by Invertase. For iOS is really easy you just need to follow the steps. Don't forget to copy the package from the project settings.

Now for Android is a tricky one, you follow the steps described in the [Website](https://invertase.io/oss/react-native-firebase/quick-start/android-firebase-credentials). However you need to copy the package name from your Android project opening Android Studio to generate the file correctly, otherwise it will throw errors. Now that you have done that you'll have some errors because the Gradle project is version 4.X and you need the 5.X. You can follow this [Stackoverflow](https://stackoverflow.com/questions/17727645/how-to-update-gradle-in-android-studio) thread.

Now that you are using Gradle 5 you need to update the `gradle.build` configuration, following this [Stackoverflow thread](https://stackoverflow.com/questions/53709282/cannot-add-task-wrapper-as-a-task-with-that-name-already-exists).

Now you are good to go, your Application should be running in both OSs.

If you have any issues you can ask me directly on [Twitter](https://twitter.com/adancarrasco), I will be happy to help!

Thanks for reading!
