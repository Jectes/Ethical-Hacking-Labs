<h1>Case 2 · AndroRAT</h1>

**Malicious App Entry to a Mobile Device: A Controlled Android APK Sideloading Simulation Case Study**

Joseph Gonzalez · ZeroDay Tech Labs

<h2>Description</h2>

Mobile devices are attractive initial-access targets because software installation often happens quickly, on a trusted personal device, and under ordinary browsing conditions. Unlike a credential-only phishing page, a malicious application package can shift the risk from a single entered password to persistent device-side exposure once the package is downloaded, approved, and opened.

This case study examines a controlled sideloading scenario. The purpose is to show how the human decision to approve an untrusted app can become the entry point, while keeping the discussion defensive and focused on what a reader should recognize and prevent.

<h2>Utilities and environment</h2>

- **Utilities** — AndroRAT, Apache HTTP Server, Android Studio, and Chrome
- **Environment** — Kali Linux lab host and a virtual Android 8 test device

<details>
<summary>Study scope and evidence</summary>

|Study detail | Description |
| --- | --- |
| Scope | Controlled lab simulation on a virtual Android 8 test device using self-operated infrastructure; educational and analytical purposes only |
| Environment | Kali Linux, AndroRAT, Android Studio emulator, local Apache delivery service, Chrome, redacted localhost/LAN artifacts |
| Primary Evidence | APK build screenshot, local server staging screenshot, listener screenshot, APK download screenshot, redacted HTTP request and separate Android SDK x86 session capture |
| Primary Attack Vector | Browser-based APK sideloading in an Android emulator. The evidence includes a download prompt, an HTTP request, and a separately captured emulator session. |

For educational and analytical purposes only. Local network details shown in figures have been redacted.

</details>

<h2>Lab walk-through</h2>

## Methodology

A controlled cybersecurity simulation was conducted in an isolated lab using Kali Linux, a local web server, AndroRAT, and an Android Studio emulator configured as a virtual Android test device. The scope was limited to self-controlled infrastructure, self-generated artifacts, and a virtual handset only. No real users, public hosts, third-party phones, or production accounts were involved.

## 1. Overview

A malicious APK entry path differs from web credential theft because the attacker is attempting to place executable code on the handset itself. In this case, the analytical question was whether a locally delivered APK would progress from download to execution and generate an observable callback inside the lab.

## 2. Threat Analysis: Vector, Technique, and Human Factor

| Aspect | Analysis |
| --- | --- |
| Attack vector used | Browser-based delivery of a sideloaded APK, followed by user approval of installation from outside the trusted app-store path. |
| Social engineering techniques | Trojan-style app delivery, pretexted download, tap-through installation, and trust exploitation through a normal-looking file-download workflow. |
| Human factor exploited | Users often treat app downloads as routine, assume small files are harmless, and approve prompts quickly when the requested action appears to match what they intended to do. |
| Defensive relevance | Reducing sideloading, using trusted app stores, checking publisher identity, reviewing permissions, keeping Play Protect enabled, and updating the device materially reduce the entry path. |

> Key reader takeaway: the dangerous moment is not only the download. The real shift happens when the user approves installation and opens the package, turning the phone from a browsing device into the platform for follow-on exposure.

## 3. Lab setup

The lab host was used to prepare the test application package and stage it through a local delivery path. Figures 1 and 2 document the package build and local server staging phases. The significance of this phase is that an apparently ordinary application file could be prepared and exposed through a browser-friendly delivery mechanism.

<p align="center">
  <b>Figure 1.</b> The signed lab package<br />
  <a href="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/package-status.png">
    <img src="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/package-status.png" width="636" alt="The terminal reports that the package was built and signed as click.apk." />
  </a>
</p>

<p align="center">
  <b>Figure 2.</b> The local web server<br />
  <a href="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/local-server-status.png">
    <img src="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/local-server-status.png" width="383" alt="Apache reports that the service is active and running on the lab host." />
  </a>
</p>

## 4. Delivery, Installation, and Callback Evidence

Before the device interaction, the listener was placed in a waiting state, as shown in Figure 3. The virtual handset then navigated to the locally hosted file and received a standard APK download prompt, shown in Figure 4. To the user, the package appeared as a downloadable application file rather than as an obvious exploit.

