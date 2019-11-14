We use Testflight to deliver build via `fastlane`.

`fastlane` is a tool for developers that can do following tedious tasks with simple commands.

1. Generating screenshots.
2. Dealing with provisioning profiles.
3. Releasing application.

**Note** : 1. Please create your app on itunes and register your app id on developer account.

 2. While fastlane setup use this information, For `fastlane match` use our [repository](https://github.com/CrownStack/matchCrownstack) to store certificates and provisioning profiles.
Use the passphrase : `Home@123` (we always use this passphrase)

[How to setup fastlane](https://github.com/CrownStack/CrownStack.github.io/blob/b54f7457c2863390e91302d4f38b506ec4b53ef5/_posts/2017-11-06-fastlane.markdown)

I assume that you have setup the fastlane for your project. 

Now we want to release build for different purpose like for testing or for client review. While initializing you must have configured fastlane for one account(suppose for client). So for testing we need to create different target for your project.

## How to create different target.

Open your project from Xcode -> Select your target -> Right click on it -> Select `Duplicate`. It will create a copy for your target.

#### Rename target and enable shared

Now goto scheme from the top left of the xcode near the simulator -> Click on `Manage Scheme` -> Select your target -> Press enter and rename it(like YourAppNameTester) -> Enable the shared checkbox.

#### Configure new target with Pod

   Pod is dependent on the targets. As we created a different target we need to configure our Pod with our new 
   target. Please check my pod file example below : -

    # Uncomment the next line to define a global platform for your project
    # platform :ios, '9.0'

    # Comment the next line if you're not using Swift and don't want to use dynamic frameworks
    use_frameworks!

    def testing_pods
    pod 'Alamofire'
    pod 'SwiftyJSON'
    pod 'AlamofireNetworkActivityIndicator'
    pod 'SDWebImage'
    end

    target 'YourAppMainTarget' do
    testing_pods
    end

    target 'YourAppMainTargetTester' do
    testing_pods
    end

**Note** : Before we proceed, let me clear we need to create a different app on itunes to release build to tester. Each app needs a different app id. So we need to create a different app id from developers account.

**Why we created a different app on itunes** : Answer is simple because we want to release build to different testers without affecting the client.

## Fastlane script to deliver build to client

**Note** : Install [fastlane plugin](https://github.com/HazAT/fastlane-plugin-badge).

    desc "Submit a new Beta Build to Apple TestFlight"
    desc "This will also make sure the profile is up to date"
    lane :beta do
    match(type: "appstore") # more information: https://codesigning.guide
    get_build_number
    increment_build_number
    add_badge(shield:get_version_number + “-” + get_build_number + “-” + “-blue” + git_branch, 
    shield_no_resize:true)
    gym(scheme: "YourAppMainTarget") # Build your app - more options available
    pilot
    # sh "your_script.sh"
    # You can also use other beta testing services here (run `fastlane actions`)
    end

To deliver build, navigate to your project and run `fastlane beta`.

## Fastlane script to deliver build to tester

    lane :tester do
    match(type: "appstore", app_identifier: "com.crownstack.testers") # more information: https://codesigning.guide
    get_build_number
    increment_build_number
    add_badge(shield:get_version_number + “-” + get_build_number + “-” + “-blue” + git_branch, 
    shield_no_resize:true)
    gym(scheme: "YourAppNameTester") # Build your app - more options available
    pilot(app_identifier: "com.crownstack.testers")
    add_git_tag(
    tag: "my_custom_tag"
    )
    # sh "your_script.sh"
    # You can also use other beta testing services here (run `fastlane actions`)
    end

To deliver build to testers, navigate to your project and run `fastlane tester`.

**Note** : While `fastlane init` for your project, fastlane creates appfile and deliver file which contains `app_identifier, username, team_id and apple_id`. So for our tester release we only need to change the `app_identifier` at the run time. So we had to change the script and provide the correct identifier.


# For Fastlane Increment Build Number

 Goto your project target -> Build Settings -> Search for `Versioning` -> Set current project version with your current version and versioning system to `Apple Generic`.

# For Fastlane Badge

You need gem file, if you are setting the project with `fastlane` first time, then you need to follow the steps below :  
1. Run `bundle init` on your working directory from terminal.
2. Then run `fastlane add_plugin badge`. 
3. Run `brew install imagemagick`.
4. Run `brew install graphicsmagick`.

**Note** : If someone from your team has been already init a gem file then skip the Step 1.

# For Fastlane git branch

1. Run `brew install librsvg` in your terminal.
2. Run `fastlane action git_branch` in your terminal.
