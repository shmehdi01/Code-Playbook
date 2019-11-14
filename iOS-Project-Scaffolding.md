
# Project scaffolding guideline

Before starting the application it is very important to set up your application.

# Project Architecture

We follow MVC based architecture. Our project architecture is focused on simplicity and consistency.

Let's see the structure deeply below : 

 
https://github.com/CrownStack/crownstack-playbook/blob/master/iOSProjectStructure.png

**Remember** : We are not making a separate 'View', instead all the belonging views and related view controllers are in the **Controllers** section just to avoid complexity. Suppose if you get a bug and need to resolve it, then it will become easy if you find all the details in one group rather than of scrolling from one group to another.

# Pre Requirement

1. Must follow the above project structure.

2. Must configure your project with [swiftlint](https://github.com/realm/SwiftLint) for regular check of code styling and formatting.

3. Must add `crashlytics` by default and use the email id `developer@crownstack.com` (contact your lead for more information).

4. Must install [fastlane](https://crownstack.github.io/blog/2017/11/06/fastlane.html) and match your repository with proper certificates to deliver build.

5. Initialize your project with pod. And get our sugar functions by using `pod ‘CrownStackSugarFunctions’`.

6. Must add AppConfiguration file to your project and write all the configuration related information like crashlytics key, app url etc.


# Some basic guidelines 

*  Never use image name(or assets) directly. For that we have a separate extension on our sugar functions.

   `Usage : ProjectName.Image.imageName`

* Never use font and its size directly. Instead we have a separate extension for both font and its size.

  `Usage : ProjectName.medium(size: ProjectName.FontSize.large)`

* Your project must not contain any warnings. Remember to follow the swiftlint.

* Must **not forcefully unwrap** the optional value.

* Do not use custom navigation bar unless and until its a exceptional case because in iphone X navigation bar size get increased, so please beware.

* Always use the size classes for iPad compatibility or even for iPhones.

* While adding any image to your assets must compress it using https://tinypng.com/ , it will reduce your app size.

* For app icon use [myappicon](https://makeappicon.com/).

**Note** : Incase you need to debug your project on device, then you can download debugging profile from [here](https://github.com/CrownStack/CrownStackTestingProfile). In case of personal device, do remember to add your device uuid to the the profile, for this you can contact your manager.


