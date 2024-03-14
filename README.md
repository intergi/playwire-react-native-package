<H1 align="center">Playwire React Native SDK</H1>

<p align="center">
    <a href="http://www.playwire.com"><img alt="Version" src="https://img.shields.io/badge/version-9.2.1-blue"></a>
</p>

---

# Requirements

- iOS 12.0+
- Xcode 10.0+
- Android 5.0+
- Android Studio 3.3.2 or higher
- React Native 0.72.0+
- JDK 11+

# Installation

The module intended to be used alongside [React Native](https://github.com/facebook/react-native), so it is nice to have knowledge of core concepts and some experience with React Native development. If you feel the need, you can start with [getting started](https://reactnative.dev/docs/getting-started) tutorial.

We also assume you are working on a machine with Xcode or Android Studio installed and setup.

1. The `Playwire React Native SDK` is stored on GitHub Packages, that is why you have to create a local `.npmrc` file or edit global one (`~/.npmrc`) to configure the scope mapping for your project. In the `.npmrc` file, insert the command below so GitHub Packages knows where to route package requests. Using an `.npmrc` file prevents from accidentally searching the package in the npmjs.org instead of GitHub Packages.

    ```bash
    @intergi:registry=https://npm.pkg.github.com
    ```

2. Even though the `Playwire React Native SDK` is accessible publicly, GitHub still requires to do authentication. See the official [GitHub Package's guide](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-npm-registry#authenticating-with-a-personal-access-token) to get more details about GitHub Packages authentication. 
You can authenticate to GitHub Packages with npm by either editing your `.npmrc` file to include your personal access token(PAT) or by logging in to npm on the command line using your username and PAT. See the official [GitHub guide](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) to get more details about PAT creation.

* To authenticate by adding your personal access token to your `.npmrc` file, edit the `.npmrc` file and include the following line, replacing `<GITHUB_PERSONAL_TOKEN>` with your personal access token.

    ```bash
    @intergi:registry=https://npm.pkg.github.com
    //npm.pkg.github.com/:_authToken=<GITHUB_PERSONAL_TOKEN>
    ```

* To authenticate by logging in to npm, use the next command, replacing `USERNAME` with your GitHub username, `GITHUB_PERSONAL_TOKEN` with your personal access token, and `PUBLIC-EMAIL-ADDRESS` with your email address.
If GitHub Packages is not your default package registry for using npm, we recommend you use the `--scope` flag with the owner of the package when you authenticate to GitHub Packages.

    ```bash
    $ npm login --scope=@intergi --registry=https://npm.pkg.github.com

    > Username: USERNAME
    > Password: GITHUB_PERSONAL_TOKEN
    > Email: PUBLIC-EMAIL-ADDRESS
    ```
>**Note**: If you have faced with the `npm ERR! code UNABLE_TO_GET_ISSUER_CERT_LOCALLY` error during login, run next command to resolve it. See more options [here](https://stackoverflow.com/a/45884819/6245536).
 

    ```bash
    $ npm config set strict-ssl false
    ```

3. Install the `Playwire React Native SDK` with [npm](https://www.npmjs.com) or [yarn](https://yarnpkg.com).

    ```bash
    $ npm install @intergi/react-native-playwire-sdk

    # or

    $ yarn add @intergi/react-native-playwire-sdk
    ```

    If you want to install specific version `x.y.z`, you should run next command.

    ```bash
    $ npm install @intergi/react-native-playwire-sdk@x.y.z

    # or

    $ yarn add @intergi/react-native-playwire-sdk@x.y.z
    ```

4. Once installing has been finished, the set of dependencies should be resolved. Follow the [Dependencies installation](#dependencies-installation) section to resolve all required dependencies.
5. Search for the initialization metadata (`publisherId`, `appId`) emailed by your Playwire Account Manager.
6. Follow the [Project Configuration](#project-configuration) section to adjust project's configuration.
7. Import the `Playwire React Native SDK` to your project.

    ```ts
    import {Playwire} from '@intergi/react-native-playwire-sdk';
    ```

8. Build and run your React Native project.

## Dependencies installation

### iOS

Your project has to use [CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started) to install the `Playwire` dependency into the iOS project.

Do the following to resolve required dependencies for iOS:

1. Define the source in the Podfile where the `Playwire` dependency is stored.

    ```ruby
    source 'https://github.com/CocoaPods/Specs.git'
    source 'https://github.com/intergi/playwire-ios-podspec'

    target 'Your Project Target' do
        # ...
    end
    ```

2. Define the `Playwire` dependency in the Podfile. If you want to install the `Total` version, use dependency below.

    ```ruby
    target 'Your Project Target' do
        # ...
        pod 'GoogleUtilities', :modular_headers => true
        pod 'FirebaseCoreInternal', :modular_headers => true
        pod 'FirebaseCore', :modular_headers => true
        pod 'Playwire', '9.2.0'
        # ...
    end
    ```

    If you want to install the `COPPA` version, you should refer to the corresponding specification.

    ```ruby
    target 'Your Project Target' do
        # ...
        pod 'GoogleUtilities', :modular_headers => true
        pod 'FirebaseCoreInternal', :modular_headers => true
        pod 'FirebaseCore', :modular_headers => true
        pod 'Playwire/Coppa', '9.2.0'
        # ...
    end
    ```

3. Install the pods.

    ```bash
    $ npx pod-install
    ```

4. Open your .xcworkspace file to see the project in Xcode and check installed `react-native-playwire-sdk`.

    ```bash
    $ open your-project.xcworkspace
    ```

### Android

Your project has to use [Gradle](https://docs.gradle.org/current/userguide/getting_started.html) to install the `Playwire` dependency into the Android project.

Do the following to resolve required dependencies for Android:

1. As the `Playwire React Native SDK` consumes native Android SDK from the remote GitHub Packages repository, even though such SDK is accessible publicly there, GitHub still requires to do authentication, that is why the `Playwire React Native SDK` is configured to have **`keystore.properties`** that should contain credentials to access the GitHub package. See official [GitHub Package's guide](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-gradle-registry) to get more details about Github authentication.
You have to create a **`keystore.properties`** file by yourself using the template below and put to the root of your Android project.

    ```text
    maven_repo_read_url=https://maven.pkg.github.com/intergi/playwire-android-binaries
    maven_repo_read_username=<GITHUB_USERNAME>
    maven_repo_read_password=<GITHUB_ACCESS_TOKEN>
    ```

>**Note**: To learn more about **`keystore.properties`** usage, please visit official [Android Developer's Tutorial](https://developer.android.com/studio/publish/app-signing#secure-shared-keystore).

2. Go to `build.gradle` and modify with next changes:

    ```text
    def keystorePropertiesFile = rootProject.file("keystore.properties")
    def keystoreProperties = new Properties()
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))

    // ...

    buildscript {
        // ...
        dependencies {
            // ...
            classpath "com.google.gms:google-services:4.3.14"
            // ...
        }
        // ...
    }

    allprojects {
        // ...
        repositories {
            // ...
            maven {
                name = "GitHubPackages"
                url = uri(keystoreProperties['maven_repo_read_url'])
                credentials {
                    username = keystoreProperties['maven_repo_read_username']
                    password = keystoreProperties['maven_repo_read_password']
                }
            }
            maven {
                url 'https://android-sdk.is.com/'
            }
            maven {
                // Pangle
                url 'https://artifact.bytedance.com/repository/pangle/'
            }
            // ...
        }
        // ...
    }
    ```

3. Define the `Playwire` dependency in the `android/app/build.gradle`. If you want to install the `Total` version, use dependency below.

    ```gradle
    dependencies {
        // ...
        api 'com.intergi.playwire:playwiresdk_total:9.2.0'
        api 'com.google.firebase:firebase-analytics-ktx:21.1.1'
        // ...
    }
    ```

    If you want to install the `COPPA` version, you should refer to the corresponding dependency.

    ```gradle
    dependencies {
        // ...
        api 'com.intergi.playwire:playwiresdk_coppa:9.2.0'
        api 'com.google.firebase:firebase-analytics-ktx:21.1.1'
        // ...
    }
    ```

4. Sync project with Gradle Files to install dependencies.

## Project Configuration

### iOS

1. Update your app's `Info.plist`.
    1. A `GADApplicationIdentifier` key with a string value provided by Playwire Account Manager, along with an initialization metadata.
    2. A `SKAdNetworkItems` key with `SKAdNetworkIdentifier` values that can be found [here](https://github.com/intergi/playwire-react-native-package/blob/master/SKAdNetworkIdentifers.xml).

    ```xml
    <dict>
        <!--  Replace with your GAD App Identifier  -->
        <key>GADApplicationIdentifier</key>
        <string>YOUR_GOOGLE_APP_ID</string>
        <key>SKAdNetworkItems</key>
        <array>
            <!--  ...  -->
            <dict>
                <key>SKAdNetworkIdentifier</key>
                <string>p78axxw29g.skadnetwork</string>
            </dict>
            <!-- ...  -->
        </array>
    </dict>
    ```

2. If your project is written with Objective-C, set the `Always Embed Swift Standard Libraries` to true in your project's build settings.

### Android

1. Update your app's `AndroidManifest.xml`. If you installed the `Total` version, add next values to the `AndroidManifest.xml`.

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <!--optional by Amazon if you need geo location -->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

    <!--recommended by AdColony -->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.VIBRATE" />

    <application>
        <!--required by Google -->
        <meta-data
            android:name="com.google.android.gms.ads.AD_MANAGER_APP"
            android:value="true"/>
        <meta-data
            android:name="com.google.android.gms.ads.APPLICATION_ID"
            android:value="YOUR_GOOGLE_APP_ID"/>
        
        <!--required by Amazon -->
        <activity android:name="com.amazon.device.ads.DTBInterstitialActivity"/>
        <activity android:name="com.amazon.device.ads.DTBAdActivity"/>
    </application>
    ```

    If you installed the `COPPA` version, add only next ones.

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.VIBRATE" />

    <application>
        <meta-data
            android:name="com.google.android.gms.ads.AD_MANAGER_APP"
            android:value="true"/>
        <meta-data
            android:name="com.google.android.gms.ads.APPLICATION_ID"
            android:value="YOUR_GOOGLE_APP_ID"/>
    </application>
    ```

2. Update your app's `build.gradle` to declare Java 8 compatibility.

    ```text
    android {
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
    }
    ```

# Upgrade guide

This section describes 3 steps for updating the `Playwire React Native SDK` to the latest version in your project.

1. Update `@intergi/react-native-playwire-sdk` package to the latest version.

    * If you are using `yarn` then run one of this commands:

    ```bash
    $ yarn upgrade @intergi/react-native-playwire-sdk
    # or
    $ yarn upgrade @intergi/react-native-playwire-sdk --latest
    ```

    > **Note**: Differences between such commands are described [here](https://jsramblings.com/how-to-upgrade-a-yarn-package-to-the-latest-version/).

    * If you are using `npm` then run such command:

    ```bash
    $ npm install @intergi/react-native-playwire-sdk@latest
    ```

    * Verify that you did update the `Playwire React Native SDK` to the latest version by checking `@intergi/react-native-playwire-sdk` directory inside `node_modules` directory by looking at `package.json` file that has `"version": "???"` field.

2. Update the `Playwire` pod dependency in iOS project.

    * If you previously decided to integrate the `Playwire` pod using certain SDK version `x.y.z` then just update `x.y.z` to the latest `Playwire` version and run such command in root directory:

    ```bash
    $ npx pod-install
    ```

    * If you did not use certain SDK version, run such command in `ios` directory:

    ```bash
    $ pod update Playwire
    # or
    $ pod update Playwire/Coppa
    ```

    * Verify that you did update the `Playwire` pod to the latest version by checking logs from `npx pod-install` or `pod update Playwire` commands. You can also verify it by opening `ios/Podfile.lock` file and searching for `- Playwire (x.y.z)` line, where `x.y.z` is the version of the `Playwire` pod that was downloaded and integrated.

3. Perform gradle sync in Android project.
    
    * Run Terminal CLI command `./gradlew sync` in `android` directory or preferably click "Sync now" in Android Studio. This should be enough if you have done 1st step beforehand correctly. 

    * Verify that you did update the `Playwire` dependency by going through "External Libraries" window in Android Studio and searching for `com.intergi.playwire:playwiresdk:x.y.z`, where `x.y.z` is the version of the `Playwire` dependency that was downloaded and integrated.
# Usage
## Initialization

Initialize the `Playwire React Native SDK` in your app.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';
import {Platform} from 'react-native';


var YOUR_PUBLISHER_ID = 'YOUR_PUBLISHER_ID';
var YOUR_APP_ID = '';

switch (Platform.OS) {
case 'ios':
    YOUR_APP_ID = 'YOUR_IOS_APP_ID';
    break;
case 'android':
    YOUR_APP_ID = 'YOUR_ANDROID_APP_ID';
    break;
default:
    console.error("Running on an unexpected platform.");
    return;
}

Playwire.initializeSDK(YOUR_PUBLISHER_ID, YOUR_APP_ID, () => {
    // Playwire SDK is initialized here.
});
```

> **Note**: A configuration file metadata such as `YOUR_PUBLISHER_ID`, `YOUR_APP_ID`, etc., must be provided by your Playwire Account Manager.

## Request for ads

### Test ads

To avoid ad filling issues during development, you may enable the 'test mode' to receive test ad creatives. It should be enabled before any ads requests, e.g. before SDK initialization. For banners, interstitials and app open ads, test mode will show a blue ad. For video ads, i.e - rewarded and rewarded interstitials, test mode will show an actual video instead of a blue ad.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';

Playwire.setTest(true);
```

> **Note**: The test mode must be enabled only for development builds. Make sure you disabled it for production builds, otherwise it may impact on revenue.
### Request banner ads

To display a banner ad on your app, you must first import `PlaywireBannerView`. It extends `React.Component` and can be used as view during rendering.

```ts
import {PlaywireBannerView} from '@intergi/react-native-playwire-sdk';
```

To request a banner ad, you must provide the ad unit.

```ts
import {PlaywireBannerView} from '@intergi/react-native-playwire-sdk';
// ...
render() {
    return (
        /* ... */
        <PlaywireBannerView adUnitId={'AdUnitId'} />
        /* ... */
    );
}
// ...
```

By default, size of `PlaywireBannerView` equals to ad content's size. But if you need to render a banner ad in the custom size, you can provide specific width and height for the banner container.

```ts
import {PlaywireBannerView} from '@intergi/react-native-playwire-sdk';
// ...
render() {
    var customSize = {
        width: 320,
        height: 120,
    };

    return (
        /* ... */
        <PlaywireBannerView adUnitId={'AdUnitId'} size={customSize} />
        /* ... */
    );
}
// ...
```

If the banner is loaded successfully, you would receive `onAdLoaded` callbacks and the banner content would be rendered automatically. If not, you would receive `onAdFailedToLoad` callbacks.

```ts
import {PlaywireBannerView} from '@intergi/react-native-playwire-sdk';
// ...
render() {
    return (
        /* ... */
        <PlaywireBannerView adUnitId={'AdUnitId'}
        onAdLoaded={(adUnitId) => {
            // The banner ad is loaded and rendered onscreen.
        }}
        onAdFailedToLoad={(adUnitId) => {
            // The banner ad is not loaded and would be loaded again.
        }} />
        /* ... */
    );
}
// ...
```

`PlaywireBannerView` provides the banner-related callbacks to inform you of the banner ad lifecycle. You can subscribe to be notified about events and how to handle them.
See the list below for banner-related callbacks.

```ts
  PlaywireBannerView.propTypes = {
    /**
    * It is fired when the banner ad successfully loaded content.
    */
    onAdLoaded: PropTypes.func,
    /**
    * It is fired when the banner ad failed to load content.
    */
    onAdFailedToLoad: PropTypes.func,
    /**
    * It is fired when the banner ad presented content.
    */
    onAdOpened: PropTypes.func,
    /**
    * It is fired when the banner ad dismissed content.
    */
    onAdClosed: PropTypes.func,
    /**
    * It is fired when a click has been recorded for the banner ad.
    */
    onAdClicked: PropTypes.func,
    /**
    * It is fired when an impression has been recorded for the banner ad.
    */
    onAdImpressionRecorded: PropTypes.func,
  };

```

### Request for interstitial ads

To display an interstitial ad on your app, you must first request it and provide the ad unit.

When requesting an interstitial ad, we recommend that you do so in advance before planning to present it to your user as the loading process may take time.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';

var adUnitId = "AdUnitId";
Playwire.loadInterstitial(adUnitId);
```

> **Note**: An interstitial ad is a one-time-use object, which means it must be loaded again after its shown. Use the `Playwire.getIsInterstitialReady(adUnitId, callback)` method to check if the ad is ready to be presented.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';

var adUnitId = "AdUnitId";
Playwire.getInterstitialReady(adUnitId, isReady => {
    if (isReady) {
        // The interstitial ad is ready and can be presented.
    } else {
        // The interstitial ad cannot be presented and must be loaded.
    }
});
```

If the interstitial is loaded successfully, you can present full screen content.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';

string adUnitId = "AdUnitId";
Playwire.showInterstitial(adUnitId);
```

> **Note**: The interstitial ad is presented only if it is loaded and not shown previously. Otherwise, `onFailedToOpenEvent` is invoked.

Playwire provides interstitial-related callbacks to inform you about an interstitial ad lifecycle. You can add listeners to be notified about events and how to handle them.

```ts
onSDKInitialization = () => {
    Playwire.addInterstitialLoadedEventListener((adUnitId) => {
        // ...
        Playwire.getInterstitialReady(adUnitId, isReady => {
            if (isReady) {
                Playwire.showInterstitial(adUnitId);
            }
            else {
                // Load interstitial again or show error.
            }
        });
        // ...
    });

    Playwire.addInterstitialFailedToLoadEventListener((adUnitId) => {
        // ...
    });
};
```

See the list below for interstitial-related methods to add listeners.

```ts
export namespace Playwire = {
    /**
    * It is fired when the interstitial ad successfully loaded full screen content and ready to be presented.
    */
    export function addInterstitialLoadedEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when the interstitial ad failed to load full screen content.
    */
    export function addInterstitialFailedToLoadEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when the interstitial ad presented full screen content.
    */
    export function addInterstitialOpenedEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when the interstitial ad failed to present full screen content.
    */
    export function addInterstitialFailedToOpenEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when an interstitial ad dismissed full screen content and the user goes back to the application screen.
    */
    export function addInterstitialClosedEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when an impression has been recorded for the interstitial ad.
    */
    export function addInterstitialRecordedImpressionEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when a click has been recorded for the interstitial ad.
    */
    export function addInterstitialClickedEventListener(callback: (adUnitId: string) => void): void;
}
```

### Request for rewarded ads

To display a rewarded ad on your app, you must first request it and provide the ad unit.

When requesting a rewarded ad, we recommend that you do so in advance before planning to present it to your user as the loading process may take time.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';

var adUnitId = "AdUnitId";
Playwire.loadRewarded(adUnitId);
```

> **Note**: A rewarded ad is a one-time-use object, which means it must be loaded again after its shown. Use the `Playwire.getIsRewardedReady(adUnitId, callback)` method to check if the ad is ready to be presented.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';

var adUnitId = "AdUnitId";
Playwire.getIsRewardedReady(adUnitId, isReady => {
    if (isReady) {
        // The rewarded ad is ready and can be presented.
    } else {
        // The rewarded ad cannot be presented and must be loaded.
    }
});
```

If the rewarded ad is loaded successfully, you can present full screen content.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';

var adUnitId = "AdUnitId";
Playwire.showRewarded(adUnitId);
``` 

>**Note**: The rewarded ad is presented only if it is loaded and not shown previously. Otherwise `onFailedToOpenEvent` is invoked.

Playwire provides rewarded-related callbacks to inform you about a rewarded ad lifecycle. You can add listeners to be notified about the events and how to handle them.

```ts
onSDKInitialization = () => {
    Playwire.addRewardedLoadedEventListener((adUnitId) => {
        // ...
        Playwire.getIsRewardedReady(adUnitId, isReady => {
            if (isReady) {
                Playwire.showRewarded(adUnitId);
            }
            else {
                // Load rewarded again or show error.
            }
        });
        // ...
    });

    Playwire.addRewardedFailedToLoadEventListener((adUnitId) => {
        // ...
    });
};
```

See the list below for rewarded-related methods to add listeners.

```ts
export namespace Playwire {
    /**
    * It is fired when the rewarded ad successfully loaded full screen content and is ready to be presented.
    */
    export function addRewardedLoadedEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when the rewarded ad failed to load full screen content.
    */
    export function addRewardedFailedToLoadEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when the rewarded ad presented full screen content.
    */
    export function addRewardedOpenedEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when the rewarded ad failed to present full screen content.
    */
    export function addRewardedFailedToOpenEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when an rewarded ad dismissed full screen content and the user goes back to the application screen.
    */
    export function addRewardedClosedEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when an impression has been recorded for the rewarded ad.
    */
    export function addRewardedRecordedImpressionEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when a reward has been earned.
    */
    export function addRewardedEarnedEventListener(callback: (reward: AdReward) => void): void;
    /**
    * It is fired when a click has been recorded for the rewarded ad.
    */
    export function addRewardedClickedEventListener(callback: (adUnitId: string) => void): void;
}
```

### Request for rewarded interstitial ads

To display a rewarded interstitial ad on your app, you must first request it and provide the ad unit.

When requesting a rewarded interstitial ad, we recommend that you do so in advance before planning to present it to your user as the loading process may take time.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';

var adUnitId = "AdUnitId";
Playwire.loadRewardedInterstitial(adUnitId);
```

> **Note**: A rewarded interstitial ad is a one-time-use object, which means it must be loaded again after its shown. Use the `Playwire.getIsRewardedInterstitialReady(adUnitId, callback)` method to check if the ad is ready to be presented.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';

var adUnitId = "AdUnitId";
Playwire.getIsRewardedInterstitialReady(adUnitId, isReady => {
    if (isReady) {
        // The rewarded interstitial ad is ready and can be presented.
    } else {
        // The rewarded interstitial ad cannot be presented and must be loaded.
    }
});
```

If the rewarded interstitial ad is loaded successfully, you can present full screen content.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';

var adUnitId = "AdUnitId";
Playwire.showRewardedInterstitial(adUnitId);
``` 

>**Note**: The rewarded interstitial ad is presented only if it is loaded and not shown previously. Otherwise `onFailedToOpenEvent` is invoked.

Playwire provides rewarded interstitial-related callbacks to inform you about a rewarded ad lifecycle. You can add listeners to be notified about the events and how to handle them.

```ts
onSDKInitialization = () => {
    Playwire.addRewardedInterstitialLoadedEventListener((adUnitId) => {
        // ...
        Playwire.getIsRewardedInterstitialReady(adUnitId, isReady => {
            if (isReady) {
                Playwire.showRewardedInterstitial(adUnitId);
            }
            else {
                // Load the rewarded interstitial ad again or show error.
            }
        });
        // ...
    });

    Playwire.addRewardedInterstitialFailedToLoadEventListener((adUnitId) => {
        // ...
    });
};
```

See the list below for rewarded interstitial-related methods to add listeners.

```ts
export namespace Playwire {
    /**
    * It is fired when the rewarded interstitial ad successfully loaded full screen content and is ready to be presented.
    */
    export function addRewardedInterstitialLoadedEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when the rewarded interstitial ad failed to load full screen content.
    */
    export function addRewardedInterstitialFailedToLoadEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when the rewarded interstitial ad presented full screen content.
    */
    export function addRewardedInterstitialOpenedEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when the rewarded interstitial ad failed to present full screen content.
    */
    export function addRewardedInterstitialFailedToOpenEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when an rewarded interstitial ad dismissed full screen content and the user goes back to the application screen.
    */
    export function addRewardedInterstitialClosedEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when an impression has been recorded for the rewarded interstitial ad.
    */
    export function addRewardedInterstitialRecordedImpressionEventListener(callback: (adUnitId: string) => void): void;
    /**
    * It is fired when a reward has been earned.
    */
    export function addRewardedInterstitialEarnedEventListener(callback: (reward: AdReward) => void): void;
    /**
    * It is fired when a click has been recorded for the rewarded interstitial ad.
    */
    export function addRewardedInterstitialClickedEventListener(callback: (adUnitId: string) => void): void;
}
```

### Request for app open ads

To display an app open ad on your app, you must first request it and provide the ad unit.

When requesting an app open ad, we recommend that you do so in advance before planning to present it to your user as the loading process may take time.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';

var adUnitId = "AdUnitId";
Playwire.loadAppOpenAd(adUnitId);
```

> **Note**: An app open ad is a one-time-use object, which means it must be loaded again after its shown. Use the `Playwire.getAppOpenAdReady(adUnitId, callback)` method to check if the ad is ready to be presented.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';

var adUnitId = "AdUnitId";
Playwire.getAppOpenAdReady(adUnitId, isReady => {
    if (isReady) {
        // The app open ad is ready and can be presented.
    } else {
        // The app open ad cannot be presented and must be loaded.
    }
});
```

An app open ad will time out after four hours. If you present an ad content that was requested for more than four hours, it will no longer be valid and may not earn revenue.
To ensure you do not show an expired ad, you can check how long it has been since your ad loaded and reload it manually, or you may enable `appOpenAdAutoReloadOnExpiration` to let the `Playwire React Native SDK` monitors the ad expiration and take care about reloading the expired ad.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';

var adUnitId = "AdUnitId";
Playwire.getAppOpenAdAutoReloadOnExpiration(adUnitId, isEnabled => {
    if (!isEnabled) {
        Playwire.setAppOpenAdAutoReloadOnExpiration(adUnitId, true);
    }
});
```

If the app open ad is loaded successfully, you can present full screen content.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';

var adUnitId = "AdUnitId";
Playwire.showAppOpenAd(adUnitId);
```

> **Note**: The app open ad is presented only if it is loaded and not shown previously. Otherwise, `onFailedToOpenEvent` is invoked.

As app open ads are designed to be shown when a user brings your app to the foreground, you need to listen to the application state changes. You can do it by subscribing to [AppState](https://reactnative.dev/docs/appstate).
```ts
import React, {useRef} from 'react';
import {AppState} from 'react-native';

const appState = useRef(AppState.currentState);

onSDKInitialization = () => {
    AppState.addEventListener('change', nextAppState => {
      if (appState.match(/background/) && nextAppState === 'active') {

        var adUnitId = "AdUnitId";
        Playwire.showAppOpenAd(adUnitId);
      }
    });
}
```

Playwire provides 'app open ad'-related callbacks to inform you about an app open ad lifecycle. You can add listeners to be notified about events and how to handle them.

```ts
onSDKInitialization = () => {
    Playwire.addAppOpenAdLoadedEventListener((adUnitId) => {
        // ...
        Playwire.getAppOpenAdReady(adUnitId, isReady => {
            if (isReady) {
                Playwire.showAppOpenAd(adUnitId);
            }
            else {
                // Load the app open ad again or show error.
            }
        });
        // ...
    });

    Playwire.addAppOpenAdFailedToLoadEventListener((adUnitId) => {
        // ...
    });
};
```

See the list below for 'app open ad'-related methods to add listeners.

```ts
export namespace Playwire = {
    /**
    * Add a handler to event when the app open ad successfully loaded full screen content. 
    */
    export function addAppOpenAdLoadedEventListener(handler: AdUnitEventHandler): void
    /**
     * Add a handler to event when the app open ad failed to load full screen content. 
     */
    export function addAppOpenAdFailedToLoadEventListener(handler: AdUnitEventHandler): void;
    /**
     * Add a handler to event when the app open ad presented full screen content. 
     */
    export function addAppOpenAdOpenedEventListener(handler: AdUnitEventHandler): void;
    /**
     * Add a handler to event when the app open ad failed to present full screen content. 
     */
    export function addAppOpenAdFailedToOpenEventListener(handler: AdUnitEventHandler): void;
    /**
     * Add a handler to event when the app open ad dismissed full screen content. 
     */
    export function addAppOpenAdClosedEventListener(handler: AdUnitEventHandler): void;
    /**
     * Add a handler to event when an app open has been recorded for the app open ad. 
     */
    export function addAppOpenAdRecordedImpressionEventListener(handler: AdUnitEventHandler): void;
    /**
     * Add a handler to event when a click has been recorded for the app open ad. 
     */
    export function addAppOpenAdClickedEventListener(handler: AdUnitEventHandler): void;
}
```

### Custom Targeting

If you require specifying the type of content of the ads that will be delivered you could tag your requests with specific words.

Tags are handled in the `PlaywireTargeting` class. You can pass `[key: string]: string` with targets, set and remove tags at desired indexes or using arrays.

```ts
import {PlaywireTargeting} from '@intergi/react-native-playwire-sdk';

var targets = {'custom_key': 'ad_unit'};
var targeting = new PlaywireTargeting();

// Add object `[key: string]: string`
targeting.add(targets);

// Remove keys
targeting.remove(['custom_key']);

// Set client tag at index 1
targeting.setClientTag('custom_tag', 1);

// Remove client tag at index 1
targeting.removeClientTag(1);

// Clear all
targeting.clear();
```

The custom targeting can be set at several levels:

1. **Config level**

    You can decide to tag a specific ad unit in the backend configuration, no code will be required in the application. Please contact your Account Manager for it.

2. **Global level**

    You can put tags in the global PlaywireSDK object. These tags will be applied to every request of every ad unit in your application.

    ```ts
    import {Playwire, PlaywireTargeting} from '@intergi/react-native-playwire-sdk';
    // ...
    var targets = {'global_key': 'global'};
    var targeting = new PlaywireTargeting();
    targeting.add(targets);
    targeting.setClientTag('global_tag', 1);

    Playwire.setGlobalTargeting(targeting);
    // ...
    ```

3. **Ad unit level**

    You can put tags in the ad unit level and they will be taken into account for any future requests on these ad units. In view ads like banners, changing tags after loading the ad will take effect in future refresh of ad content.

    ```ts
    import {Playwire, PlaywireTargeting} from '@intergi/react-native-playwire-sdk';
    // ...
    var targets = {'ad_unit_key': 'ad_unit'};
    var targeting = new PlaywireTargeting();
    targeting.add(targets);
    targeting.setClientTag('ad_unit_tag', 1);

    var adUnitId = 'adUnitId';

    // Make sure SDK is initialized to set ad unit level targeting
    Playwire.setInterstitialTargeting(adUnitId, targeting);
    Playwire.setRewardedTargeting(adUnitId, targeting);
    Playwire.setRewardedInterstitialTargeting(adUnitId, targeting);
    Playwire.setAppOpenAdTargeting(adUnitId, targeting);
    // ...
    ```

    ```ts
    import {PlaywireBannerView, PlaywireTargeting} from '@intergi/react-native-playwire-sdk';
    // ...
    render() {

        var targeting = new PlaywireTargeting()
        .add({'ad_unit_key': 'ad_unit'})
        .setClientTag('ad_unit_tag', 1);

        return (
            /* ... */
            <PlaywireBannerView adUnitId={'AdUnitId'} targeting={targeting} />
            /* ... */
        );
    }
    // ...
    ```

4. **Ad unit loading level**

    If you need to provide the custom targets which will be included in a single ad request, pass the `targeting` argument to the load method.

    ```ts
    import {Playwire, PlaywireTargeting} from '@intergi/react-native-playwire-sdk';
    // ...
    var targets = {'ad_unit_key': 'ad_unit'};
    var targeting = new PlaywireTargeting();
    targeting.add(targets);
    targeting.setClientTag('ad_unit_tag', 1);

    var adUnitId = 'adUnitId';

    Playwire.loadInterstitial(adUnitId, targeting);
    Playwire.loadRewarded(adUnitId, targeting);
    Playwire.loadRewardedInterstitial(adUnitId, targeting);
    Playwire.loadAppOpenAd(adUnitId, targeting);
    // ...
    ```

## Logger

To start monitoring events inside the `Playwire React Native SDK` use a logger to log events to the IDE console. The logs can contain information about the event's name, ad unit parameters, ad server response, etc.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';
Playwire.startConsoleLogger();
```