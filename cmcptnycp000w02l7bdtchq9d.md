---
title: "Is YONO SBI Safe? Understanding the CVE-2025-45080 Vulnerability"
seoTitle: "Is SBI YONO Safe? Understanding the CVE-2025-45080 Vulnerability"
seoDescription: "Understanding the CVE-2025-45080 Vulnerability"
datePublished: Sat Jul 05 2025 05:47:59 GMT+0000 (Coordinated Universal Time)
cuid: cmcptnycp000w02l7bdtchq9d
slug: is-yono-sbi-safe
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751694336101/3eb391d4-c0fe-4d0b-8063-b80d3c0d78ef.png
tags: security, hacking, banking, secure-coding

---

A new entry in the global cybersecurity logs, CVE-2025-45080, has recently ignited a fascinating and important debate. The subject is an old version of the State Bank of India's (SBI) widely-used YONO banking application. But the controversy isn't about a massive data breach or an active, raging cyberattack. Instead, it’s about a ghost in the machine: a theoretical flaw in outdated software, and the public relations storm that followed.

The assignment of this CVE led many to question the entire process. How can a vulnerability be declared for an app version from 2021 that is no longer on the official Play Store? Does this represent a flaw in the CVE system itself? Just as these questions began to circulate, SBI issued a public statement, adding a new, complex layer to the story.

To understand the full picture, we need to peel back the layers of this case—from the technical specifics of the flaw and the philosophy of vulnerability management to the corporate response it triggered.

#### **Chapter 1: Deconstructing the Vulnerability**

To understand the debate, we must first understand the flaw itself. It’s not a complex piece of malware, but a single, critical line of code within the app's core configuration.

**The Blueprint of an App:** `AndroidManifest.xml`

Every Android application has a file named `AndroidManifest.xml`. Think of this as the app's passport. It declares everything the app is, what it needs, and what it's allowed to do. It tells the Android operating system what permissions the app requires and is the fundamental contract between the app and the device.

**The Flaw: A Deliberate Downgrade in Security**

Within this manifest file for YONO SBI version 1.23.36, researchers found the following attribute: `android:usesCleartextTraffic="true"`

This line is a specific instruction to the Android OS. "Cleartext traffic" refers to data transmitted over unencrypted HTTP protocol. To grasp the severity of this, consider an analogy:

* **HTTPS (Secure)** is like sending a sensitive document inside a locked, steel briefcase via an armoured van. Only the sender and the intended recipient have the key.
    
* **HTTP (Cleartext)** is like writing that same sensitive information on a postcard and dropping it in the mail. Anyone who handles the postcard can read its contents.
    

Recognizing this inherent danger, Google made a crucial security change starting with Android 9 (API level 28), making the default for `usesCleartextTraffic` to be `false`. The YONO app version in question deliberately opted out of this modern, fundamental security protection. This is formally classified as **CWE-319: Cleartext Transmission of Sensitive Information**, a well-understood and critical weakness.

**The "**[**Proof of Concept**](https://pastebin.com/z2gmGWP9)**" (PoC)**

The proof for this CVE was not a video of a live hack. The PoC was the result of static analysis—decompiling the app's installation file (the APK) and presenting this line of code as irrefutable evidence. In the world of vulnerability research, this is a perfectly valid form of proof.

#### **Chapter 2: The Why Behind the CVE - A Librarian's Logic**

The biggest point of contention is why this issue warrants a CVE at all, given its historical nature. This stems from a misunderstanding of the role of MITRE and the CVE program.

**MITRE's Role: A Library, Not a Courtroom**

The CVE program is not designed to be a judge that weighs the immediate, real-world risk of a flaw. It is a library. Its purpose is to catalog every publicly known, unique software vulnerability and assign it a standard identifier. When a researcher discovers a flaw, they report it to a CVE Numbering Authority (CNA), which verifies the flaw is real and hasn't been previously cataloged. The existence of the vulnerability itself is sufficient for a CVE.

**The Importance of Cataloging Historical Flaws**

Cataloging a flaw in an app from 2021 is crucial for:

* **Corporate & Government IT:** Teams managing devices that may be running older software need to know about all documented vulnerabilities.
    
* **Digital Forensics:** Investigators need a historical record of all possible weaknesses that could have been exploited in a breach.
    
* **The "Long Tail" of Technology:** Users of older devices or those who restore from old backups remain at risk from historical flaws.
    

#### **Chapter 3: The Corporate Response – SBI Enters the Fray**

As the CVE report gained traction on social media, the State Bank of India intervened with an official statement to manage the narrative and reassure its customers.

In their public post, SBI clarified its position:

> "The referred version (1.23.36) is an obsolete version which was released in April 2021 and is not available on Playstore / Appstore.
> 
> Current version of YONO App is 1.24.24 which is safe and secure and no such vulnerabilities are found in the current version.
> 
> We request you to refrain from posting such incorrect information on Social Media."

This statement is a textbook example of corporate crisis communication. SBI both validates the core facts of the report (the old version number and its obsolete status) while simultaneously reframing the narrative. By calling the circulation of this news "incorrect information," they are not disputing the technical reality of the past flaw. Instead, they are combatting the **implication** that their current, secure application is at risk, aiming to prevent unnecessary panic among their millions of users.

#### **Chapter 4: The Synthesis and Takeaways for Everyone**

This case leaves us with two truths existing in parallel: The security researchers and the CVE program were correct in identifying and cataloging a historical flaw according to global standards. At the same time, SBI was correct in reassuring the public about the safety of its current product.

This incident offers critical lessons for different groups:

**For the Average YONO SBI User:** SBI's statement provides the most important takeaway: there is no need to panic if your app is updated. This serves as a powerful reminder of three golden rules of digital hygiene:

1. **Always update your apps** from official sources.
    
2. **Only download from the official Google Play Store or Apple App Store.**
    
3. **Be cautious on public Wi-Fi,** especially for financial transactions.
    

**For Application Developers:** This is a stark reminder that security is not a feature to be added later.

* **Secure by Default:** Adhere to the latest security best practices. Overriding a secure default requires immense justification.
    
* **Code is a Permanent Record:** Anything you release to the public can be analyzed, even years later.
    
* **Own Your Vulnerabilities:** While SBI's response aimed to control the narrative, it also confirmed the core details, highlighting the importance of addressing security issues head-on.
    

#### **Conclusion: A System Working as Intended**

The assignment of CVE-2025-45080 is not a sign of a broken system. On the contrary, it shows all parts of the cybersecurity ecosystem working as intended—from discovery and documentation to corporate response and public education. The debate it sparked is healthy, pushing us all to better understand the distinction between a vulnerability, a threat, and a risk. This case proves that in cybersecurity, building on a secure foundation is paramount, because every line of code has a long and very public life.