[README.md](https://github.com/user-attachments/files/33087922/README.md)
# Vail
A encrypted sms app that is safer then most. In beta .. I am improving this consistantly . 
# Veil Messenger

Veil Messenger is an Android SMS/MMS messaging application designed to
provide a privacy-focused messaging experience while retaining
compatibility with the cellular SMS/MMS system.

This README describes the supplied APK build (`app-release.apk`, version
`1.0.0`) based on an inspection of the packaged application.

## Overview

Veil Messenger is intended to function as a full Android messaging
application rather than simply a text-encryption utility.

The supplied APK contains Android SMS/MMS components for:

-   Sending SMS messages
-   Receiving SMS messages
-   Receiving MMS/WAP push messages
-   Sending messages through Android's SMS/MMS mechanisms
-   Handling SMS delivery events
-   Handling MMS delivery events
-   Responding to messaging intents such as `SEND`, `SENDTO`, and
    `RESPOND_VIA_MESSAGE`
-   Reading contacts and SMS data
-   Recording audio for messaging-related functionality

The application package is:

`com.veil.messenger`

The main application activity is:

`com.veil.messenger.MainActivity`

The application class is:

`com.veil.messenger.VeilApplication`

## Privacy and Security

Veil Messenger includes several security-related components in the
supplied APK.

Notable components detected in the build include:

-   SQLCipher native libraries for encrypted SQLite database support
-   An `EnhancedEncryption` component
-   Fingerprint/key-related components
-   `FingerprintProtocol.proto`
-   `WhisperTextProtocol.proto`
-   `LocalStorageProtocol.proto`

These components indicate that the application contains infrastructure
for encrypted local storage and enhanced/encrypted messaging.

**Important:** The presence of encryption-related code or libraries does
not, by itself, prove that every message is encrypted end-to-end in
every situation. SMS and MMS sent through a cellular carrier remain
subject to the normal limitations of those carrier protocols. The actual
encrypted messaging protocol should be independently tested and audited
before making strong security guarantees.

## SMS/MMS Compatibility

Veil Messenger contains the Android components normally required for an
SMS/MMS application, including:

-   `SmsDeliverReceiver`
-   `MmsDeliverReceiver`
-   `HeadlessSmsSendService`

The manifest also declares Android SMS/MMS permissions and messaging
intent filters.

This allows the application to participate in the Android messaging
system and, where Android permits it, operate as the user's default SMS
application.

### Carrier SMS/MMS

When a message is sent as ordinary carrier SMS/MMS, the message travels
through the cellular messaging infrastructure.

Veil Messenger cannot change the fundamental transport characteristics
of carrier SMS/MMS simply by being the application used to compose the
message.

For privacy-sensitive communication, the recipient must support the
application's encrypted messaging mechanism if the message is to remain
protected beyond the normal SMS/MMS transport.

## Permissions

The supplied APK requests permissions associated with messaging and
communications, including:

-   `READ_SMS` --- access SMS data
-   `SEND_SMS` --- send SMS messages
-   `RECEIVE_SMS` --- receive SMS messages
-   `RECEIVE_MMS` --- receive MMS messages
-   `RECEIVE_WAP_PUSH` --- receive WAP push/MMS delivery messages
-   `BROADCAST_SMS` --- SMS broadcast handling
-   `BROADCAST_WAP_PUSH` --- WAP push broadcast handling
-   `SEND_RESPOND_VIA_MESSAGE` --- respond through Android's messaging
    integration
-   `READ_CONTACTS` --- access contacts
-   `RECORD_AUDIO` --- microphone/audio functionality
-   `POST_NOTIFICATIONS` --- display notifications on supported Android
    versions
-   `DUMP` --- declared by the supplied build

Only grant permissions that are necessary for the functions you intend
to use. Android may also restrict some permissions or messaging
capabilities depending on the device and whether Veil is configured as
the default SMS application.

## Installation

### Android

1.  Transfer `app-release.apk` to the Android device.
2.  Open the APK using a file manager or browser.
3.  If Android asks for permission to install apps from that source,
    allow the file manager/browser to install unknown applications.
4.  Install Veil Messenger.
5.  Open Veil Messenger.
6.  Grant only the permissions required for the messaging features you
    intend to use.
7.  If you want Veil to handle normal SMS/MMS, configure it as the
    default SMS application when Android offers that option.

### Updating an Existing Installation

Android normally requires an update APK to be signed with the same
signing identity as the existing installation.

If the signing key is different, Android may require the old application
to be uninstalled before this build can be installed. Uninstalling may
remove locally stored application data, so back up anything important
first.

## Basic Use

After installation:

1.  Launch **Veil Messenger**.
2.  Complete the initial permissions/setup requested by Android.
3.  Allow SMS/MMS access if you want Veil to manage carrier messaging.
4.  Create or select a conversation.
5.  Use the application's messaging controls to compose and send a
    message.
6.  When communicating with another Veil user, use the application's
    encrypted messaging functionality where supported.
7.  Verify security/fingerprint information when the application
    provides it.

## Security Verification

For security-sensitive use, do not rely solely on the application's name
or user interface.

A proper verification process should include:

### 1. Verify the APK

Record the cryptographic hash of the APK you install and compare it with
the hash supplied by the developer/distribution source.

Example:

``` bash
sha256sum app-release.apk
```

### 2. Verify the signing certificate

The APK should be checked to determine who signed it and whether future
releases are signed by the expected certificate.

Android's signing model uses the application signing certificate to
establish update authenticity.

### 3. Test encrypted messaging

Use two test devices and verify:

-   Whether the encrypted mode actually performs cryptographic
    encryption
-   Whether plaintext is transmitted outside the intended encrypted
    protocol
-   Whether keys are generated securely
-   Whether key/fingerprint verification works
-   Whether messages remain encrypted before transport
-   Whether message history is protected locally
-   What happens when the recipient does not support the encrypted
    protocol
-   Whether the application silently falls back to ordinary SMS/MMS

### 4. Inspect network traffic

For a serious privacy evaluation, test the application while monitoring
its network and cellular behavior.

Look for:

-   Unexpected remote connections
-   Plaintext message content
-   Metadata transmission
-   Analytics/telemetry
-   External authentication services
-   Cloud synchronization
-   Third-party SDK traffic

## Local Data Protection

The supplied APK includes a native SQLCipher library:

-   `libsqlcipher.so`

for multiple Android CPU architectures.

The build also contains local-storage protocol definitions and
security-related components.

This suggests that protected local database storage is part of the
application's architecture.

However, the existence of SQLCipher does not automatically mean that
every piece of application data is encrypted. A complete security audit
should determine:

-   Which databases use SQLCipher
-   How database keys are generated
-   Where keys are stored
-   Whether backups contain plaintext
-   Whether logs contain sensitive information
-   Whether temporary files contain message content
-   Whether notifications expose message text

## Technical Information

### Package

`com.veil.messenger`

### Version

`1.0.0`

### Main Activity

`com.veil.messenger.MainActivity`

### Application Class

`com.veil.messenger.VeilApplication`

### Messaging Components

-   `com.veil.messenger.sms.SmsDeliverReceiver`
-   `com.veil.messenger.sms.MmsDeliverReceiver`
-   `com.veil.messenger.sms.HeadlessSmsSendService`

### Notable packaged security/protocol components

-   SQLCipher
-   Enhanced Encryption
-   Fingerprint protocol
-   Whisper text protocol
-   Local storage protocol

### UI Framework

The APK contains Jetpack Compose / Material 3 components.

### Native Architectures

The supplied APK contains native libraries for:

-   ARM64 (`arm64-v8a`)
-   ARM 32-bit (`armeabi-v7a`)
-   x86
-   x86_64

## Current Status

This README documents the supplied APK build as an application package.

It is **not** a certification that the application is cryptographically
secure.

Before using Veil Messenger for high-risk communications, perform a
complete security audit covering the cryptographic implementation, key
management, fallback behavior, local storage, backups, notifications,
logs, network connections, and SMS/MMS transport.

## Known Transport Limitation

Ordinary SMS/MMS is not an end-to-end encrypted transport.

The cellular carrier can generally obtain information associated with
SMS/MMS traffic, including routing and delivery metadata, and ordinary
SMS/MMS content is not protected by the application's encryption merely
because Veil Messenger is used to send it.

Veil's encrypted messaging system should therefore be treated as a
separate communication mode and tested independently.

## Recommended Testing Checklist

Before distributing the application:

-   [ ] Install on a clean Android device
-   [ ] Test SMS sending
-   [ ] Test SMS receiving
-   [ ] Test MMS sending
-   [ ] Test MMS receiving
-   [ ] Test contact access
-   [ ] Test notification behavior
-   [ ] Test microphone-related features
-   [ ] Test encrypted messaging between two Veil installations
-   [ ] Verify fingerprints/keys
-   [ ] Test invalid or changed fingerprints
-   [ ] Test encrypted-to-non-Veil fallback
-   [ ] Confirm fallback is clearly indicated to the user
-   [ ] Inspect local database contents
-   [ ] Test application backup/restore
-   [ ] Inspect application logs
-   [ ] Inspect network traffic
-   [ ] Verify APK signing certificate
-   [ ] Record the release SHA-256 hash
-   [ ] Test on the Android versions/devices you intend to support

## Disclaimer

Veil Messenger is provided as software for testing and development.

Do not assume that a message is secure merely because it is displayed as
encrypted. Security claims should be based on verification of the actual
cryptographic implementation and communication protocol.

Android, SMS, MMS, and related trademarks and technologies belong to
their respective owners.

## License

No license information was identified from the supplied APK inspection.

If this project is distributed publicly, include an appropriate
`LICENSE` file and update this section with the applicable license
terms.

------------------------------------------------------------------------

**Build documented:** `app-release.apk`\
**Application:** Veil Messenger\
**Package:** `com.veil.messenger`\
**Version:** `1.0.0`
