# react-native-webpack-starter-kit

[![Build Status](https://travis-ci.org/jhabdas/react-native-webpack-starter-kit.svg?branch=master)](https://travis-ci.org/jhabdas/react-native-webpack-starter-kit)
[![Dependency Status](https://david-dm.org/jhabdas/react-native-webpack-starter-kit.svg)](https://david-dm.org/jhabdas/react-native-webpack-starter-kit)
[![devDependency Status](https://david-dm.org/jhabdas/react-native-webpack-starter-kit/dev-status.svg)](https://david-dm.org/jhabdas/react-native-webpack-starter-kit#info=devDependencies)
[![Gitter](https://img.shields.io/gitter/room/nwjs/nw.js.svg)](https://gitter.im/jhabdas/react-native-webpack-starter-kit)

A forward-looking approach to building with React Native.

![React Native Webpack Starter Kit](https://dl.dropboxusercontent.com/u/10150480/rn-starter-kit-hero-wordswag.jpg)

Takes a minimalistic lean on tooling. Follows the latest React Native stable release. Uses Babel 6 for ES6-style JSX transpilation with ES7 Stage 1 support, and Webpack as a dev server and module bundler. Provides static code linting using ESLint and build output in the same console window, and Source Maps for debugging in the browser.

Leverages [`react-native-webpack-server`](https://github.com/mjohnston/react-native-webpack-server). Incorporates sane default linting rules. Uses [Greenkeeper](https://github.com/greenkeeperio/greenkeeper) to help keep dependencies fresh. Unprescriptive in terms of test frameworks and Flux implementations. Use with [EditorConfig](http://editorconfig.org/) to help code consistency between editors. Try with [`webpack-notifier`](https://github.com/Turbo87/webpack-notifier) for desktop notifications on OS X.

For an example implementation take a look at [Lumpen Radio](https://github.com/jhabdas/lumpen-radio). Or check out a few other [Awesome React Boilerplates](http://habd.as/awesome-react-boilerplates/#react-native).

## Requirements

- [Node](https://nodejs.org) 4.x or better
- [React Native](http://facebook.github.io/react-native/docs/getting-started.html) for development
- [Xcode](https://developer.apple.com/xcode/) for iOS development (optional)
- [Android SDK](https://developer.android.com/sdk/) for Android development (optional)
- [Android Lollipop](https://www.android.com/versions/lollipop-5-0/) or better for Android device testing (optional)

## Stack

- [React Native](http://facebook.github.io/react-native/) for native app development
- [Babel](http://babeljs.io/) for ES6+ support
- [Webpack](https://webpack.github.io/) module loader and bundler
- [Docker](https://www.docker.com/) and [VirtualBox](https://www.virtualbox.org) for Windows-based development

## Installation

OS X users start by cloning this repo and installing dependencies once your [environment is set-up](https://facebook.github.io/react-native/docs/getting-started.html):

```sh
git clone https://github.com/jhabdas/react-native-webpack-starter-kit.git native-starter-kit && cd $_
npm i
```

The official React Native [Getting Started documentation](https://facebook.github.io/react-native/docs/getting-started.html) suggests OS X is required for development. However, I've created a custom virual environment for Windows users to get in on the fun too. See [Using with Docker](#using-with-docker) section for setup instructions.

## Running

Once dependencies are installed, run the starter kit with:

```sh
npm start
```

This will start the React Packager and a [Webpack Dev Server](https://github.com/webpack/webpack-dev-server). The dev server will watch your JS files for changes and automatically lint and generate the `index.[platform].js` files expected by your React Native iOS or Android app.

### iOS

Open `ios/App.xcodeproj` in Xcode, build and run the project.

Unlike the app currently generated by `react-native init` this app removes the `UIViewControllerBasedStatusBarAppearance` key to prevent the app from logging an error in Xcode and leading to an App Store rejection. The key may be added back, if desired, but its value must be set to `true` to prevent unexpected rejection during the review process.

### Android

For android development use the following:

```sh
npm run android-setup-port # Note that this option is available on devices running android 5.0+ (API 21)
react-native run-android
```

Note Android support in React Native is relatively new, so expect some hiccups. Please see the official [Android Setup](http://facebook.github.io/react-native/docs/android-setup.html#content) documentation for getting set-up and the [`README` in RNWS](https://github.com/mjohnston/react-native-webpack-server/blob/27218e70f655983aff99a8a31d258eda5254aaf6/README.md) for additional information. And here's some [helpful npm scripts](https://github.com/mjohnston/react-native-webpack-server/issues/65#issuecomment-151222398) courtesy of [@niftylettuce](https://github.com/niftylettuce).

If you run into any issues please see the [Getting Started](http://facebook.github.io/react-native/docs/getting-started.html) guide for React Native before submitting an issue.

## Testing

As a minimalist seed this project does not introduce a testing framework. Instead it leverages simple static code analysis to help prevent coding mistakes and introduce some good practices for building React Native apps with ES6 and ES7.

Webpack is configured with a pre-loader to lint the application with ESLint using the Babel parser during app development. And the `npm test` command may be run at anytime to lint source code otherwise. See the `.eslintrc` file to adjust [linter rules](http://eslint.org/docs/rules/) to your liking.

## Bundling

Building the app for distribution.

1. Execute `npm run bundle` to generate the [offline JS bundle](https://facebook.github.io/react-native/docs/running-on-device-ios.html#using-offline-bundle).
1. For iOS, update `AppDelegate.m` to load from pre-bundled file on disk.
1. Test the application, create an archive and submit to the store.

## Submitting to Store

Please see [Submitting to App Store](http://habd.as/reflecting-on-react-native-development/#submitting-to-app-store) for instructions on iOS. If you have any good Android instructions, please send a PR this way. Good luck and have fun out there!

## Using with Docker

Windows users may experience problems with React Native development. This kit includes a `Dockerfile` which can be used to create a virtualized development environment for building your app on a Windows machine. To use it [set-up Docker Machine](https://docs.docker.com/machine/get-started/) then run the following commands with [cmder](http://cmder.net/) (or similar) to get going:

1. Clone this repo then update `.watchmanconfig` to the following: `{"ignore_dirs": ["node_modules"]}`.
1. Run `docker build --rm .` command from the project root directory to build a virtualized Linunx environment configured for development using this starter kit.
1. Get the ID of the built Docker image by running `docker images` and looking for the most recently created image.
1. Then shell into the box with `docker run -it 09608e4ec865 /bin/bash` (where `09608e4ec865` is the Image ID) and run the app with `npm start`.

Support should be considered _experimental_, though I'm planning to develop a workflow allowing development on CoreOS running on a [Raspberry Pi](https://www.raspberrypi.org).

If iOS and Android device emulators are not available for your development environment (anything except OS X, basically) consider shipping code directly to a native device using [Exponent](https://exponentjs.com/).
