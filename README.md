<H1 align="center">Playwire React Native SDK</H1>

<p align="center">
    <a href="http://www.playwire.com"><img alt="Version" src="https://img.shields.io/badge/version-4.0.1-blue"></a>
</p>

---

# Requirements

- iOS 11.0+
- Xcode 10.0+
- Android 5.0+
- Android Studio 3.3.2 or higher
- React Native 0.63.0+

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
5. Search for the initialization metadata (`publisherId`, `appId` and `version`) emailed by your Playwire Account Manager.
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
        pod 'Playwire', '4.0.1'
        # ...
    end
    ```

    If you want to install the `COPPA` version, you should refer to the corresponding specification.

    ```ruby
    target 'Your Project Target' do
        # ...
        pod 'Playwire/Coppa', '4.0.1'
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
                url "https://s3.amazonaws.com/smaato-sdk-releases/"
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
        api "com.intergi.playwire:playwiresdk_total:4.0.1"
        // ...
    }
    ```

    If you want to install the `COPPA` version, you should refer to the corresponding dependency.

    ```gradle
    dependencies {
        // ...
        api "com.intergi.playwire:playwiresdk_coppa:4.0.1"
        // ...
    }
    ```

4. Sync project with Gradle Files to install dependencies.

## Project Configuration

### iOS

1. Update your app's `Info.plist`.
    1. A `GADApplicationIdentifier` key with a string value provided by Playwire Account Manager, along with a config file.
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

    <!--recommended by Smaato -->
    <uses-feature android:name="android.hardware.location.network" />

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

# Migration guide
## Migrating from the Playwire React Native SDK 3.X.Y to the Playwire React Native SDK 4.X.Y

1. If you haven't made it yet, upgrade to the `Playwire React Native SDK` 4.X.Y package. See the [Upgrade guide](#upgrade-guide) section to update the package.
2. See the [Project Configuration](#project-configuration) section to adjust the project's configuration.
3. The `Playwire React Native SDK` 4.X.Y introduces the new approach to fetch config files from the remote, that is why you have to remove old config files that are stored locally. They are called **`PWConfigFile.json`** usually, if you have not renamed it.
4. Resolve all errors and issues regarding public API changes, e.g., `setConfigName` method does not exist anymore. See the [Initialization](#initialization) section to get more details.

# Usage
## Initialization

Initialize the `Playwire React Native SDK` in your app.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';
import {Platform} from 'react-native';


var YOUR_PUBLISHER_ID = 'YOUR_PUBLISHER_ID';
var YOUR_APP_ID = '';
var YOUR_VERSION = '';

switch (Platform.OS) {
case 'ios':
    YOUR_APP_ID = 'YOUR_IOS_APP_ID';
    YOUR_VERSION = 'YOUR_IOS_APP_VERSION';
    break;
case 'android':
    YOUR_APP_ID = 'YOUR_ANDROID_APP_ID';
    YOUR_VERSION = 'YOUR_ANDROID_APP_VERSION';
    break;
default:
    console.error("Running on an unexpected platform.");
    return;
}

Playwire.initializeSDK(YOUR_PUBLISHER_ID, YOUR_APP_ID, YOUR_VERSION, () => {
    // Playwire SDK is initialized here.
});
```

> **Note**: A configuration file metadata such as `YOUR_PUBLISHER_ID`, `YOUR_APP_ID`, etc., must be provided by your Playwire Account Manager.

## Request for ads
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

## Logger

To start monitoring events inside the `Playwire React Native SDK` use a logger to log events to the IDE console. The logs can contain information about the event's name, ad unit parameters, ad server response, etc.

```ts
import {Playwire} from '@intergi/react-native-playwire-sdk';
Playwire.startConsoleLogger();
```