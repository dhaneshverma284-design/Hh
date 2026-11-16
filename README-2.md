# chat DHANESH — Android starter project

Native Android app scaffold using Kotlin + Jetpack Compose. Package: `com.dhanesh.chat`.

## Current status

This is an **alpha UI prototype**, not a production messaging service. It contains a chat list, local preview chat room, community placeholder, separate AI assistant screen, and light/dark theme switch. Sending a message currently updates local preview state only. AI responses are intentionally disabled until a private AI backend is configured.

**No end-to-end encryption is currently implemented.** The lock text in the preview is a design placeholder and must not be interpreted as a security guarantee. Do not use this build for private or sensitive conversations.

## Build

Requirements: Android Studio Ladybug or newer, JDK 17+, Android SDK Platform 35, Gradle 8.9+ (Android Gradle Plugin 8.7.3).

1. Open this folder in Android Studio.
2. Allow Gradle sync and install the required SDK components.
3. Build > Build Bundle(s) / APK(s) > Build APK(s).
4. Android Studio will show the generated APK location, normally `app/build/outputs/apk/debug/app-debug.apk`.

This environment did not include the Android SDK, Gradle, or an APK build tool, so no APK could be compiled here.

## Work required before online release

1. Add a real backend (for example Firebase Authentication + Firestore, or a custom WebSocket service), security rules, abuse controls, push notifications and account recovery.
2. Integrate a maintained, independently reviewed end-to-end encryption implementation (for example Signal Protocol/libsignal) with authenticated identity keys, prekeys, session establishment, rotation, multi-device handling and safety-number/device verification. Never write a custom cryptographic protocol.
3. Encrypt attachments client-side and define secure key backup/recovery. Keep message content out of server logs, analytics and push payloads.
4. Deploy the AI service separately. The AI must receive only text the user explicitly chooses to share; it must never have direct database access to private chat history.
5. Add tests, privacy policy, data deletion, rate limits, security review and production signing.

## Suggested backend contract

- `POST /v1/auth/register`, `POST /v1/auth/login`
- `GET /v1/users/{id}/prekeys` (public key material only)
- `POST /v1/messages` (ciphertext envelope only)
- `GET /v1/conversations/{id}/messages` (ciphertext envelopes only)
- `POST /v1/ai/respond` (explicitly selected user prompt only; no chat-history access)

The backend must never receive plaintext message bodies or private keys.
