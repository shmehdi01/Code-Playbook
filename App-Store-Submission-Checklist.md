## User/Text Inputs 

- [ ] Text Fields are not hidden by keyboard on all screen sizes. 
- [ ] Text Selection (including disabled when appropriate)
- [ ] Enable/Disable Dictionary when appropriate 
- [ ] Return key name (Done/Next/Go) should match the context
- [ ] Show Default keyboard type when appropriate 
- [ ] Show Email keyboard type when appropriate 
- [ ] Show URL keyboard type when appropriate 
- [ ] Show Phone number keyboard type when appropriate 
- [ ] [Shake to undo or redo](https://developer.apple.com/ios/human-interface-guidelines/interaction/undo-and-redo/) (To be used when appropriate)

## API Network Connectivity. 
Handle following cases and show appropriate error messages depending on the context. Error messages if shown should be relevant and meaningful to the user. Please never show direct dump which only a developer can understand.

- [ ] If the user is not connected to the network.
- [ ] If API request fails and throws an error message. 
- [ ] If server timeout error occurs. 

## Location Services
Please handle following cases in location services. 

- [ ] Handle Accuracy 
- [ ] Cell tower location, True GPS, Wi-fi location
- [ ] When unable to find location
- [ ] Permissions
- [ ] NO results returned 
- [ ] Location services turned off
- [ ] Location services disabled for this App 
- [ ] background location services, if applicable

## Video/Audio 

- [ ] Should stop on receiving a call 
- [ ] Other actions on other interruptions
- [ ] Video/Audio recording runs out of space

## Authentication 

- [ ] Logging in with social apps
- [ ] New User
- [ ] Return User
- [ ] Forgot Password
- [ ] Terms of services and privay policy

## Settings 

- [ ] Company/Developer name 
- [ ] Company/Developer Website
- [ ] App version number
- [ ] Support/Contact information
- [ ] Credits/Attribution 
- [ ] Login Credentials if app requires Login, so that app can be reviewed

## Screen Sizes and Orientation 

- [ ] Double height status bar
- [ ] Orientation change if applicable
- [ ] Orientation Lock 
- [ ] Tested on 3.5-inch device (e.g., iPhone 4s)
- [ ] Tested on 4-inch device (e.g., iPhone 5)
- [ ] Tested on 4.7-inch device (e.g., iPhone 6s)
- [ ] Tested on 5.5-inch device (e.g., iPhone 6 Plus)
- [ ] Tested on iPad
- [ ] Tested on iPad mini
- [ ] Tested on iPad Air

## User interaction design standards

- [ ] Primary content fits the screen without zooming or scrolling 
- [ ] UI elements are designed for touch gesture
- [ ] Tappable elements measure at least 44x44 points
- [ ] Text is at least 11 points 
- [ ] There is enough contrast between the font and the background colours to make a test legible. 
- [ ] Text does not overlap
- [ ] All images have 2x and 3x versions 
- [ ] All images are sized at the right aspect ratio to avoid distortion 
- [ ] Controls are placed near the content they modify
- [ ] Permission requests are timed right and provide context 
- [ ] verify app meets [human interface standards](https://developer.apple.com/ios/human-interface-guidelines/interaction/3d-touch/) if using 3D touch

## App Store Listing

### iTunes Connect App Information

For other details, refer to the Apple's [iTunes Connect Properties](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Appendices/Properties.html).

- [ ] Primary Language		
- [ ] Bundle ID		
- [ ] Bundle ID Suffix		
- [ ] Apple ID		
- [ ] SKU Number		
- [ ] Name		
- [ ] Privacy Policy URL		
- [ ] Primary Category		
- [ ] Secondary Category *(optional)*		
- [ ] Subcategory *(optional)*		
- [ ] Pricing		
- [ ] Description		
- [ ] Keywords		
- [ ] Release Notes *(not for first version)*		
- [ ] Version Number		
- [ ] Version Release Settings		
- [ ] Support URL		
- [ ] Marketing URL *(optional)*		
- [ ] Copyright		
- [ ] Primary Contact Info		
- [ ] Routing App Coverage File *(optional)*		
- [ ] Custom License Agreement *(optional)*		
- [ ] Rating		
- [ ] Made for Kids *(optional)*		
- [ ] Demo Account Login		
- [ ] App File Size		
- [ ] User Roles		
- [ ] Notes		

## App Store Icon

- [ ] Pixel dimensions are 1024 x 1024		
- [ ] Smaller artwork is not scaled up		
- [ ] 72 dpi, RGB, flattened, no transparency, no rounded corners		
- [ ] High-quality JPEG or PNG		

## Screenshots

For other details, refer to Apple's [Screenshot Properties](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Appendices/Properties.html#//apple_ref/doc/uid/TP40011225-CH26-SW2).

- [ ] App Preview (optional)		
- [ ] At least one 5.5-inch Retina Display screenshot		
- [ ] (If for iPad) at least one 12.9-inch iPad screenshot		
- [ ] (If an iMessage iPhone app) at least one 5.5-inch screenshot		
- [ ] (If an iMessage iPad app) at least one 12.9-inch screenshot		
- [ ] (If for OS X) at least one Mac screenshot		
- [ ] (If for tvOS) at least one tvOS screenshot		
- [ ] (If for Apple Watch) at least one WatchKit extension screenshot		

## Don't Forget

Consider the following kinds of items as well.

- [ ] Code signing certificate / provisioning profile correct		
- [ ] Re-prompt permission requests if denied		
- [ ] [Push notifications](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html) especially for production app		
- [ ] [Local notifications](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)		
- [ ] [In-app purchases](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)	

- [ ] Production API Key/ID in place		
- [ ] dSYM uploading for production crash reporting		
- [ ] Check upgrade against live App Store version		
- [ ] Check upgrade against older, non-live App Store version		
- [ ] Considerations around migration of local database data, keychain information, user settings		
- [ ] Analytics tool is set up and tested to make sure it’s collecting data properly		
- [ ] Integrate SiriKit with your updated iOS 10 app (if applicable)		
- [ ] Custom widgets display proper content, are formatted correctly, and function as intended (if applicable)		

References:
[Savvy Apps](https://quip.com/FtjnAWlMMnJS)