<p align="center">
  <b>Figure 3.</b> The receiver before the device test<br />
  <a href="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/waiting-for-connections.png">
    <img src="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/waiting-for-connections.png" width="468" alt="The AndroRAT console reads “Waiting for Connections.”" />
  </a>
</p>

<p align="center">
  <b>Figure 4.</b> The download prompt on the emulator<br />
  <a href="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/android-download-prompt.png">
    <img src="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/android-download-prompt.png" width="540" alt="The browser offers Ghosts.apk as a 2.22 MB download and displays Cancel and Download." />
  </a>
</p>

The host captured an HTTP request. Its user-agent reports Android 8.1.0, and a second header identifies a Chromium WebView shell.

<p align="center">
  <b>Figure 5.</b> An HTTP request with Android WebView headers<br />
  <a href="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/inbound-request.png">
    <img src="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/inbound-request.png" width="820" alt="The host receives an HTTP GET whose headers identify Android 8.1.0 and a Chromium WebView shell." />
  </a>
</p>

A separate terminal capture shows an Android SDK x86 session at an idle interpreter prompt. The screenshot records the session greeting. Camera access, message retrieval, and other follow-on activity are unverified.

<p align="center">
  <b>Figure 6.</b> A separately captured emulator session<br />
  <a href="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/android-emulator-session.png">
    <img src="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/android-emulator-session.png" width="467" alt="The terminal reports a shell for Android SDK built for x86 and displays an idle interpreter prompt." />
  </a>
</p>

**Evidence note.** The signed package is named `click.apk`. The download prompt names `Ghosts.apk`. The report supplies no hash linking those files or the two connection captures to one package.

## 5. Sideloading on Android 8

The result is consistent with how Android 8 handles software from outside the trusted store path. Installations from non-Play sources are governed per source, meaning a user can explicitly allow a browser or file-handling path to install unknown apps. If that approval is granted, the package installer can continue the workflow even though the application did not originate from Google Play.

The case also highlights a limit of scanning-based protection. Google Play Protect is an important security layer, but protections depend on detection, configuration, device age, and user decisions. The screenshots show a browser download prompt. The package installer’s decision and the Play Protect verdict were not recorded.

## 6. Real-World Impact to the Individual

For an individual user, a malicious app install can be more damaging than a one-time phishing page because the phone itself may become the instrument of surveillance or follow-on fraud. Depending on the application capabilities and permissions, consequences can include exposure of messages, calls, screenshots, audio, location data, device identifiers, or other personal artifacts.

The personal impact is amplified because a smartphone concentrates identity, communication, photos, banking access, cloud sessions, and recovery channels in one place. Once trust in the handset is broken, the user may need to reset accounts, rotate credentials, revoke sessions, re-enroll authentication methods, and treat the device as untrusted until it is cleaned or rebuilt.

## 7. Mitigation, Patching, and Prevention

The patch path begins with removing the entry route. Real devices should run a supported Android version, stay current on operating system and security updates, keep Google Play services and Play Protect enabled, and avoid granting install rights to browsers or file-handling paths unless there is a clearly documented need.

After a suspected malicious install, response should be immediate: disconnect the device from sensitive accounts, uninstall the application, review high-risk permissions such as accessibility or device administration access, rotate passwords beginning with email, revoke active sessions, and update the device.

## 8. Reader Preparation Checklist

| When | What to do |
| --- | --- |
| Before installing | Use trusted app stores, verify the publisher, and avoid granting install rights to browsers or file managers without a documented reason. |
| During the prompt | Treat unknown-source installation as a high-risk decision, even when the file name or download page appears familiar. |
| After a suspected install | Remove the app, review high-risk permissions, revoke sessions, rotate passwords beginning with email, and update or rebuild the device if trust is lost. |

## 9. Ethical Consideration

This case study was conducted in a controlled environment strictly for educational and analytical purposes. The delivery path, listener, and virtual device were operated by the researcher using self-controlled resources, and local network identifiers visible in the original evidence were redacted before publication. No real users, public distribution channels, or third-party devices were involved.

The defensive lesson is straightforward: reduce sideloading, keep the platform current, and treat unexpected app-install prompts as high-risk decisions rather than routine tap-through screens.
