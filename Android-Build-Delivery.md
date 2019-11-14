Build delivery is a tedious process, but using Fastlane we can make it much easier. Fastlane works on the `Lane`. 

### How to setup Fastlane
 * Fastlane init
 * Give the package name 
 * Provide the Google JSON file.
 * Add the lane as the requirement.

### Fastfile Configuration
There will be two flavor environment(developers and production) for project as given below.

``` flavorDimensions "default"
    productFlavors {
        production {
            buildConfigField 'String', 'BASE_URL', '"http://production.com"'
        }
        developers {
            buildConfigField 'String', 'BASE_URL', '"http://developer.com"'
        }
    }
```

  ### Fastlane lane to deliver build on developer account. 
  Add productFlavor name as postfix in gradle task.

```
    desc "Upload a new development Build to Hockey App"
        lane :developers do
           gradle(task: 'assembledevelopers', build_type: 'Debug')

           value = get_version_name(
                   #app_folder_name:"project"
                   gradle_file_path:"app/build.gradle",
                   ext_constant_name:"versionName"
           )
           puts "VALUE = #{value}"

            badge(glob: "/app/src/main/res/mipmap-*dpi/ic_launcher.{png,PNG}",
           shield: "Version-#{value}-blue",
           no_badge: true)

           hockey(
              api_token: '8f6c93966a104a25b2ed74c417f89172',
              notes_type: '0',
              notes: changelog_from_git_commits(commits_count: '100',
                                                pretty: '___ %h %s %n')
           )
     end
```

To deliver the build using fastlane go to the project path and run `Fastlane developers`


#### Script to deliver build on developer account and create tag on `Git`

    #!/bin/bash

    # file path 
    f='./app/build.gradle'

    v=`grep 'versionName' $f`

    # get versionName value from app/build.gradle file
    appversionname=v$(echo `grep 'versionName' $f` | grep -Eo '[0-9]{1,4}.[0-9]{1,4}.[0-9]{1,4}')
    echo "Version number is "$appversionname

    # checking the appversionname exist as tag. if exist return the versiontag else return null
    VERSIONTAG="$(git tag -l $appversionname)"

    # check the VERSIONTAG and appversionname are same then stop script else create tag, push and upload build on hockeyapp.
    if [ "$VERSIONTAG" == "$appversionname" ]
    then 
    echo "An error occurred. Tag already exist. Please update versionName to create a new tag. Exiting..."

    #finish the script forcly
    exit 1

    else
    # create tag
    git tag $appversionname

    # push the tag on remote
    git push origin $appversionname

    # upload the build on hockey app
    fastlane developers
    echo "Successfully uploaded on hockey app."

    fi

Store this file with name build.sh on the project root directory, to deliver developers build using on the command ./build.sh

### Fastlane lane to deliver build on production account. 
    desc "Upload a new production Build to Hockey App"
        lane :production do
           gradle(task: 'assembleproduction', build_type: 'Debug')

           value = get_version_name(
                   #app_folder_name:"project"
                   gradle_file_path:"app/build.gradle",
                   ext_constant_name:"versionName"
           )
           puts "VALUE = #{value}"

            badge(glob: "/app/src/main/res/mipmap-*dpi/ic_launcher.{png,PNG}",
           shield: "Version-#{value}-blue",
           no_badge: true)

           hockey(
              api_token: '8f6c93966a104a25b2ed74c417f89172',
              notes_type: '0',
              notes: changelog_from_git_commits(commits_count: '100',
                                                pretty: '___ %h %s %n')
           )
     end
 
To deliver the build using fastlane go to the project path and run `Fastlane production`

#### Script to deliver build on production account and create tag on `Git`

    #!/bin/bash

    # file path 
    f='./app/build.gradle'

    v=`grep 'versionName' $f`

    # get versionName value from app/build.gradle file
    appversionname=v$(echo `grep 'versionName' $f` | grep -Eo '[0-9]{1,4}.[0-9]{1,4}.[0-9]{1,4}')
    echo "Version number is "$appversionname

    # checking the appversionname exist as tag. if exist return the versiontag else return null
    VERSIONTAG="$(git tag -l $appversionname)"

    # check the VERSIONTAG and appversionname are same then stop script else create tag, push and upload build on hockeyapp.
    if [ "$VERSIONTAG" == "$appversionname" ]
    then 
    echo "An error occurred. Tag already exist. Please update versionName to create a new tag. Exiting..."

    #finish the script forcly
    exit 1

    else
    # create tag
    git tag $appversionname

    # push the tag on remote
    git push origin $appversionname

    # upload the build on hockey app
    fastlane production
    echo "Successfully uploaded on hockey app."

    fi

Store this file with name production_build.sh on the project root directory, to deliver production build using on the command ./production_build.sh


You can create multiple lane and script file(to deliver the build on different hockey app). Copy the lane and change the lane name and hockey app API key. Create a new script file and  just change the fastlane command.

