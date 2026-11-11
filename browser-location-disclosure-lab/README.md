<h1>Seeker</h1>

**Browser-Based Location Disclosure: A Controlled Seeker Link Simulation Case Study**

by Joseph Gonzalez 

<h2>Description</h2>

Mobile browsers routinely ask for sensitive permissions during ordinary web activity, especially when a site claims it needs location to personalize the experience. That creates a different exposure from credential phishing or malicious app sideloading: the user does not need to install software or type a password. The risk appears at the permission prompt.

This case study examines a controlled Seeker simulation using a temporary public tunnel and a researcher-owned phone posing as the victim device. The goal is to document what the page looked like, how the browser presented the location request, what data appeared after permission was granted, and why this matters for individual users outside the lab.

<h2>Utilities and environment</h2>

- **Utilities** — Seeker, ngrok, and a mobile web browser
- **Environment** — Kali Linux lab host and a researcher-owned Android phone

<details>
<summary>Study scope and evidence</summary>

|Study detail | Description |
| --- | --- |
| Scope | Controlled lab simulation using a researcher-owned phone posing as the victim device; educational and analytical purposes only. |
| Environment | Kali Linux, Seeker, ngrok tunnel, mobile browser, researcher-owned Android phone, redacted public URL/IP/location artifacts. |
| Primary Evidence | Seeker launch screenshot, ngrok tunnel screenshot, request listener screenshot, mobile location-permission screenshot, redacted terminal capture of device/location fields. |
| Primary Attack Vector | Permission-based disclosure through a location-themed public link that persuades the user to approve browser location access. |

*Educational and analytical use only. Public URL, IP, device, and location details shown in figures are redacted.*

</details>

<h2>Lab walk-through</h2>

## Methodology

A controlled cybersecurity simulation was conducted in an isolated lab using Kali Linux, Seeker, an ngrok tunnel, and a mobile browser on a researcher-owned Android phone. The scope was limited to self-controlled infrastructure, a temporary test link, and consent-based interaction by the researcher. No real victims, third-party devices, production accounts, or unauthorized tracking were involved.

## 1. Overview

This scenario shows how a web page can request location access and record browser-provided device and network details once the user grants permission. The analytical question was whether a believable location-themed page, delivered through a temporary public tunnel, could produce observable location and device data from a phone that voluntarily accepted the browser prompt.

## 2. Threat Analysis: Vector, Technique, and Human Factor

| Aspect | Analysis |
| --- | --- |
| Attack vector used | A public link to a location-themed web page that requests browser location access and records returned device, network, and location fields after approval. |
| Social engineering techniques | Permission phishing, pretexting, contextual deception, link-based lure, and origin inattention. |
| Human factor exploited | Mobile prompt fatigue, task fixation, trust in the page theme, focus on large approval buttons, and reduced attention to temporary or unfamiliar domains. |
| Defensive relevance | Location prompts should be treated as security decisions. Users should deny unfamiliar requests, verify the site origin, review saved permissions, and revoke access immediately after a suspicious approval. |

**Key reader takeaway:** This is not a silent operating-system compromise. The browser did ask. The exposure happened because the page made the request feel normal enough for the user to approve.

## 3. Lab Setup and Public Link Delivery

The lab host was used for the Seeker environment and a location-themed test page. Figure 1 identifies Seeker on the lab host. Figure 2 shows the temporary tunnel online before any connections were recorded.

<p align="center">
  <b>Figure 1.</b> Seeker in the lab console<br />
  <a href="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/seeker-console.png">
    <img src="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/seeker-console.png" width="420" alt="The terminal displays the Seeker banner." />
  </a>
</p>

<p align="center">
  <b>Figure 2.</b> The tunnel before the phone visited<br />
  <a href="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/tunnel-session-status.png">
    <img src="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/tunnel-session-status.png" width="614" alt="The ngrok status screen reports an online session and zero connections." />
  </a>
</p>

## 4. Permission Prompt and Evidence Collected

