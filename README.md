[[README.md](https://github.com/user-attachments/files/33088277/README.md)
# Veil Messenger

**End-to-end encrypted messaging over ordinary SMS and MMS.**

**Version:** 1.0.0 (build 1)  
**Package:** `com.veil.messenger`  
**Minimum Android:** Android 8.0 (API 26)  
**Target Android:** Android 15 (API 35)

---

## What is Veil?

Veil is an Android messaging application designed to provide end-to-end encrypted messaging while using the normal cellular SMS/MMS network.

Veil uses the **Signal Protocol** for message encryption and can operate as the phone's default SMS application.

There is **no account or sign-up process**, and the application does **not request the Android `INTERNET` permission**. Key exchange and encrypted message transport are performed through the SMS/MMS system.

When both participants use Veil, the message content is encrypted before it is sent through the carrier. The carrier still transports the message, but the protected message content is presented as ciphertext rather than readable plaintext.

If the recipient does not use Veil, the conversation can continue as a normal SMS/MMS conversation. This allows Veil to function as a replacement for a conventional messaging application while providing secure messaging when both parties support it.

---

## Key Features

| Feature | Details |
|---|---|
| **End-to-end encryption** | Signal Protocol using the Double Ratchet and pre-key system |
| **No account or central server** | No sign-up and no central messaging server; key exchange occurs through SMS |
| **No Internet permission** | The APK does not request Android's `INTERNET` permission |
| **Default SMS/MMS app** | Sends and receives SMS/MMS and handles Android messaging intents |
| **Encrypted photos** | Attachments receive an additional AES-256-GCM encryption layer |
| **Encrypted voice notes** | Voice notes are recorded in-app and protected before transport |
| **Safety numbers** | Numeric fingerprint verification helps users confirm contact identity |
| **Identity-change warnings** | Users are warned when a contact's cryptographic identity changes |
| **Delivery/read receipts** | Sent, delivered and read states are supported |
| **Encrypted local storage** | Message database uses SQLCipher |
| **App security lock** | Veil Security Lock protects access using the device's screen lock |

---

# Installation

## Android

1. Copy `app-release.apk` to your Android device.
2. Open the APK with your browser or file manager.
3. If Android asks for permission to install applications from that source, allow it.
4. Tap **Install**.
5. Open **Veil**.
6. Grant the permissions required for messaging.
7. Set Veil as your **default SMS application** when prompted.

The supplied APK is signed using Android v1, v2 and v3 signing schemes and contains native libraries for:

- `arm64-v8a`
- `armeabi-v7a`
- `x86`
- `x86_64`

---

# Getting Started

### 1. Make Veil your default SMS application

If Veil is not the default SMS application, the home screen displays a prompt explaining that the default SMS role is required to enable secure messaging.

### 2. Start a conversation

Tap **New conversation** and enter the contact's phone number.

### 3. Start Secure Chat

Tap **Start Secure Chat**.

Veil sends the initial key-exchange message through SMS. The other Veil installation processes the handshake automatically.

### 4. Confirm the secure session

Once the secure session has been established, the conversation displays:

> `[Veil] Secure session established.`

A locked padlock indicates that the conversation is operating in secure mode.

Messages, photos and voice notes sent through the secure session are encrypted.

### 5. Verify the safety number

Open the conversation's **Security** controls and compare the displayed safety number with the contact through another communication channel, such as a phone call.

When the numbers match, select **I've verified the code**.

This provides an additional identity-verification step and helps protect against an attacker attempting to impersonate a contact.

---

# How Veil Works

Veil places its own messaging protocol around ordinary SMS text.

Veil messages use the following marker:

```text
VEIL1:
```

The marker is followed by a message type and a Base64-encoded payload.

Content without the Veil marker is handled as ordinary SMS.

## Protocol Message Types

| Marker | Type | Purpose |
|---|---|---|
| `VEIL1:H:` | Handshake | Carries the public-key bundle, including the identity key, signed pre-key and one-time pre-key |
| `VEIL1:P:` | Pre-key message | Establishes the initial encrypted session using the recipient's pre-key |
| `VEIL1:M:` | Encrypted message | Normal Double Ratchet message after a secure session exists |
| `VEIL1:R:` | Receipt | Carries delivery/read confirmation information |

### Attachments

Photos and voice notes receive a separate **AES-256-GCM** encryption layer.

The one-time attachment key is sent inside an already encrypted Veil message. This means the carrier does not receive the attachment key as ordinary plaintext.

Attachments are transported using MMS.

---

# Encryption

Veil's documented cryptographic architecture includes:

### Messages

**Signal Protocol**

The messaging layer uses the Signal Protocol, including:

- Identity keys
- Signed pre-keys
- One-time pre-keys
- Pre-key session establishment
- Double Ratchet messaging

### Attachments

**AES-256-GCM**

Photos and voice notes receive an additional encryption layer before being transported through MMS.

### Fingerprints

**SHA-256**

Safety numbers/fingerprints are derived for identity verification.

### Local database

**SQLCipher**

The local message database is encrypted.

The database passphrase is stored using Android encrypted preferences.

---

# SMS/MMS Compatibility

Veil is designed to operate as a normal Android SMS/MMS application as well as a secure messaging application.

It supports Android messaging functions including:

- SMS
- MMS
- `sms:`
- `smsto:`
- `mms:`
- `mmsto:`
- Respond-via-message integration

The application contains the Android messaging components required for default SMS operation, including:

```text
SmsDeliverReceiver
MmsDeliverReceiver
HeadlessSmsSendService
```

---

# Secure vs. Regular Messages

Veil has two distinct messaging states.

## Secure Veil conversation

When both participants have Veil and a secure session has been established:

- Message content is encrypted.
- Signal Protocol protection is used.
- Attachments receive additional AES-256-GCM protection.
- The conversation displays secure-session indicators.

## Regular SMS/MMS conversation

If the recipient does not use Veil, the conversation operates as ordinary SMS/MMS.

Those messages are **not end-to-end encrypted by Veil**.

The application identifies the conversation as an ordinary SMS/MMS conversation rather than a secure Veil conversation.

### Important

Using Veil as the messaging application does **not** automatically make every SMS or MMS end-to-end encrypted.

**Both participants must use Veil and establish a secure session for Veil's end-to-end encryption to apply.**

---

# What Veil Protects

According to the supplied build documentation, Veil is designed to protect:

### Message content

Messages between two Veil users are protected using the Signal Protocol.

### Attachments

Photos and voice notes receive an additional AES-256-GCM encryption layer.

### Local message storage

The message database is encrypted using SQLCipher.

### Application backup

Android automatic backup is disabled so application keys are not copied to cloud backup.

### Network access

The application does not request the Android `INTERNET` permission.

Cleartext network traffic is also disabled.

---

# What Veil Cannot Hide

Veil protects message content, but SMS/MMS still exposes metadata to the cellular carrier.

Depending on the carrier and network, information such as the following can remain visible:

- Who you are communicating with
- When messages are sent
- Message routing information
- Approximate message size
- Cellular delivery information

This is an important limitation of using SMS/MMS as the underlying transport.

**Veil encrypts the message content; it does not make the cellular SMS/MMS network anonymous.**

---

# Identity Changes

Reinstalling Veil or clearing the application's data creates a new cryptographic identity.

When this happens, contacts are warned that the identity has changed.

A changed identity should be treated as requiring verification.

Users should compare the new safety number with their contact before continuing sensitive communications.

---

# Delivery Receipts

Veil uses visual indicators for message status:

- **One check:** Sent
- **Two checks:** Delivered
- **Blue indicator:** Read

Receipt information is handled by the Veil protocol.

---

# Permissions

| Permission | Purpose |
|---|---|
| `SEND_SMS` | Send SMS messages and secure-session handshakes |
| `RECEIVE_SMS` | Receive SMS messages |
| `RECEIVE_MMS` | Receive MMS messages |
| `RECEIVE_WAP_PUSH` | Receive WAP push/MMS delivery messages |
| `READ_SMS` | Read existing messages from the Android SMS store |
| `READ_CONTACTS` | Display contact names instead of only phone numbers |
| `RECORD_AUDIO` | Record voice notes |
| `POST_NOTIFICATIONS` | Display new-message notifications on Android 13+ |

### Internet access

Veil does **not** request:

```text
android.permission.INTERNET
```

This is an intentional part of the application's architecture and means the application is not designed to communicate with a conventional Internet-based messaging server.

---

# Privacy Notes

Veil's design is intended to keep the encrypted messaging process between participating phones while using the carrier as the transport layer.

This architecture provides an important distinction:

**The carrier transports the messages, but secure Veil message content is encrypted before carrier transmission.**

The carrier can still observe the normal metadata associated with SMS/MMS.

For this reason, Veil should be understood as a **privacy and encryption layer over SMS/MMS**, not as an anonymous cellular communications system.

---

# Security Verification

For sensitive communications, users should verify the identity of contacts using Veil's safety-number system.

A recommended verification process is:

1. Open the secure conversation.
2. Open the **Security** section.
3. Display the contact's safety number.
4. Compare it with the contact using a separate trusted communication channel.
5. Confirm that both numbers match.
6. Mark the contact as verified.

If Veil reports an identity change, stop and verify the new identity before continuing sensitive communication.

---

# Technical Details

| Item | Value |
|---|---|
| **Application** | Veil |
| **Package** | `com.veil.messenger` |
| **Version** | 1.0.0 |
| **Version code** | 1 |
| **Minimum SDK** | Android 8.0 / API 26 |
| **Target SDK** | Android 15 / API 35 |
| **Language** | Kotlin |
| **UI** | Jetpack Compose + Material 3 |
| **Navigation** | Navigation Compose |
| **Messaging encryption** | Signal Protocol |
| **Attachment encryption** | AES-256-GCM |
| **Fingerprint hashing** | SHA-256 |
| **Database** | Room + SQLCipher |
| **Secure preferences** | Android EncryptedSharedPreferences |
| **SMS component** | `SmsDeliverReceiver` |
| **MMS component** | `MmsDeliverReceiver` |
| **Default SMS service** | `HeadlessSmsSendService` |
| **Native ABIs** | arm64-v8a, armeabi-v7a, x86, x86_64 |
| **APK size** | Approximately 37.5 MB |
| **APK signatures** | v1, v2 and v3 |

---

# Compatibility

Veil requires a device capable of providing cellular SMS/MMS service.

A SIM and carrier plan supporting SMS/MMS are required for normal messaging.

Carrier charges for SMS/MMS may apply.

The application is designed for Android 8.0 and newer, with the supplied build targeting Android 15.

---

# Troubleshooting

## Veil says it is not the default SMS app

Open Android's default-app settings and select Veil as the default SMS application.

The secure messaging functionality depends on Veil being able to operate as the device's SMS application.

## Secure Chat cannot be established

Check that:

1. Both users have Veil installed.
2. Both users have SMS service.
3. Both applications are able to send and receive SMS.
4. The initial handshake was delivered.
5. Neither application has had its data cleared or identity replaced unexpectedly.

## A contact's identity changed

Do not automatically accept the change.

Verify the contact's new safety number through another trusted communication channel.

## Messages are being sent as ordinary SMS

Confirm that:

- The recipient is using Veil.
- A secure session has been established.
- The conversation displays the secure-session indicator.
- The message is not being sent through a regular SMS/MMS thread.

---

# Important Security Statement

Veil's secure messaging architecture is designed to provide end-to-end encryption between participating Veil users.

However, no application should be considered secure solely because it uses the Signal Protocol or another recognized cryptographic library.

Users and developers should distinguish between:

- **The cryptographic design**
- **The implementation**
- **The device security**
- **The Android operating system**
- **The cellular transport**
- **Metadata protection**

Veil's encryption protects the contents of secure Veil messages. It does not eliminate SMS/MMS metadata visible to the carrier, and ordinary SMS/MMS conversations with non-Veil users remain unencrypted.

---

# Project Information

**Application:** Veil Messenger  
**Package:** `com.veil.messenger`  
**Version:** 1.0.0 (build 1)

Questions, bug reports and feedback can be sent to:

**thecanadianfrost@protonmail.com**

---

## Documentation Basis

This README reflects the supplied Veil 1.0.0 release documentation and APK inspection.

The documented behavior applies specifically to **Veil 1.0.0 (build 1)** and may change in later releases.

