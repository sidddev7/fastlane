## User Session Tracking tools : -

- [AWS](https://docs.amplify.aws/lib/analytics/autotrack/q/platform/react-native/)
- [FullStory](https://www.fullstory.com/)
- [Amplitude](https://amplitude.com/)
- [DataDog](https://www.datadoghq.com/)
- [MouseFlow](https://chartbeat.com/)
- [UxCam](https://uxcam.com/)
- [Posthog](https://posthog.com/)

## Fastlane Setup in android and IOS, react native application

### Installing Fastlane in Windows machine

Install Ruby: Fastlane requires Ruby to run. Download and install the latest Ruby+Devkit version from the RubyInstaller website [Rubyinstaller](https://rubyinstaller.org/downloads/). Choose the version that matches your system architecture (x64 or x86).

During the Ruby installation, make sure to check the option "Add Ruby executables to your PATH" to enable running Ruby commands from the command prompt.

Close and reopen your command prompt or terminal to ensure the changes to the PATH environment variable take effect.

Install OpenSSL: Fastlane requires OpenSSL to handle code signing. Download the OpenSSL binary distribution from the OpenSSL website [OpenSSL](https://slproweb.com/products/Win32OpenSSL.html). Choose the version that matches your Ruby installation (either x64 or x86).

During the OpenSSL installation, select the destination directory (e.g., C:\OpenSSL) and ensure that the "The OpenSSL binaries (/bin) directory" is added to your system PATH.

Install Fastlane: Open your command prompt or terminal and run the following command to install Fastlane:

`gem install fastlane -NV`

The -NV options will prevent the installation process from attempting to install documentation or generating RDoc.

### Installing Fastlane in Mac OS

- Run this command in your terminal
  `sudo gem install fastlane -NV`

### Setup fastlane files

- run `fastlane init` on the root directory of your project
- if it does not work, run the same command inside the ios folder
- Move the generated folder to root directory
- [Link](https://carloscuesta.me/blog/shipping-react-native-apps-with-fastlane#getting-started)

- here is an example of the Fastfile that can be used

```javascript
fastlane_version '2.213.0'
platform :ios do
    # iOS Lanes
    lane :debug do   # Can run using `fastlane ios debug`
        increment_build_number
        build_app
        upload_to_testflight
    end
    lane :release do # Can run using `fastlane ios release`
        increment_build_number
        build_app
        upload_to_app_store
    end
 end

 platform :android do
   # Android Lanes
   lane :debug do # Can run using `fastlane android debug`
    increment_build_number
    build_app
    upload_to_testflight
    end
    lane :release do # Can run using `fastlane android release`
        increment_build_number
        build_app
        upload_to_app_store
    end
 end

```

### We can add multiple commands in the Fastfile that can be used afterwards

## Android

- Sign the Application using this command

  `keytool -genkeypair -v -storetype PKCS12 -keystore my-upload-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000`

- Now Follow [this](https://shift.infinite.red/simple-react-native-android-releases-319dc5e29605) guide to obtain the json api key from Google Play store services
- Test the json key validity with

  `fastlane run validate_play_store_json_key json_key:/path/to/your/downloaded/file.json`

- If that works, add the following code to Appfile : -
  ```
  json_key_file("path/to/your/play-store-credentials.json")
  package_name("my.package.name")
  ```
- ### Configure Supply

- `Supply is used for syncing the screenshots and other binaries to app stores`
- Edit Appfile and change the json_key_file link : -

  `json_key_file "/path/to/your/downloaded/key.json"`

- Now run `fastlane supply init` if your app has been created on Google play store console, so that supply can start managing it

- For Grabbing the screenshots follow this documentation [Grab screenshots for android](https://docs.fastlane.tools/getting-started/android/screenshots/)

- Deploying to Beta distribution service - Building your app

  ```javascript
  lane :beta do

        # Adjust the `build_type` and `flavor` params as needed to build the right APK for your setup

        gradle(
        task: 'assemble',
        build_type: 'Release'
        flavor: 'Release'
        )

        # ...

  end
  ```

> :bulb: **Tip:** you can get commands for specific actions from fastlane itself

Example: -

`fastlane actions gradle `

`fastlane actions upload_to_play_store`

## IOS
