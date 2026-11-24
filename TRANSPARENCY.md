# ChatLoop Technical Transparency

This document describes what the official ChatLoop Android app can access, what it does with that access and how the published APK can be checked independently.

Scope of this report: **ChatLoop v1.0.2**, package `com.chatloop.prototype`, version code `37`.

## Short version

- ChatLoop does **not** request Android's Internet permission.
- It has no ChatLoop account, login, analytics, advertising SDK, API key or external voice service.
- It uses the microphone locally for voice-activity detection. It does not transcribe, save or upload microphone audio.
- ChatGPT's own Dictation feature performs speech-to-text.
- Its Accessibility service is configured for the official ChatGPT Android package, `com.openai.chatgpt`.
- Accessibility is used to recognise and operate ChatGPT's Dictation, Send and Read Aloud controls and to observe when the relevant ChatGPT state changes.
- ChatLoop stores app settings locally. It does not store conversations or microphone recordings.

Accessibility and microphone access are sensitive permissions. This document explains why ChatLoop needs them rather than asking users to dismiss Android's warnings blindly.

## Declared Android permissions

The signed v1.0.2 APK declares exactly these permissions:

| Permission | Purpose |
| --- | --- |
| `android.permission.RECORD_AUDIO` | Supplies local audio samples to voice-activity detection so ChatLoop can distinguish speech from a pause and apply the selected pause-before-send timer. |
| `android.permission.FOREGROUND_SERVICE` | Supports Android's rules for user-visible, ongoing work. |
| `android.permission.FOREGROUND_SERVICE_MICROPHONE` | Allows microphone-related ongoing work under Android's foreground-service rules. |

The APK does **not** declare `android.permission.INTERNET`. Under Android's permission model, ChatLoop cannot directly open ordinary Internet connections. The official ChatGPT app uses its own network access when the user sends a message; that is separate from ChatLoop.

The APK also does not request access to contacts, SMS, call logs, location, camera, photos, storage, notifications or accounts.

## Accessibility configuration

The Accessibility service is configured with:

- target package: `com.openai.chatgpt`;
- event types: window-state, window-content, view-scroll and view-text changes;
- permission to retrieve interactive window content; and
- permission to perform gestures.

The service component is not exported to other apps. Android still describes Accessibility as powerful access because an enabled Accessibility service can inspect interface content and perform actions. In ChatLoop, that access is used to:

1. confirm that the active interface belongs to the official ChatGPT app;
2. find the relevant Dictation, Send and Read Aloud controls;
3. detect when Dictation, response generation and Read Aloud change state;
4. operate those controls during a session the user started; and
5. stop or pause rather than blindly pressing when the expected ChatGPT control cannot be identified safely.

Because ChatLoop can retrieve ChatGPT's active window content, it can technically encounter text displayed in the ChatGPT interface. The implementation uses interface text and control metadata to identify controls and state. It does not save or transmit conversation text.

ChatLoop does not ask for a ChatGPT password, login code, session cookie or API key.

## Microphone and local voice detection

ChatLoop uses Android `AudioRecord` and a packaged Silero voice-activity-detection model. The model runs locally through ONNX Runtime.

The local detector answers a narrow question: **is the user currently speaking, or has the selected pause elapsed?** It does not convert speech into words. Audio samples are processed in memory for that detection and are not written to a recording file or sent to a server.

When ChatLoop opens ChatGPT Dictation, the official ChatGPT app handles transcription under the user's OpenAI account and settings.

## Data flow during a normal turn

1. The user starts ChatLoop and returns to an ordinary conversation in the official ChatGPT app.
2. ChatLoop activates ChatGPT Dictation.
3. ChatGPT handles speech-to-text and displays the transcription.
4. ChatLoop's local voice detector recognises the selected pause after speech.
5. ChatLoop activates ChatGPT's Send control.
6. The official ChatGPT app sends the message and produces the response.
7. ChatLoop identifies the completed readable response and activates ChatGPT's Read Aloud control.
8. When playback finishes, ChatLoop returns to Dictation for the next turn.

