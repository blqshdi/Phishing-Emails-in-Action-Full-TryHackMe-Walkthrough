# Phishing Emails in Action — Full TryHackMe Walkthrough

## Introduction

Phishing remains one of the most common attack vectors in real-world cyberattacks. In this TryHackMe room, *Phishing Emails in Action*, we analyze multiple realistic phishing email samples to understand how attackers manipulate users into revealing sensitive information, clicking malicious links, or executing harmful attachments.

The focus of this lab is not just identifying phishing emails, but understanding the **techniques behind them**, including:

* Email spoofing and impersonation
* Link manipulation and redirection chains
* Tracking pixels for monitoring victims
* Credential harvesting pages
* Malicious attachments delivering payloads

Each task in this room demonstrates a different phishing technique used in real-world attacks.

---

# Task 1: Introduction

This section introduces the purpose of the room.

We are expected to analyze real phishing samples and identify how attackers construct convincing emails that appear legitimate at first glance.

### Key Objectives

By the end of this room, we should be able to:

* Identify social engineering techniques
* Analyze email headers for spoofing
* Detect suspicious URLs and redirect chains
* Recognize malicious attachments
* Understand credential harvesting workflows

No questions are asked in this task.

---

# Task 2: Cancel Your Order (PayPal Impersonation Attack)

## Overview

This phishing email impersonates PayPal and claims that a transaction has been made. The attacker’s goal is to create urgency so the victim clicks the “Cancel Order” button without verifying authenticity.

---

## Step 1: Email Header Analysis

The first step in any phishing investigation is to inspect the email headers.

We observe:

* The **From address** appears as:

  ```
  service@paypal.com
  ```
* However, the actual sending domain is:

  ```
  gibberish@sultanbogor.com
  ```

### Key Insight

This mismatch indicates **email spoofing**, where attackers fake trusted brands to gain credibility.

Additionally, the **To field** shows an unusual recipient address, not associated with normal PayPal communication systems.

---

## Step 2: Social Engineering in Email Body

The email body contains:

* Fake purchase notification
* Gift card transaction details
* A “Cancel Order” button

This is designed to:

* Trigger panic
* Encourage immediate action
* Reduce critical thinking

Attackers rely heavily on emotional manipulation here.

---

## Step 3: Button Link Analysis

Instead of clicking the button, we inspect the raw HTML source.

We discover:

* The button contains a **shortened URL**
* The URL hides the final destination

### Why this matters

URL shorteners are commonly used in phishing because they:

* Hide malicious domains
* Bypass email filters
* Prevent users from previewing the real URL

To analyze it safely, tools like **WhereGoes** can reveal the redirect chain without visiting the site.

---

## Answer

The merchant listed in the email is:

```text id="t2a1"
Amazing Stuff
```

---

# Task 3: Track Your Package (Tracking Pixel Attack)

## Overview

This email pretends to be a shipping notification, creating urgency by referencing a tracking number.

---

## Step 1: Initial Email Analysis

We observe:

* Sender name: “Distribution Center”
* Actual email domain: `contact@beginpro.club`
* Fake tracking number included in subject line

### Red Flags

* Domain does not match any known courier service
* Sender identity is inconsistent
* Subject line creates urgency

---

## Step 2: Understanding Tracking Pixels

When analyzing the raw email source, we find:

```html id="px1"
<img src="Tracking.png">
```

However, this image is not harmless.

### What is a tracking pixel?

A tracking pixel is a **1x1 invisible image** embedded in emails that:

* Loads when the email is opened
* Sends a request back to the attacker server
* Confirms the victim is active

### Why attackers use it:

* Track email engagement
* Validate active targets
* Improve phishing effectiveness

Email providers like Yahoo often block images automatically due to this risk.

---

## Step 3: Link Analysis

The tracking URL resolves to:

```text id="dom1"
devret[.]xyz
```

### Why defang?

We defang the URL to prevent accidental clicking.

---

## Answer

```text id="ans1"
devret[.]xyz
```

---

# Task 4: Download Document Here (Multi-Stage Redirect Attack)

## Overview

This phishing campaign is more advanced. It uses trusted cloud services like OneDrive and Adobe to disguise malicious activity.

---

## Step 1: Email Characteristics

We observe:

