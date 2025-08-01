# _fastlane_ Plugin for Ionic CLI

[![fastlane Plugin Badge](https://rawcdn.githack.com/fastlane/fastlane/master/fastlane/assets/plugin-badge.svg)](https://rubygems.org/gems/fastlane-plugin-ionic) [![License](https://img.shields.io/badge/license-MIT-green.svg?style=flat)](https://github.com/ionic-zone/fastlane-plugin-ionic/blob/master/LICENSE)
[![Gem](https://img.shields.io/gem/v/fastlane-plugin-ionic.svg?style=flat)](http://rubygems.org/gems/fastlane-plugin-ionic)

This _fastlane_ plugin helps you build your **Ionic Capacitor** project via the [`ionic` CLI](https://ionicframework.com/docs/cli/).

It is based on [fastlane-plugin-cordova](https://github.com/bamlab/fastlane-plugin-cordova) (where it borrows a lot of its code. Thanks!).
Modifed to work with capacitor

## Getting Started

This project is a [fastlane](https://github.com/fastlane/fastlane) plugin. To get started with `fastlane-plugin-ionic`, add it to your project by running:

```bash
fastlane add_plugin ionic
```

## Actions

### `ionic`

Runs `ionic capacitor build` (technically: `ionic capacitor prepare` first, then `ionic capacitor compile` [which is the same as what `build` does internally]) to build your Ionic project.

```ruby
ionic(
  platform: 'ios', # Build your iOS Ionic project
)
ionic(
  platform: 'android', # Build your Android Ionic project
  release: false # Build a "Debug" app
)
```


## Examples

Lanes using these actions could look like this:

```ruby
platform :ios do
  desc "Deploy ios app on the appstore"

  lane :deploy do
    match(type: "appstore")
    ionic(platform: 'ios')
    deliver(ipa: ENV['CAPACITOR_IOS_RELEASE_BUILD_PATH'])
  end
end

platform :android do
  desc "Deploy android app on play store"

  lane :deploy do
    ionic(
      platform: 'android',
      keystore_path: './prod.keystore',
      keystore_alias: 'prod',
      keystore_password: 'password'
    )
    supply(apk: ENV['CAPACITOR_ANDROID_RELEASE_BUILD_PATH'])
  end
end
```

with an `Appfile` such as

```ruby
app_identifier "com.awesome.app"
apple_id "apple@id.com"
team_id "28323HT"
```

---

The `ENV['CAPACITOR_ANDROID_RELEASE_BUILD_PATH']` is only valid for `capacitor-android` 7.x and newer (which you should be using anyway!).

If you're using **Crosswalk** (which oyu should not really be doing anymore), replace `supply(apk: ENV['CAPACITOR_ANDROID_RELEASE_BUILD_PATH'])` (and equivalents) by:

```ruby
supply(
  apk_paths: [
   'platforms/android/build/outputs/apk/android-armv7-release.apk',
   'platforms/android/build/outputs/apk/android-x86-release.apk'
  ],
)
```

## Plugin API

To check what's available in the plugin, install it in a project and run at the root of the project:

```
fastlane actions ionic
```

Which will produce:

| Key | Description | Env Var | Default |
|-----|-------------|---------|---------|
| **platform**             | Platform to build on. <br>Should be either android or ios | CAPACITOR_PLATFORM            |        |
| **release**              | Build for release if true,<br>or for debug if false       | CAPACITOR_RELEASE             | *true* |
| **device**               | Build for device                                          | CAPACITOR_DEVICE              | *true* |
| **prod**                 | Build for production                                      | IONIC_PROD                  | *false* |
| **type**                 | This will determine what type of build is generated by Xcode. <br>Valid options are development, enterprise, adhoc, and appstore| CAPACITOR_IOS_PACKAGE_TYPE    | appstore |
| **verbose**              | Pipe out more verbose output to the shell | CAPACITOR_VERBOSE    | false |
| **team_id**              | The development team (Team ID) to use for code signing  | CAPACITOR_IOS_TEAM_ID               | *28323HT* |
| **provisioning_profile** | GUID of the provisioning profile to be used for signing | CAPACITOR_IOS_PROVISIONING_PROFILE  |           |
| **android_package_type**         | This will determine what type of Android build is generated. Valid options are `apk` and `bundle` | CAPACITOR_ANDROID_PACKAGE_TYPE    | apk |
| **keystore_path**        | Path to the Keystore for Android                        | CAPACITOR_ANDROID_KEYSTORE_PATH     |           |
| **keystore_password**    | Android Keystore password                               | CAPACITOR_ANDROID_KEYSTORE_PASSWORD |           |
| **key_password**         | Android Key password (default is keystore password)     | CAPACITOR_ANDROID_KEY_PASSWORD      |           |
| **keystore_alias**       | Android Keystore alias                                  | CAPACITOR_ANDROID_KEYSTORE_ALIAS    |           |
| **build_number**         | Sets the build number for iOS and  version code for Android   | CAPACITOR_BUILD_NUMBER              |           |
| **browserify**           | Specifies whether to browserify build or not            | CAPACITOR_BROWSERIFY                |  *false*  |
| **capacitor_prepare**      | Specifies whether to run `ionic capacitor prepare` before building  | CAPACITOR_PREPARE               |  *true*   |
| **min_sdk_version**      | Overrides the value of minSdkVersion set in `AndroidManifest.xml` | CAPACITOR_ANDROID_MIN_SDK_VERSION   | ''       |
| **capacitor_no_fetch**      | Specifies whether to run `ionic capacitor platform add` with `--nofetch` parameter  | CAPACITOR_NO_FETCH               |  *false*   |
| **capacitor_no_resources** | Specifies whether to run `ionic capacitor platform add` with `--no-resources` parameter  | CAPACITOR_NO_RESOURCES               |  *false*   |
| **build_flag**           | An array of Xcode buildFlag. Will be appended on compile command.  | CAPACITOR_IOS_BUILD_FLAG               | [] |
| **capacitor_build_config_file**      | Call `ionic capacitor compile` with `--buildConfig=<ConfigFile>` to specify build config file path  | CAPACITOR_BUILD_CONFIG_FILE               |     |


## Run tests for this plugin

To run both the tests, and code style validation, run

```
rake
```

To automatically fix many of the styling issues, use
```
rubocop -a
```

## Issues and Feedback

For any other issues and feedback about this plugin, please submit it to this repository.

## Troubleshooting

If you have trouble using plugins, check out the [Plugins Troubleshooting](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/PluginsTroubleshooting.md) doc in the main `fastlane` repo.

## Using `fastlane` Plugins

For more information about how the `fastlane` plugin system works, check out the [Plugins documentation](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Plugins.md).

## About `fastlane`

`fastlane` is the easiest way to automate beta deployments and releases for your iOS and Android apps. To learn more, check out [fastlane.tools](https://fastlane.tools).
