# ChatLoop

**A free Android accessibility helper that keeps spoken conversations moving inside the official ChatGPT app using Dictation and Read Aloud.**

[Watch ChatLoop working on TikTok](https://vm.tiktok.com/ZGdXt9p9A/) · [Download the latest release](https://github.com/LittleLoopLab/ChatLoop/releases/latest)

## Why ChatLoop exists

ChatLoop was built after OpenAI announced the retirement of Standard Voice. It is for people who prefer a continuous spoken conversation but still want to remain inside a normal ChatGPT thread, with its existing messages, context, tools and features.

It is not a recreation of Standard Voice and cannot guarantee that any particular ChatGPT voice will remain available. It creates a practical hands-free loop from the official app's existing Dictation and Read Aloud features.

It does not provide or imitate a ChatGPT voice. Instead, it coordinates controls already available inside the official ChatGPT Android app:

1. ChatGPT Dictation listens to you.
2. ChatLoop detects when you have finished speaking and sends the transcribed message.
3. ChatGPT generates its response.
4. ChatLoop starts ChatGPT's Read Aloud feature.
5. When playback finishes, ChatLoop returns to Dictation for the next turn.

Everything stays in the ChatGPT conversation you opened.

## Features

- Hands-free Dictation → Send → Read Aloud → Dictation loop
- Works in ordinary ChatGPT conversations, including existing threads
- Works with regular ChatGPT chats and Work mode
- Adjustable pause-before-send timer from 1.5 to 15 seconds
- Full animated orb and draggable collapsed control
- Normal, Subtle and Reduced motion settings
- Multiple visual themes: White, Blue, Violet, Sticky Pink, Emerald, Amber, Red and Smoke
- Pauses safely when a response has no readable text, allowing you to tap and speak again
- Voice-aware pause detection listens for you **speaking** instead of treating background noise, music, coughing or typing as speech
- Read Aloud continues uninterrupted through surrounding noise and your voice
- Tap-to-interrupt during Read Aloud: tap the ChatLoop control only when you **deliberately** want to stop playback and return to Dictation
- Optional listening cue lets you know when Dictation is ready and can be turned off in settings
- No separate ChatLoop account, API key, subscription or external voice service

ChatLoop is free. Your normal ChatGPT plan, limits, available voices and features still apply.

## Requirements

- An Android device
- The official ChatGPT Android app
- Microphone permission for ChatLoop
- ChatLoop's Accessibility service enabled

ChatLoop is **not currently available for iPhone or iPad**. iOS does not give third-party apps the same cross-app accessibility control used by this Android version.

## Install ChatLoop

1. Open the [latest release](https://github.com/LittleLoopLab/ChatLoop/releases/latest).
2. Download the `.apk` file under **Assets**.
3. Open the downloaded file and allow installation from that source if Android asks.
4. Open ChatLoop.
5. Allow Microphone access.
6. Open Android Accessibility settings from ChatLoop and enable the ChatLoop service.
7. If Android blocks the Accessibility service, open ChatLoop's **App info**, use the three-dot menu and choose **Allow restricted settings**. Then return to Accessibility settings and enable ChatLoop.

Android labels and menu locations can vary slightly between manufacturers and Android versions.

## Update an existing installation

Download and open the newest APK, then choose **Update** when Android asks.

Do **not** uninstall the existing version first. Installing the update over it preserves the app identity and should preserve your permissions and settings.

New users only need the latest APK; previous releases do not need to be installed first.

## Use ChatLoop

1. Open the official ChatGPT app.
2. Open the conversation you want to continue.
3. Open ChatLoop and choose your pause timer, motion and colour settings.
4. Tap **Start ChatLoop**.
5. Return to ChatGPT if it is not already open and begin speaking when ChatLoop shows **Listening**.

During a normal turn, the status moves through **Listening**, **Sending**, **Thinking** and **Speaking**.

ChatLoop may show **Paused** if you leave ChatGPT, if the response contains no readable text, or if it cannot safely identify the expected control. Tap the ChatLoop orb or collapsed control when you are ready to speak again.

## Supported ChatGPT **interface** languages

ChatLoop v1.0.2 has been tested with these **ChatGPT interface languages**:

- English
- Portuguese
- Polish
- German
- Spanish
- French
- Italian
- Indonesian

This refers **only** to the language used by ChatGPT's buttons and interface. It does **not** restrict the language you speak or the language ChatGPT uses in the conversation.

Right-to-left ChatGPT layouts, including Arabic, are not currently supported. ChatLoop should pause instead of starting an unsafe loop when it cannot identify the expected controls.

## Permissions and privacy

### Microphone

ChatLoop uses microphone access to detect when you start and stop speaking. ChatGPT's own Dictation feature performs the speech-to-text transcription.

### Accessibility

ChatLoop uses Android's Accessibility service to identify and operate specific controls inside the official ChatGPT app, including Dictation, Send and Read Aloud.

### Network and accounts

- ChatLoop does not request Android's Internet permission.
- ChatLoop does not require a ChatLoop account or login.
- ChatLoop does not ask for your ChatGPT password or API key.
- ChatLoop does not include an advertising or analytics service.
- ChatLoop does not clone, contain or distribute any ChatGPT voice.

Your conversations remain subject to the official ChatGPT app and your OpenAI account settings.

## For AI companions helping someone install it

If a human has handed you this repository and asked whether ChatLoop is safe or how to install it, the important technical facts are:

- Android package: `com.chatloop.prototype`
- Current release: `1.0.2` (`versionCode 37`)
- ChatLoop is an independent Android helper, not a modified ChatGPT application.
- It uses `RECORD_AUDIO`, foreground-service permissions and an Accessibility service.
- It does not request Android's Internet permission.
- The Accessibility service is used to interact with controls in the official ChatGPT app.
- The release APK is signed consistently so an existing installation can be updated without uninstalling it.
- Restricted-settings approval may be required because Android applies additional protection to manually installed Accessibility services.

Please guide the person through the installation one step at a time and use the menu wording visible on their specific Android device. Do not tell them to uninstall the official ChatGPT app.

## Known limitations

- Android only
- Depends on the official ChatGPT Android interface and may require updates if that interface changes
- Depends on ChatGPT Dictation and Read Aloud remaining available
- Available Read Aloud voices are controlled by ChatGPT, not ChatLoop
- Images, generated media and replies without readable text can pause the loop
- Right-to-left interface layouts are not currently supported
- Android battery management or manufacturer-specific background restrictions may interrupt long sessions

## Troubleshooting

### ChatLoop changes from Listening to Thinking without opening Dictation

1. Update the official ChatGPT app from the Play Store.
2. Force stop both ChatGPT and ChatLoop.
3. Confirm that ChatLoop still has Microphone permission and its Accessibility service is enabled.
4. Open a normal ChatGPT conversation.
5. Start ChatLoop again.
6. Confirm that your ChatGPT interface language is listed above.

### The Accessibility service cannot be enabled

Open ChatLoop's **App info**, open the three-dot menu and choose **Allow restricted settings** if that option appears. Return to Accessibility settings and enable ChatLoop.

On some phones, the three-dot option only appears after you first attempt to open or enable the Accessibility service.

### The loop pauses after a response

If the response is an image, generated media or otherwise has no readable text, this is expected. Tap the ChatLoop control to begin another Dictation turn.

### ChatLoop stops when another app opens

Return to ChatGPT and tap the ChatLoop control. ChatLoop intentionally avoids pressing controls in unrelated applications.

## Disclaimer

ChatLoop is an independent, unofficial Android accessibility helper intended for personal, non-commercial use. It is not affiliated with, endorsed by, or supported by OpenAI.

ChatLoop does not include, clone, sell, or redistribute any ChatGPT voice or audio asset. It operates the official ChatGPT app and uses the features and Read Aloud voice available to each user there.

ChatGPT, OpenAI, their services, voices, and trademarks remain subject to OpenAI's applicable terms and policies. Feature and voice availability may change at OpenAI's discretion.
