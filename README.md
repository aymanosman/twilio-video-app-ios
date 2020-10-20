# Twilio Video iOS App

This app is a sample video conferencing app that uses the [Twilio Programmable Video SDK](https://www.twilio.com/docs/video/ios). The open source app can be easily configured by developers to try out real-time video and audio features. 

![App Preview](https://user-images.githubusercontent.com/12685223/94631109-cfca1c80-0284-11eb-8b72-c97276cf34e4.png)

## Features

- [x] Video conferencing with real-time video and audio
- [x] Enable/disable camera
- [x] Mute/unmute mic
- [x] Switch between front and back camera
- [x] [Dominant speaker](https://www.twilio.com/docs/video/detecting-dominant-speaker) indicator
- [x] [Network quality](https://www.twilio.com/docs/video/using-network-quality-api) indicator
- [x] [Bandwidth Profile API](https://www.twilio.com/docs/video/tutorials/using-bandwidth-profile-api)

## Requirements

iOS Deployment Target | Xcode Version | Swift Language Version
------------ | ------------- | -------------
11.0 | 12.0 | Swift 5

## Getting Started

### Deploy Twilio Access Token Server

**NOTE:** The Twilio Function that provides access tokens via a passcode should *NOT* be used in a production environment. This token server supports seamlessly getting started with the collaboration app, and while convenient, the passcode is not secure enough for production environments. You should use an authentication provider to securely provide access tokens to your client applications. You can find more information about Programmable Video access tokens [in this tutorial](https://www.twilio.com/docs/video/tutorials/user-identity-access-tokens).

The app requires a back-end to generate [Twilio access tokens](https://www.twilio.com/docs/video/tutorials/user-identity-access-tokens). Follow the instructions below to deploy a serverless back-end using [Twilio Functions](https://www.twilio.com/docs/runtime/functions).

1. [Install Twilio CLI](https://www.twilio.com/docs/twilio-cli/quickstart).
1. Run `twilio login` and follow prompts to [login to your Twilio account](https://www.twilio.com/docs/twilio-cli/quickstart#login-to-your-twilio-account).
1. Run `twilio plugins:install @twilio-labs/plugin-rtc`.
1. Run `twilio rtc:apps:video:deploy --authentication passcode`.
1. The passcode that is output will be used later to [sign in to the app](#start-video-conference).

The passcode will expire after one week. To generate a new passcode, run `twilio rtc:apps:video:deploy --authentication passcode --override`.

#### Troubleshooting

If any errors occur after running a [Twilio CLI RTC Plugin](https://github.com/twilio-labs/plugin-rtc) command, or the application fails to validate a passcode, then try the following steps.

1. Update your application to the latest source
1. Run `twilio plugins:update` to update the RTC plugin to the latest version.
1. Run `twilio rtc:apps:video:delete` to delete any existing authentication servers.
1. Run `twilio rtc:apps:video:deploy --authentication passcode` to deploy a new authentication server.

#### App Behavior with Different Room Types

**NOTE:** Usage charges will apply for most room types. See [pricing](https://www.twilio.com/video/pricing) for more information.

After running the command [to deploy a Twilio Access Token Server](#deploy-twilio-access-token-server), the room type will be returned in the command line output. Each room type provides a different video experience. More details about these room types can be found [here](https://www.twilio.com/docs/video/tutorials/understanding-video-rooms). The rest of this section explains how these room types affect the behavior of the video app.

*Group* - The Group room type allows up to fifty participants to join a video room in the app. The Network Quality Level (NQL) indicators and dominant speaker are demonstrated with this room type. Also, the VP8 video codec with simulcast enabled along with a bandwidth profile are set by default in order to provide an optimal group video app experience.

*Small Group* - The Small Group room type provides an identical group video app experience except for a smaller limit of four participants.

*Peer-to-peer* - Although up to ten participants can join a room using the Peer-to-peer (P2P) room type, it is ideal for a one to one video experience. The NQL indicators, bandwidth profiles, and dominant speaker cannot be used with this room type. Thus, they are not demonstrated in the video app. Also, the VP8 video codec with simulcast disabled and 720p minimum video capturing dimensions are also set by default in order to provide an optimal one to one video app experience. If more than ten participants join a room with this room type, then the video app will present an error.

*Go* - The Go room type provides a similar Peer-to-peer video app experience except for a smaller limit of two participants. If more than two participants join a room with this room type, then the video app will present an error.

If the max number of participants is exceeded, then the video app will present an error for all room types.

### Install Dependencies

1. Install [CocoaPods](https://cocoapods.org).
1. Run `pod install`.

### Configure Signing

1. Open `VideoApp.xcworkspace` with Xcode.
1. In Xcode navigate to the [Signing & Capabilities pane](https://developer.apple.com/documentation/xcode/adding_capabilities_to_your_app) of the project editor for the `Video-Community` target.
1. Change `Team` to your team.
1. Change `Bundle identifier` to something unique.

### Run

1. In Xcode use the [Scheme menu](https://developer.apple.com/library/archive/documentation/ToolsLanguages/Conceptual/Xcode_Overview/BuildingYourApp.html) to select the `Video-Community` scheme. 
1. In Xcode use the [Scheme menu](https://developer.apple.com/library/archive/documentation/ToolsLanguages/Conceptual/Xcode_Overview/BuildingYourApp.html) to select a destination. Cameras do not work in the simulator so select a device for best results.
1. Run `⌘R` the app.

The `Video-Internal` scheme uses authentication that is only available to Twilio employees in order to make internal testing easier. 

### Start Video Conference

For each device:

1. [Run](#run) the app.
1. Enter any unique name in the `Your name` field.
1. Enter the passcode from [Deploy Twilio Access Token Server](#deploy-twilio-access-token-server) in the `Passcode` field.
1. Tap `Sign in`.
1. Enter a room name.
1. Tap `Join`.

The passcode will expire after one week. Follow the steps below to sign in with a new passcode.

1. [Generate a new passcode](#deploy-twilio-access-token-server).
1. In the app tap `Settings > Sign Out`.
1. Repeat the [steps above](#start-video-conference).

## Tests

### Unit Tests

For unit tests use:

- `Video-Internal` scheme.
- `Video-InternalTests` target.
- `Unit` test plan.
- [Quick and Nimble](https://github.com/Quick/Quick) to write unit tests.
- [Swift Mock Generator](https://github.com/seanhenry/SwiftMockGeneratorForXcode) to create mocks.

### UI Tests

UI tests require credentials that are only available to Twilio employees.

### Known Issues

1. Running tests `⌘U` will crash if the app was run `⌘R` on the device previously. See issue [#12](https://github.com/twilio/twilio-video-app-ios/issues/12) for a workaround and more details.

## Related

- [Twilio Video Android App](https://github.com/twilio/twilio-video-app-android)
- [Twilio Video React App](https://github.com/twilio/twilio-video-app-react)
- [Twilio CLI RTC Plugin](https://github.com/twilio-labs/plugin-rtc)

## For Twilions

Twilio employees should follow [these instructions](ForTwilions.md) for internal testing.

## License

Apache 2.0 license. See the [LICENSE](LICENSE) file for details.
