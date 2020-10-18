# An Introduction and Workshop to React Native

A quick word: This is an introductory workshop based on my personal experience of: (1) learning React from the web development training programme I underwent in Founders and Coders, (2) developing in React Native commercially for a few months, and (3) going through a few learning resources I've used when I initially started my React Native journey. I myself am new to React Native, but I will try to share as much insight as I've gained working with the framework. Feel free to contact me if you find any areas that need clarification or correcting. 

To proceed with this workshop you will need:
1. an understanding of React principles
2. a laptop to code with (and to test with if wanted)
3. a mobile device to test with

## What is React Native (vs React)?

React Native is an open-source mobile application framework created by Facebook, Inc. It is used to develop applications for Android, Android TV, iOS, macOS, tvOS, Web, Windows and UWP by enabling developers to use React along with native platform capabilities. (Wikipedia).

React Native uses the React JavaScript library to write code in through the React Native framework using specific components that then gets compiled into native code.

library vs framework: The technical difference between a framework and library lies in a term called inversion of control. When you use a library, you are in charge of the flow of the application. You are choosing when and where to call the library. When you use a framework, the framework is in charge of the flow. (FreeCodeCamp)

![](https://i.imgur.com/vyeKtFu.jpg)

## How we write React vs How we'd write with React Native vs How it gets compiled into its native code:

![](https://i.imgur.com/kD2O38u.png)

These components get compiled from React Native code to the native code that the app actually uses.

![](https://i.imgur.com/QmOx65K.png)

## What are my options for coding in React Native?

* Standalone - you'll need to dabble in the native code of Xcode and Android Studio but you'll have the full power of React Native at your disposal

* Expo - is a wrapper for React Native that handles all of the build processes for the native code. The benefits are:
    * provides you with the bundler for running the app in dev mode (the app finds Xcode and the Android Emulator for you)
    * provides you with On-The-Air updates
    * gives access to Expo libraries 
    
With one big limitation of: some libraries are incompatible with Expo. (You can always expo eject to change your app into Standalone).

For a list of compatibilities, check out what works and not with the tick box of of Expo in the directory: https://reactnative.directory/.

Note: Expo-based apps can still use other libraries not in the directory as long they're not specifc React Native libraries and instead are JS-only libraries (like moment). Another way to find out if a library is compatible or not is to check if it needs (react-native link) as part of its installation process. If it doesn't say so, then it works with Expo.

## Expo as a Start

To minimise complexity and maximise the learning opportunities of this workshop, we'll proceed with Expo so that the native code handling is taken away from us and we head directly towards app development.


If in doubt, React Native docs itself mentions that as a new starter to React Native, Expo-Cli will help to quick start the developer.

https://reactnative.dev/docs/0.60/getting-started

## Installation of Expo
In terminal:
```
npm install -g expo-cli
```

This bypasses the need to install react-native as expo will cover the installation of React-Native.

## Developing with Mobile Devices

Install Expo Client (from App Store or Play Store depending on your device)

## Developing with Laptops/Desktops

For Mac:

https://docs.expo.io/workflow/ios-simulator/

For Windows/Linux/Mac:

https://docs.expo.io/workflow/android-studio-emulator/

Take note: A Windows/Linux OS user will only be able to develop with the Android Studio Emulator unless they install a Virtual Mac or rent a Mac from the Cloud while a MacOS user will be able to develop in both because that user will have access to Xcode as well as the open-source Android Studio Emulator

## Working With Expo

To initialise app: In terminal, head to the directory you want to create your app in and type (changing project name to a name you'd like separated by dashes):
```
expo init project-name
```

To start metro bundler: Head to the root of the app through terminal and type:
```
expo start
```
To develop using your device for Android, head into the app and click the "Scan QR Code" and scan the QR Code in Metro Bundler

To develop using your device for iOS, go to your camera and Expo should run when it sees the QR Code in Metro Bundler

To develop using your laptop or desktop, you'll have to go through the installation process and install each one respectively, connect the simulator/emulator through the terminal, and run it through the Metro Bundler. For the purposes of this workshop, we'll stick with developing with devices.

## Explanation

![](https://i.imgur.com/34cpK6A.png)

Some common components:
* View - standard container for where other components live in
* ScrollView - standard container that extends the component for when the app needs more space
* Image - takes in source and styling (Does not render if missing height/width)
* Text - takes in text
* TouchableOpacity - acts as a view with an onPress that takes in an action
* TextInput - takes in a value usually used with useState that then changes depending on input

https://reactnative.dev/docs/components-and-apis

The StyleSheet is similar to React with a few key things:
* It starts out with Flexbox
* Styling usually stays in the same file as what it's styling either inline or within the StyleSheet

## Demo

* Place a Text component that says "Cat Facts"
* Style the Text component inline
* Place a Text component that says "Cat Facts 5"
* Style the Text component inline
* Place a Text component that says "Cat Facts 10"
* Style Text component inline
* Place an Image component that shows a cat below "Cat Facts"
* Make a function that gets Cat Facts 5 from the API: 
https://cat-fact.herokuapp.com/facts
* Use one of React's hooks to make the API call on load of the app
* Display it in a Text component below Cat Facts 5 using state.
* Make another function that gets Cat Facts 10.
* Use a Text component within a Touchable Opacity component to create an onPress Cat Facts 10 api call that displays the state pre- and post- api call.
* Style the Touchable Opacity component inline
* Move the styles to the dedicated StyleSheet on the bottom

## End Result: 

```javascript=
import React, { useEffect, useState } from 'react';
import { Image, StyleSheet, Text, TouchableOpacity, View } from 'react-native';

export default function App() {

  const [catFact5, setCatFact5] = useState("")
  const [catFact10, setCatFact10] = useState("")


  function getCatFactNumber5() { 
    fetch('https://cat-fact.herokuapp.com/facts')
    .then((res) => res.json())
    .then((result) => {
      setCatFact5(result.all[4].text)
    })
  }

  function getCatFactNumber10() { 
    fetch('https://cat-fact.herokuapp.com/facts')
    .then((res) => res.json())
    .then((result) => {
      setCatFact10(result.all[9].text)
    })
  }


  useEffect(() => {
    getCatFactNumber5()
  }, [])

  return (
    <View style={styles.container}>
      <Text style={styles.catFactTitle}>Cat Facts</Text>
        <Image
        style={{height: 200, width: 200}}
        source={{
          uri: 'https://images.unsplash.com/photo-1574144611937-0df059b5ef3e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1300&q=80',
        }}
      />
      <Text style={styles.catFactTitle}>Cat Fact 5</Text>
      <Text> {catFact5} </Text>

      <TouchableOpacity style={{
        backgroundColor: 'teal',
        borderWidth: 2,
      }} onPress={()=>{
        getCatFactNumber10()      
      }}>
        <Text style={styles.catFactTitle}>Cat Fact 10</Text>
      </TouchableOpacity>
      <Text>{catFact10}</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
  catFactTitle: {
    fontSize: 20,
    fontWeight: 'bold',
  },
});
```

## On Device/Emulator/Simulator

![](https://i.imgur.com/XXbzTaF.png){:height="50%" width="50%"}

## Project

Make your own 1 screen React Native App and discuss it via Expo that utilises:
* some of the various components above (View, Text, Touchable Opacity, Image)
* with StyleSheet styling
* that uses an API call (or more than 1) from a website of choice (test with Postman)
* uses React Hooks (usually state and useEffect) to create the app
* style accordingly taking note that the app starts with Flexbox and moves forward from there

If there's more time:
* Use Expo's library to add a video or audio to your app
* Use React Navigation to create more than one screen for your app
* Use React's Context hook in the app for state management

Check out the resources for React Native components, Expo, and React Navigation to learn more about React Native

## Resources

* React Native - https://reactnative.dev/docs/getting-started

* Expo - https://docs.expo.io/

* React Navigation - https://reactnavigation.org/

* React Native - https://en.wikipedia.org/wiki/React_Native

* FreeCodeCamp - https://www.freecodecamp.org/news/the-difference-between-a-framework-and-a-library-bd133054023f/#:~:text=The%20technical%20difference%20between%20a,in%20charge%20of%20the%20flow.

* Building React Native Apps — Expo or not? - https://medium.com/@adhithiravi/building-react-native-apps-expo-or-not-d49770d1f5b8

* Maximillan Schawrzmüller, Academind - https://www.youtube.com/watch?v=qSRrxpdMpVc