* Fake document sharing notification
* Expiration urgency (“expires today”)
* Button labeled “Download Document Here”

### Psychological manipulation:

Attackers create urgency to force immediate action.

---

## Step 2: Redirection Chain

Clicking the button triggers multiple redirects:

1. Fake OneDrive page
2. Fake Adobe document page
3. Credential login page

Each step increases legitimacy by using trusted branding.

---

## Step 3: Credential Harvesting Page

The final page mimics a login portal.

However:

* It does NOT authenticate real accounts
* It only collects input data
* Credentials are sent to attacker-controlled server

---

## Answer

This attack type is:

```text id="ans2"
Credential Harvesting
```

---

# Task 5: Your Account Is on Hold (Fake Billing Attack)

## Overview

This phishing email impersonates Netflix billing services.

---

## Step 1: Email Header Analysis

We identify:

* Display name: “Netllx billing” (intentional misspelling)
* Fake account suspension warning
* Billing update request

### Key Indicators:

* Domain mismatch
* Misspelled brand name
* Urgent tone

---

## Step 2: Attachment-Based Attack

Unlike previous tasks, this email contains a **PDF attachment**.

Inside the PDF:

* A clickable link labeled “Update Payment Account”
* Redirects to a non-official domain

### Why attachments are dangerous:

* Email filters may not scan embedded links
* Users trust PDFs more than external links
* Attackers hide payloads inside documents

---

## Answer

Hidden sender email address:

```text id="ans3"
z99@musacombi.online
```

---

# Task 6: Your Recent Purchase (Blank Email Attack)

## Overview

This phishing email contains no body text and relies entirely on an attachment.

---

## Step 1: Email Header Analysis

We observe:

* Sender impersonates Apple Support
* Recipient is BCC’d (hidden recipients)
* Fake purchase urgency

---

## Step 2: Understanding BCC

BCC stands for Blind Carbon Copy.

### Why attackers use BCC:

* Hide other victims
* Avoid exposing recipient list
* Mass phishing campaigns

---

## Step 3: Attachment Analysis

The attachment is:

```text id="file1"
.dot file (Microsoft Word Template)
```

This is unusual for receipts or invoices.

Inside the file:

* Embedded redirect link
* Leads to phishing website

---

## Answers

```text id="ans4"
Blind Carbon Copy
```

```text id="ans5"
.dot
```

---

# Task 7: Scheduled Shipment (Malicious Excel Attack)

## Overview

This phishing email impersonates DHL Express using branded HTML.

---

## Step 1: Email Analysis

We observe:

* Fake DHL branding
* Shipping confirmation subject
* Suspicious sender domain

---

## Step 2: Excel Attachment Investigation

The email contains:

```text id="xlsx1"
.xlsx file
```

Inside the file:

* A clickable link
* Redirects to malicious download

---

## Step 3: Payload Execution

Clicking the link attempts to download:

```text id="exe1"
regasms.exe
```

### What happens if executed?

If the file runs, the attacker could:

* Install malware
* Steal credentials
* Maintain persistence
* Exfiltrate sensitive data

---

## Answer

```text id="ans6"
regasms.exe
```

---

# Conclusion

This room demonstrates how phishing attacks are carefully engineered using:

### Techniques Observed

* Brand impersonation (PayPal, Netflix, DHL, Apple)
* URL shortening and redirection chains
* Tracking pixels
* Malicious attachments (.pdf, .dot, .xlsx)
* Credential harvesting portals
* Social engineering urgency tactics

---

## Key Takeaways

* Never trust display names alone
* Always inspect email headers
* Avoid clicking attachments blindly
* Analyze links before interacting
* Use sandbox tools for investigation

---

## Final Answers Summary

| Task         | Answer                                              |
| ------------ | --------------------------------------------------- |
| Merchant     | Amazing Stuff                                       |
| Domain       | devret[.]xyz                                        |
| Attack Type  | Credential Harvesting                               |
| Sender Email | [z99@musacombi.online](mailto:z99@musacombi.online) |
| BCC Meaning  | Blind Carbon Copy                                   |
| File Type    | .dot                                                |
| Executable   | regasms.exe                                         |

---

## Final Note

Phishing attacks are becoming more sophisticated with AI-generated emails and realistic branding. Continuous practice in email analysis is essential for SOC analysts and cybersecurity professionals.
