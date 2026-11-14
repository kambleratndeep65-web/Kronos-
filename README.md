# KRONOS: Android voice assistant (Kotlin + Jetpack Compose)

**Status: source project complete, NOT yet compiled.** It was written in a sandbox without the Android SDK,
so no APK has been built and nothing here has been run on a phone. See "What is verified" below.

## Get the APK (no coding, about 10 minutes)

The project includes a GitHub Actions workflow that runs the unit tests, builds the debug APK and publishes it
as a GitHub Release with a direct download link.

1. Create a free GitHub account and a new **public** repository (public so the release link works without login).
2. Upload this whole folder (the `.github` folder and `gradlew` must be included). Easiest: unzip it, then in the
   repository choose *Add file > Upload files* and drag everything in. Commit to `main`.
3. Open the **Actions** tab, wait for "Build KRONOS APK" to turn green (about 5 to 8 minutes).
4. Open **Releases** on the repository page and tap `app-debug.apk` to download.
   If the build is red, open the failed step and send me the error text; I will fix it.

Alternative: open the folder in Android Studio and run *Build > Build APK(s)*.

## Install on your phone

1. Download the APK on the phone. Android will ask to allow installs from your browser/Files app; allow it for this once.
2. Open the APK, tap Install. If Play Protect warns about an unknown app, that is expected for a self-built debug APK.
3. Open KRONOS, tap **Settings**, choose an AI provider, create a free key at the site shown, paste it, tap **Test AI connection**.
4. Tap the orb and speak. Say "Open YouTube", "What time is it?", "आज की तारीख क्या है", or ask anything.

## Free AI (researched 14 Nov 2026)

KRONOS ships with no key. Providers with a no-credit-card free tier (per published comparisons; verify before relying on them):

| Provider | Free limits (stated) | Privacy note |
|---|---|---|
| Groq | ~30 requests/min, ~1,000/day | Not used for training per comparison tables |
| Google Gemini | Free tier on Flash models; numeric limits not published on the pricing page | Free-tier prompts may be used to improve Google products |
| OpenRouter free models | ~20 requests/min, ~50/day without a top-up | Varies by model |

Model names change often; they are editable in Settings. The default model IDs have **not** been checked against live APIs.

## Features and honest limits

| Feature | How it works | Limit |
|---|---|---|
| Voice input | Android SpeechRecognizer (Hindi, English, or both) | Usually sends audio to Google unless an offline pack is installed. This is recognition, not the AI brain. |
| AI brain | OpenAI-compatible chat call to the provider you choose | Needs internet and your own free key; free tiers have limits |
| Spoken replies | Android TextToSpeech, Hindi or English voice by script | Needs the voice data installed on the phone |
| Continuous chat | Re-listens after each reply; stops after 2 silent rounds | |
| "Hello KRONOS" | Optional foreground service loops the recognizer and matches the phrase | Android reserves true hotword for the system assistant. Uses battery, shows a notification, may be killed by the phone's battery manager, and recognizers can beep. |
| Open apps | Matches app names, launches directly | Only while KRONOS is on screen; otherwise a tap-to-open notification (Android restriction) |
| Time and date | Local, instant, no AI | |
| Notifications | Reads the live notification list on request | Needs notification access. Nothing stored or uploaded. OTP messages are never read aloud. |
| Downloads | Lists Downloads, flags risky names | Needs All files access. **Filename heuristics only, not an antivirus.** Snapshot on request, not a live watcher. |
| Delete a file | Only after you say yes (or tap YES) | Only files directly in Downloads |
| Messages, payments | Not implemented | By design |

Privacy: no audio is recorded or stored by KRONOS. Chat text is sent to your chosen AI provider only when a
request goes to the AI. Time, date, apps, notifications and downloads are handled locally and never sent to the AI.
The API key is stored in the app's private storage on the phone.

## What is verified

- Written and reviewed by reading only. **Never compiled. Never run.**
- Unit tests exist (`app/src/test`) for command routing, wake-phrase matching, language detection, download-risk
  heuristics, spoken date/time text, app-name matching and AI request/response parsing. **They have not been run.**
- **Not tested at all:** Hindi and English speech recognition, AI connectivity with any provider, spoken replies,
  the UI on a real screen, the wake service, notification reading, downloads scan, running on your phone model.

Target device: model V2545. I could not confirm its Android version, so `minSdk` is 30 (Android 11) and `targetSdk` is 35.
