---
title: "LINUX Systems Under Attack via Printing System (CUPS)"
seoTitle: "LINUX Systems Under Attack via Printing System (CUPS)"
datePublished: Fri Sep 27 2024 10:23:02 GMT+0000 (Coordinated Universal Time)
cuid: cm1kkrbj5000709lc5xmk1vfu
slug: linux-systems-under-attack-via-printing-system-cups
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1727432203897/d6165091-fa48-4104-a267-dbd165e1b843.png
tags: linux, security, hacking

---

On September 26th, 2024, security researcher @[evilsocket](https://x.com/evilsocket) publicly reported a collection of serious vulnerabilities in the Common UNIX Printing System (CUPS). The vulnerabilities, known as [CVE-2024-47076](https://access.redhat.com/security/cve/CVE-2024-47076), [CVE-2024-47175](https://access.redhat.com/security/cve/CVE-2024-47175), [CVE-2024-47176](https://access.redhat.com/security/cve/CVE-2024-47176), and [CVE-2024-47177](https://access.redhat.com/security/cve/CVE-2024-47177), were discovered sooner than expected owing to a possible breach in the disclosure process. These weaknesses offer major security threats to any UNIX-based system that uses CUPS, allowing attackers to execute arbitrary commands and potentially obtain complete control of afflicted computers.

---

### **Overview of the Vulnerabilities**

CUPS is a popular printing solution for UNIX-based settings that facilitates communication between programs and printers. However, several components of CUPS have been found vulnerable due to insufficient input validation and poor security practices, leading to dangerous security flaws:

1. **CVE-2024-47176**: This vulnerability affects cups-browsed versions up to 2.0.1, and it binds to UDP INADDR\_ANY:631. The service allows packets from any source without verification, enabling attackers to send a Get-Printer-Attributes IPP request to an attacker-controlled URL. The estimated CVSS score is 8.6.
    
2. **CVE-2024-47076**: This vulnerability exists in libcupsfilters &lt;= 2.1b1, where the method cfGetPrinterAttributes5 fails to verify or sanitize IPP attributes from an IPP server. This enables malicious data to flow unchecked into the CUPS system. The estimated CVSS score is 8.6.
    
3. **CVE-2024-47175**: This issue affects libppd version &lt;= 2.1b1. The method ppdCreatePPDFromIPP2 fails to correctly check IPP properties before writing them to a temporary PPD file, allowing attacker-controlled data injection. The estimated CVSS score is 8.6.
    
4. **CVE-2024-47177**: The foomatic-rip component in cups-filters &lt;= 2.0.1 is vulnerable to arbitrary command execution via the FoomaticRIPCommandLine PPD argument, making it the most serious of the bunch. This bug has a critical CVSS of 9.9.
    

### **Chaining the Vulnerabilities: A Path to Remote Code Execution**

By chaining these vulnerabilities, attackers can achieve unauthenticated remote code execution on vulnerable systems. The attack sequence typically involves creating a new printer configuration (CVE-2024-47176) with a malicious IPP URL. When a print job is initiated on the victim machine, the malicious configuration triggers arbitrary command execution (CVE-2024-47177), compromising the system.

**Key Points:**

* **Unauthenticated Attack**: The attacker does not need to authenticate to exploit these flaws, making attacks easier to execute.
    
* **Trigger Requirement**: The attack only executes when a print job is started, meaning systems that rarely print may remain unexploited unless actively used.
    

### **Who is Vulnerable?**

These vulnerabilities affect a wide range of UNIX-based systems packaged with CUPS. Notably, some distributions may not enable the CUPS service by default, but if activated, they are at risk. Vulnerable distributions include:

* **Arch Linux**: Affected.
    
* **Debian**: Affected.
    
* **Red Hat / Fedora**: Vulnerable, but `cups-browsed` is disabled by default, so Not Affected.
    
* **openSUSE**: Affected.
    
* **Ubuntu**: Affected.
    

### **Why Are These Vulnerabilities Dangerous?**

These vulnerabilities are particularly dangerous because they do not require per-target research to exploit. Attackers can scan IP ranges for open UDP port 631 and use the disclosed exploit to plant a malicious printing directive (PPD) on the target machine. Once planted, the code execution payload only triggers when a print job is initiated.

**Key Risks:**

* **Remote Command Execution**: Attackers can execute arbitrary commands, potentially gaining complete control over the system.
    
* **Network Exposure**: Systems with exposed UDP port 631 are at heightened risk, especially those directly accessible from the internet.
    

### **Technical Overview and Exploitation Conditions**

Successful exploitation of these vulnerabilities relies on specific conditions:

1. **Malicious IPP Service**: The attacker must advertise a malicious Internet Printing Protocol (IPP) service that is accessible to the victim, either on the public internet or within an internal trusted network.
    
2. **cups-browsed Service Running**: The victim must have the `cups-browsed` service running, which scans for available printers. This allows the malicious IPP service to send a temporary printer definition to the victim’s system.
    
3. **Execution of Malicious Code**: Once the malicious printer configuration is accepted, the attacker can send arbitrary code, which, when the victim attempts to print from the malicious printer, will execute as the unprivileged `lp` user.
    

**Note**: Although this scenario allows remote code execution, the impact is limited by the `lp` user’s privileges, preventing escalation to higher privileges or access to secure user data.

### **Detection, Mitigation and Workarounds**

To determine if the `cups-browsed` service is running, use the following command:

```bash
$ sudo systemctl status cups-browsed
```

If the service is listed as “running” or “enabled,” check the `/etc/cups/cups-browsed.conf` file for the `BrowseRemoteProtocols` directive. If set to "cups," the system is vulnerable.

Example configuration indicating vulnerability:

```bash
BrowseRemoteProtocols dnssd cups
```

Currently, no official patches or fixed versions have been published by the upstream projects or major Linux distributions. However, you can mitigate the risks by taking the following actions:

1. **Disable and Remove the** `cups-browsed` Service: If not needed, disable the service entirely to prevent unauthorized access.
    
    ```bash
    sudo systemctl stop cups-browsed
    sudo systemctl disable cups-browsed
    ```
    
2. **Block UDP Port 631**: Restrict traffic to this port to prevent external exploitation.
    
    ```bash
    sudo ufw deny 631/udp
    ```
    
3. **Block DNS-SD Traffic**: Prevent service discovery traffic that might be exploited by attackers.
    
4. **Alternatively, Modify the Configuration to Prevent Vulnerability:**
    
    If keeping `cups-browsed` running is necessary, modify the `BrowseRemoteProtocols` directive to “none” in the configuration file and restart the service:
    
    ```bash
    BrowseRemoteProtocols none
    $ sudo systemctl restart cups-browsed
    ```
    

### **Conclusion**

The recent disclosures of these vulnerabilities in CUPS serve as a stark reminder of the importance of securing network services, especially those running with elevated privileges. As CUPS is a critical component of many UNIX-based systems, administrators must take immediate action to mitigate these vulnerabilities, even in the absence of official patches. Disabling unnecessary services, blocking exposed ports, and carefully monitoring systems for unusual activity are crucial steps in protecting your infrastructure against these critical flaws.

Stay vigilant and keep your systems updated as more information and patches become available.

### References

1. [Attacking UNIX Systems via CUPS, Part I](https://www.evilsocket.net/2024/09/26/Attacking-UNIX-systems-via-CUPS-Part-I/)
    
2. [RHSB-2024-002](https://access.redhat.com/security/vulnerabilities/RHSB-2024-002#section--Affected-products)
    
3. [CVE-2024-47076](https://access.redhat.com/security/cve/CVE-2024-47076)
    
4. [CVE-2024-47175](https://access.redhat.com/security/cve/CVE-2024-47175)
    
5. [CVE-2024-47176](https://access.redhat.com/security/cve/CVE-2024-47176)
    
6. [CVE-2024-47177](https://access.redhat.com/security/cve/CVE-2024-47177)
    

---

`Disclosure: This blog may contain AI-generated content intended to provide insights and information on the discussed topic. While efforts have been made to ensure the accuracy and clarity of the information presented, readers are encouraged to verify details with trusted sources.`