After the public test link was prepared, the phone opened the location-themed page as the simulated victim device. Figure 3 shows request activity in the tunnel session after the page was visited. Figure 4 shows the mobile browser displaying a location permission request for the site. This is the key decision point: the exposure depends on the user granting access, not on an invisible bypass of the browser permission model.

<p align="center">
  <b>Figure 3.</b> Requests when the test page opened<br />
  <a href="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/http-requests.png">
    <img src="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/http-requests.png" width="635" alt="The request log lists successful GET and POST requests." />
  </a>
</p>

The mobile browser then displayed a location permission request for the site. The phone offered three choices: “Allow while visiting the site,” “Allow this time,” and “Never allow.” The researcher approved access during this test.

<p align="center">
  <b>Figure 4.</b> The decision on the phone<br />
  <a href="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/location-permission.png">
    <img src="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/location-permission.png" width="565" alt="The browser asks to use the device’s location and offers Allow while visiting the site, Allow this time, and Never allow." />
  </a>
</p>

Additional terminal evidence appears with the next artifact. Redactions remove private IP, URL, and location details while preserving the forensic value of the test. The recorded fields include latitude, longitude, and a reported horizontal accuracy of about 13.84 metres. The researcher did not independently measure the phone’s position or test which device or network fields would also appear after denial.

<p align="center">
  <b>Figure 5.</b> What the lab host recorded<br />
  <a href="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/recorded-fields.png">
    <img src="https://raw.githubusercontent.com/Jectes/Jectes/main/lab-images/recorded-fields.png" width="405" alt="The terminal lists device, network, and location fields, including a location accuracy estimate. Identifying values are covered." />
  </a>
</p>

## 5. Why the Disclosure Occurred

The disclosure occurred because the site asked for a permission that browsers are designed to support, and the user granted it. The near-you theme made the request feel contextually appropriate: a page claiming to show nearby people naturally appears to need location access.

The browser did show the site origin, but users often focus on the task language and the large approval buttons rather than verifying the temporary domain. The tunnel also mattered because it turned a local service into a reachable public page, allowing the phone to interact with the lab-hosted content from outside the localhost context.

## 6. Real-World Impact to the Individual

For an individual user, granting location to a deceptive page can expose more than a single data point. Location can reveal where someone lives, works, studies, worships, or spends time with family. When combined with public IP address, device type, browser version, and network provider information, it can support targeted phishing, harassment, doxxing, stalking, or highly personal follow-up lures.

The risk is serious on phones because people treat mobile permission prompts as routine. A user may approve location while distracted, then forget that the site ever asked. Even without malware, the collected data can help an attacker build a profile, time future messages, or craft a more believable follow-up.

## 7. Mitigation, Hardening, and Prevention

The practical fix is to reduce unnecessary location grants and treat permission prompts as security decisions. Users should deny location requests from unfamiliar links, avoid opening location-themed links from messages or social media, and check the browser address bar before approving access.

If a suspicious location prompt was approved, the user should revoke the site permission, clear the site data, close the tab, and avoid interacting with the page again. Long-term prevention includes updating the browser and operating system, preferring official apps and known domains, and disabling precise location when it is not needed.

## 8. Reader Preparation Checklist

| When | What to do |
| --- | --- |
| Before approving location | Verify the domain, ask why the site needs exact location, and deny requests from unfamiliar links by default. |
| After a suspicious approval | Revoke the site permission, clear site data, close the tab, and avoid using the page again. |
| Long-term habit | Review browser permissions regularly, reduce precise-location sharing, and teach family members that *Allow this time* still releases sensitive data. |

## 9. Ethical Consideration

This case study was conducted strictly for educational and analytical purposes. The site, tunnel, terminal session, and phone interaction were operated by the researcher in a bounded test scenario. The phone used in the screenshots intentionally acted as the victim device, and sensitive URLs, IP details, and location values were redacted before the report was prepared.

The defensive lesson is straightforward: browser permission prompts are not harmless pop-ups. They are trust decisions. When a site asks for location, the safest habit is to pause, verify the origin, and deny the request unless the need is legitimate and expected.
