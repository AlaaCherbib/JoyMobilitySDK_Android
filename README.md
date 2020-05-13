# JoyMobilitySDK

![CI Status](https://api.travis-ci.org/travis-ci/travis-web.svg?branch=master)
[![](https://jitpack.io/v/AlaaCherbib/JoyMobility_SDK.svg)](https://jitpack.io/#AlaaCherbib/JoyMobility_SDK)
[![License](https://img.shields.io/cocoapods/l/JoyMobilitySDK.svg?style=flat)](https://cocoapods.org/pods/JoyMobilitySDK)

Use JoyMobilitySDK to integrate our Carpooling service within your own App.  You can also use it to have your own white label Mobility App with just few lines of Code. The SDK is fully written in Kotlin.

![Feed](https://github.com/AlaaCherbib/JoyMobilitySDK_Android/blob/master/doc-assets/feed.png)
![Details](https://github.com/AlaaCherbib/JoyMobilitySDK_Android/blob/master/doc-assets/details.png)

## Requirements
Android API Level 23 and above (Android 6.0 +)

Sign up to our platform and get your customer id

## Installation

JoyMobilitySDK is available through [JitPack](https://jitpack.io/). To install
it, simply add the following to your project build.gradle file:

```ruby
    allprojects {
        repositories {
            ...
            maven { url 'https://jitpack.io' }
        }
    }
```

and to your App build.gradle file:
```ruby
    dependencies { 
        ...
        implementation 'com.github.Joy-Mobility:JoyMobility_SDK:1.0.1'
        implementation "androidx.lifecycle:lifecycle-runtime:2.0.0"
        implementation "androidx.lifecycle:lifecycle-extensions:2.0.0"
    }
```
Sync the gradle, clean and build the project.

## Getting Started

### Add your configuration files
After signing up, you will receive two json files, one is your GoogleService info file, which will be used to configure Firebase and Google maps services. this file should never be edited and must be added to your project. The second 'builder_config.json' file contains your specific keys, customer id, api keys and some config params, add this file as well to your project as it is mandatory to run the SDK.

### Customise the settings and appearance
in builder_config.json you will be able to do the following customisations:

* Enable/Disable sign up module: Depending on your use case, you can choose to use our Login feature and signup your users through our authentication services or you can choose to use your own authentication system, in this case you will have to pass the user infos to the SDK before starting using it.
* Enable/Disable pricing: The SDK comes with a pricing feature that suggests fees for sharing the rides. you can enable/disable this feature depending on your needs.
* Show/Hide an initial app consent screen, this screen shows a customised text that the user will have to accept before using the carpool service. this screen will be only shown once if it is enabled.
* Set primary color: if you want to edit the primary color that is in the SDK screens, please add a new color to your colors resources file and call it 'joy_primary', set the color value and the SDK will use it automatically.

### Start the SDK
in the App file add the following lines: 
```kotlin
import com.joy.library.JoyMobilityApp

override fun onCreate() {
    super.onCreate()
    val joyMobilityApp = JoyMobilityApp(this)
    joyMobilityApp.configure()
    ProcessLifecycleOwner.get().lifecycle.addObserver(JoyMobilityLifecycleObserver())
}
```

### Authenticate the user if you are disabling the SDK Sign up Module (SKIP THIS STEP IF YOU USE THE SDK'S SIGN UP SCREEN)
after the user logs in to his account in your app, please make this call to the authenticate the user for the carpool service:
```kotlin
    JoyMobilityApp.signInUser(
        email = "user_email",
        firstname = "user_fisrt_name",
        lastname = "user_last_name",
        phoneNumber = "001122334455",
        gender = JoyMobilityUserGender.MALE) { success ->
            // you can use the completion block if necessary or just set it to nil
    }
```

### Start the first screen
Now all you have to do is to choose your entry point, it can be a button or a Listview cell action, or a tab bar item if it is an in app integration, or in Application file if you are using the SDK as a white label product to generate a new standalone mobility app 

* in case of in app integration:
```kotlin
    val startupActivity = JoyMobilityApp.startupActivity()
    val intent = Intent(this, startupActivity)
    startActivity(intent)
```
* in case of a standalone App:

in Application file:

```kotlin
override fun onCreate() {
    super.onCreate()
    val joyMobilityApp = JoyMobilityApp(this)
    joyMobilityApp.configure()
    ProcessLifecycleOwner.get().lifecycle.addObserver(JoyMobilityLifecycleObserver())
    
    // Starting the new activity
    val startupActivity = joyMobilityApp.startupActivity()
    val intent = Intent(this, startupActivity)
    intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK)
    intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK)
    startActivity(intent)
}
```

That is basically it, you're all set up now, the rest will be managed by the SDK. Happy coding !!



## Author

JoyMobility, info@joy-mobility.com | www.joy-mobility.com

## License

JoyMobilitySDK is available under the MIT license. See the LICENSE file for more info.
