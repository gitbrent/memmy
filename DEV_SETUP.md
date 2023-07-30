# Development Environment Setup

## About

This guide is intended for MacOS developers looking to setup a lemmy development environemt suitable for contributing and testing.

## Requirements

- [MacOS](https://www.apple.com/macos/)
- [Xcode](https://developer.apple.com/xcode/)
- [node 18](https://nodejs.org/en/download) via [nvm](https://github.com/nvm-sh/nvm)
- [yarn 1](https://classic.yarnpkg.com/en/) (_classic, version 1_)
- [Homebrew](https://brew.sh)

## Clone Repo

If you intend to submit pull requests, fork the repo on github.com and open with github desktop or clone via shell command.

```shell
git clone https://github.com/memmy-app/memmy
```

## Dependencies

The build instructions for memmy are basically: have yarn install dependencies and setup the project with CocoaPods.

```shell
cd memmy
yarn install

# EITHER
cd ios
pod install

# OR
npx pod install
```

The shipped version of macOS (currently Ventura, 13.4) requires some addtional setup before the yarn and pod commands can be successfully executed.

### CocoaPods

[CocoaPods](https://cocoapods.org) is a well-known dependency manager for iOS projects and is used by memmy to manage the various application packages.

In order to install CocoaPods, we recommend using homebrew as it involves two steps, whereas using gem requires much more effort.

#### Install Using Homebrew

[Homebrew](https://brew.sh) is a package manager for macOS. It's widely used as it makes software easy to install and update.

Installation is perfromed using a single command (check the homebrew website as the commannd below may have changed).

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Next, run the [Homebrew cocoapods](https://formulae.brew.sh/formula/cocoapods) formulae.

```shell
brew install cocoapods
```

#### Install Using Gem

While macOS comes with the `gem` command and ruby pre-installed, ruby is not recent enough to pass dependency checks when installing cocoapods.

Additionally, it's not a good idea to update the local macOS ruby installation as other software may be linked to it. Meaning a separate installation of ruby will be required.

Install `chruby` and the latest ruby, setup your environment variables, then run `gem install cocoapods`.

### Node

We recommend using `nvm` to install and manage nodejs versions.

Currenly memmy targets node ">=18", so you'll need to install v18 if you do not already have it on your system.

```shell
brew install nvm
nvm install 18
nvm use 18
```

Install `yarn` using npm.

```shell
npm install --global yarn
```

## Yarn Install

Run yarn to install base nodejs packages.

```shell
yarn install
```

## Pod Install

The last step is to install the memmy pod. In this example, we'll be using the `pod install` method.

```shell
cd ios
pod install
```

### Pod Troubleshooting

If the installation process fails on the `glog` package similar to the following example, follow the instructions below.

```text
[...]
Installing frnt (6.2.1)
Installing glog (0.3.5)
[!] /bin/bash -c
set -e
#!/bin/bash

Copyright (c) Facebook, Inc. and its affiliates.
This source code is licensed under the MIT license found in the
LICENSE file in the root directory of this source tree.
set -e

PLATFORM_NAME="${PLATFORM_NAME:-iphoneos}"
CURRENT_ARCH="${CURRENT_ARCH}"

if [ -z "$CURRENT_ARCH" ] || [ "$CURRENT_ARCH" == "undefined_arch" ]; then
# Xcode 10 beta sets CURRENT_ARCH to "undefined_arch", this leads to incorrect linker arg.
# it's better to rely on platform name as fallback because architecture differs between simulator and device
```

This error is caused by an incorrect version of xcode a [GitHub issue](https://github.com/facebook/react-native/issues/25532#issuecomment-586969512) has the solution.

```shell
sudo xcode-select --switch /Applications/Xcode.app
```

Then restart/open a new terminal window and start Pod Install again.

## Run Memmy

Run the ios command to start the build process and open the iPhone simulator. (Note that the very first run can run for several minutes).

```shell
yarn ios
```

If successful, you should see the following metro screen.

```text
[user@myiMac 18:03:01] ~/GitHub/memmy/ios
=> yarn ios

                        ▒▒▓▓▓▓▒▒
                     ▒▓▓▓▒▒░░▒▒▓▓▓▒
                  ▒▓▓▓▓░░░▒▒▒▒░░░▓▓▓▓▒
                 ▓▓▒▒▒▓▓▓▓▓▓▓▓▓▓▓▓▒▒▒▓▓
                 ▓▓░░░░░▒▓▓▓▓▓▓▒░░░░░▓▓
                 ▓▓░░▓▓▒░░░▒▒░░░▒▓▒░░▓▓
                 ▓▓░░▓▓▓▓▓▒▒▒▒▓▓▓▓▒░░▓▓
                 ▓▓░░▓▓▓▓▓▓▓▓▓▓▓▓▓▒░░▓▓
                 ▓▓▒░░▒▒▓▓▓▓▓▓▓▓▒░░░▒▓▓
                  ▒▓▓▓▒░░░▒▓▓▒░░░▒▓▓▓▒
                     ▒▓▓▓▒░░░░▒▓▓▓▒
                        ▒▒▓▓▓▓▒▒


                Welcome to Metro v0.76.7
              Fast - Scalable - Integrated

r - reload the app
d - open developer menu
i - run on iOS
a - run on Android
```

Press "i" to open the iPhone simulator.

Code changes should be reflected in the app, if not, press ⌘-D to bring up the developer menu and choose "Refresh".

### Metro Troubleshooting

If the simulator window shows a white screen with the title "There was a problem loading the project" and details showing "Unhandled JS Exception: Property 'queueMicrotask' doesn't exist." try using the following command instead.

```shell
npx react-native start
```