If ChatLoop cannot safely identify the expected control, the user leaves ChatGPT, or the response has no readable text, the loop pauses instead of continuing to press controls.

## Information stored locally

ChatLoop uses Android private app preferences for settings such as:

- pause-before-send duration;
- overlay state and whether it starts collapsed;
- visual theme;
- motion preference; and
- optional behaviour and listening-cue settings.

These preferences are private to the app under Android's normal application storage model. ChatLoop does not use them to store chat content or audio.

The app manifest sets Android backup to disabled.

## Application components

The signed APK contains:

| Component | Exported | Purpose |
| --- | --- | --- |
| `MainActivity` | Yes | Normal launcher activity used to configure and start ChatLoop. It is exported because Android launcher activities must be reachable from the device launcher. |
| `ChatGptAccessibilityService` | No | Performs the ChatGPT-specific Accessibility work after the user explicitly enables the service in Android settings. It requires Android's `BIND_ACCESSIBILITY_SERVICE` system permission. |

The manifest contains no broadcast receivers, content providers or additional exported services.

## Included runtime components

The v1.0.2 build uses:

- Android/Kotlin platform runtime components;
- Android VAD Silero `2.0.10`;
- the packaged `silero_vad.onnx` model; and
- ONNX Runtime for Android `1.22.0`, included as native libraries for supported Android processor architectures.

Silero VAD and ONNX Runtime are third-party open-source components governed by their own licences. Their inclusion does not make ChatLoop's original application code open source.

The relatively large APK size is primarily caused by the packaged local model and ONNX Runtime native libraries for multiple Android architectures, not by uploaded conversation data or a remote service.

## Release verification

Official release file:

`ChatLoop-v1.0.2.apk`

APK SHA-256:

`9cff390b0cfb3c2973adc0fac77ce6a446b8996a0ba173229bb77d0fd17e4fff`

Android package:

`com.chatloop.prototype`

Version:

`1.0.2` (`versionCode 37`)

Signing certificate:

- Subject: `CN=ChatLoop`
- SHA-256: `da64d53d4e86f55c6a5b10100ccf62d4fd940d5920505caadb0972d1e062a9c7`
- Signature scheme: APK Signature Scheme v3
- RSA key size: 2048 bits

Android only accepts an in-place update when it is signed consistently with the installed app. Users should download releases from the official LittleLoopLab repository and compare the published SHA-256 value when verification is important.

## Independent checks

The APK itself is public and can be inspected without access to the private project source. A technical reviewer can independently:

- extract and inspect the compiled Android manifest;
- confirm the absence of the Internet permission;
- list declared permissions and application components;
- inspect the Accessibility service configuration;
- verify the signing certificate and APK signature;
- calculate the APK SHA-256 hash;
- inspect packaged assets and native libraries; and
- perform static or dynamic analysis of the compiled application.

Example checks with Android SDK tools:

```bash
sha256sum ChatLoop-v1.0.2.apk
apkanalyzer manifest permissions ChatLoop-v1.0.2.apk
apkanalyzer manifest print ChatLoop-v1.0.2.apk
apksigner verify --verbose --print-certs ChatLoop-v1.0.2.apk
```

Publishing source code and verifying a release binary are related but different questions: a public source repository does not by itself prove that an attached APK was built from that source. The checks above examine the file users are actually installing.

## Source and usage terms

ChatLoop is free to download and use, but its original project source is not public and the app is not open source. This is intended to prevent the existing work from being repackaged, redistributed or commercialised while keeping the official app free for users.

Security and privacy inspection of the official APK is welcome. Reproduction, repackaging, redistribution and commercial use are not permitted. See [ChatLoop Personal Use Terms](TERMS.md).

## Scope and updates

This report applies to the exact v1.0.2 APK identified above. Future releases should publish their own hash and record any permission, component or dependency changes.

ChatLoop depends on the official ChatGPT Android interface. When that interface changes, ChatLoop may require an update—as it already did to support translated ChatGPT interfaces. The supported interface languages are listed in the main README.
