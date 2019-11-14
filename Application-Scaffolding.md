## Application scaffolding guideline

Before starting the application it is very important to set up your application.

### Application Credential

 Provide the unique applicationId. Try not to change applicationId in future. Define `minSdkVersion`, `targetSdkVersion` and `compileSdkVersion`. Define flavour in build.gradle(if needed).

### App Icons

  Provide app icon as guideline. you can check the link to follow gideline https://developer.android.com/guide/practices/ui_guidelines/icon_design_launcher.html

### Follow code style
    https://github.com/CrownStack/crownstack-playbook/wiki/android-Playbook

### Git guideline
    https://github.com/CrownStack/crownstack-playbook/wiki/Git-Usage-Guidelines

### Product Flavour
    flavorDimensions "default"
     productFlavors {
        developer {
            buildConfigField 'String', 'HOST_URL', '"http://dev.com"'
        }

        production {
            buildConfigField 'String', 'HOST_URL', '"http://production.com"'
        }
     }

 Replace `http://dev.com` and `http://production.com` URL with application development and production HOST_URL    respectively.

### Fastlane guideline

  * Fastlane setup
      https://docs.fastlane.tools/getting-started/android/setup/

  * Install plugins
      fastlane add_plugin get_version_name

  * Install `imagemagick`.

  * [Install badge pugin for fastlane](https://github.com/HazAT/badge) `sudo gem install badge`

  * fastlane init. Provide package name and json file path.

  * Create build and upload on hockey app
    fastlane hockey_build    

### Download build

  Go to HockeyApp Dashboard. Select app and you can check the build by version. Download build.
