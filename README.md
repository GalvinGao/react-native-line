# ![React Native Line](/assets/banner.png)

[![npm version](https://img.shields.io/npm/v/@xmartlabs/react-native-line.svg?style=flat-square)](https://www.npmjs.com/package/@xmartlabs/react-native-line)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

Line SDK wrapper for React Native 🚀

- [LINE SDK v5 for iOS](https://developers.line.biz/en/reference/ios-sdk-swift/), wrapped with [Swift](https://developer.apple.com/swift/).
- [LINE SDK v5 for Android](https://developers.line.biz/en/reference/android-sdk/), wrapped with [Kotlin](https://kotlinlang.org/).

## Requirements

- Android `minSdkVersion` needs to be at least version `24`.
- iOS `deploymentTarget` needs to be at least version `15.1`.
- [LINE developer account](https://developers.line.biz/console/) with a channel created.

> [!IMPORTANT]
> @xmartlabs/react-native-line v5 is now a TurboModule and **requires the new architecture to be enabled**.
> - If you want to use @xmartlabs/react-native-line v5, you need to enable the new architecture in your app (see how to [enable the new architecture for apps](https://github.com/reactwg/react-native-new-architecture/blob/main/docs/enable-apps.md))
> - If you cannot enable the new architecture yet, downgrade to @xmartlabs/react-native-line v4 for now.

## Installation

### With Expo

1. Install the JavaScript side with:

    ```bash
    npx expo install @xmartlabs/react-native-line
    ```

2. Add the plugin `expo-build-properties` to your `app.json`:

    ```json
    "plugins": [
      [
        "expo-build-properties",
        {
          "ios": {
            "useFrameworks": "static" // This is required
          }
        }
      ],
      "@xmartlabs/react-native-line"
    ]
    ```

### With react-native-cli

1. Install library:

    ```bash
    npm install @xmartlabs/react-native-line

    # --- or ---

    yarn add @xmartlabs/react-native-line
    ```

2. Link native code:

    ```bash
    cd ios && pod install
    ```

3. Change your `AppDelegate` to match the following:

    #### With Swift

    <details>
      <summary>@xmartlabs/react-native-line v4</summary>

      ```swift
      import RNLine

      ...

      override func application(_ application: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        return LineLogin.application(application, open: url, options: options)
      }
      ```
    </details>

    <details>
      <summary>@xmartlabs/react-native-line v5</summary>

      ```swift
      import react_native_line

      ...

      override func application(_ application: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        return LineLogin.application(application, open: url, options: options)
      }
      ```
    </details>

    #### With Objective-C

    <details>
      <summary>@xmartlabs/react-native-line v4</summary>

      ```objectivec
      #import "RNLine-Swift.h"

      ...

      - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
      {
        return [LineLogin application:application open:url options:options];
      }
      ```
    </details>

    <details>
      <summary>@xmartlabs/react-native-line v5</summary>

      ```objectivec
      #import "react_native_line-Swift.h"

      ...

      - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
      {
        return [LineLogin application:application open:url options:options];
      }
      ```
    </details>
    <br>

4. Insert the following snippet in your `Info.plist` to match the [LINE documentation](https://developers.line.biz/en/docs/line-login-sdks/ios-sdk/swift/setting-up-project/#config-infoplist-file):

    ```xml
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleTypeRole</key>
            <string>Editor</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>line3rdp.$(PRODUCT_BUNDLE_IDENTIFIER)</string>
            </array>
        </dict>
    </array>
    <key>LSApplicationQueriesSchemes</key>
    <array>
        <string>lineauth2</string>
    </array>
    ```

## Migration guides

### v3 → v4

1. A `setup` function has been added and needs to be called before using the library.
    ```typescript
    Line.setup({ channelId: 'YOUR_CHANNEL_ID' })
    ```

2. The `getBotFriendshipStatus` function is now called `getFriendshipStatus`.

3. The `refreshToken` function is now called `refreshAccessToken`.

### v4 → v5

1. The file name in the `AppDelegate` import has changed.
    ```objectivec
    - #import "RNLine-Swift.h"

    + #import "react_native_line-Swift.h"
    ```

2. The `login` function now expects an empty object as a default value.
    ```typescript
    - Line.login()

    + Line.login({})
    ```

## Usage

1. Import the `Line` module:

    ```typescript
    import Line from '@xmartlabs/react-native-line'
    ```

2. Initialize the module with the `setup` method:

    ```typescript
    useEffect(() => {
      Line.setup({ channelId: 'YOUR_CHANNEL_ID' })
    }, [])
    ```

3. Login with the `login` method:

    ```typescript
    Line.login({})
    ```

## API

| Function                                                 | Description                                                                                                                                                                                                                        |
| -------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `login(params: LoginParams): Promise<LoginResult>` | Starts the login flow of Line's SDK (Opens the apps if it's installed and defaults to the browser otherwise). It accepts the same argumements as the LineSDK, in an object `{ key: value }`, defaults the same way as LineSDK too. |
| `getCurrentAccessToken(): Promise<AccessToken>` | Returns the access token of the current user. |
| `getProfile(): Promise<UserProfile>` | Returns the current user profile information. |
| `logout(): Promise<void>` | Revokes the access token of the current user. |
| `refreshAccessToken(): Promise<AccessToken>` | Refreshes the access token of the current user. |
| `setup(params: SetupParams): Promise<void>` | Initializes the Line SDK. |
| `verifyAccessToken(): Promise<VerifyResult>` | Checks whether the access token of the current user is valid. |
| `getFriendshipStatus(): Promise<FriendshipStatus>` | Gets the friendship status between the LINE Official Account (which is linked to the current channel) and the user if [configured](https://developers.line.biz/en/docs/line-login-sdks/ios-sdk/swift/link-a-bot/). |

## Example

If you want to see `@xmartlabs/react-native-line` in action, just move into the [example](/example) folder and run `npm install` and then `npm run ios`/`npm run android`. By seeing its source code, you will have a better understanding of the library usage.

## License

`@xmartlabs/react-native-line` is available under the MIT license. See the LICENCE file for more info.

<p align="center">
  <img src="https://github.com/user-attachments/assets/53fab07a-54f5-4f46-a894-e3476318a68d" alt="Xmartlabs Logo" width="150" />
</p>

<p align="center">
  <b>Created with ❤️ by <a href="https://xmartlabs.com/">Xmartlabs</a></b>
</p>
