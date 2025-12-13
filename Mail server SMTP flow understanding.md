---

# SMTP / Email Flow – Complete Real-Time Summary (Production View)

This summary explains **how email actually works in real environments**, covering **submission, MTA communication, security, and services involved**, with **examples at each step**.

---

## 1️⃣ Mail User Agent (MUA / MTU) – User Side

### What it is

The application used by the end user to send or read emails.

### Real examples

* Outlook
* Gmail mobile app
* Thunderbird
* Webmail (OWA, Gmail web)

### Role in real life

* Composes the email
* Submits the email to the **SMTP Submission Server**
* Retrieves emails using **IMAP/POP**

---

## 2️⃣ SMTP Submission Server (Port 587 / 465)

### What it is

A **logical role** of the mail server that accepts emails **from users**, not from other mail servers.

> It is **not a separate product** – it is the same mail server software (Postfix, Exchange, Exim) running with submission rules.

---

### When is it internal vs internet-facing?

#### ✔ Internal (Company / On-Prem)

* Used by office LAN or VPN users
* May also be exposed to internet for remote employees

Example:

```
mail.company.com:587
```

#### ✔ Internet-facing (Cloud like Gmail / O365)

* Always public
* Used by millions of users globally

Examples:

```
smtp.gmail.com:587
smtp.office365.com:587
```

---

### Why TLS terminates at Submission Server?

**Purpose of TLS here:**

* Protects user credentials
* Protects email content
* Mandatory for compliance (SOC2, ISO, etc.)

Even if submission server is internal:

* User passwords must never travel in plain text
* Insider threats and packet sniffing are possible

---

### Real-time flow

```
MUA → Submission Server (587 / 465)
- STARTTLS or implicit TLS
- User authentication (username/password or OAuth)
```

---

### Services / Packages used

| Platform   | Service                   |
| ---------- | ------------------------- |
| Linux      | Postfix / Exim / Sendmail |
| Exchange   | Transport Service         |
| Gmail/O365 | Proprietary MTAs          |

---

## 3️⃣ Internal MTA (Mail Transfer Agent)

### What it is

The component responsible for **routing and delivering emails**.

> Submission Server and MTA are often the **same software**, acting differently based on port and policy.

---

### Role in real life

* Accepts mail from submission service
* Decides where to send mail next
* Performs DNS MX lookups
* Handles queues and retries

---

### When is MTA internal?

* High-security environments
* Banks, enterprises
* Outbound mail sent via relay

Example:

```
Internal Postfix → Mail Relay → Internet
```

---

### When is MTA internet-facing?

* Most companies
* MX record points directly to it
* Receives mail from other domains

Example:

```
MX → mail.company.com (public IP)
```

---

## 4️⃣ MTA → MTA Communication (Port 25)

### How external delivery works

Example:

```
user@company.com → user@yahoo.com
```

### Real-time flow

1. Internal MTA queries DNS:

```
MX yahoo.com
```

2. Finds destination MTA:

```
mta5.am0.yahoodns.net
```

3. Connects over port 25

4. STARTTLS is negotiated

5. Mail is transferred securely

---

### Important security clarification

| Point                       | Reality |
| --------------------------- | ------- |
| Port 25 is plain by default | True    |
| Mail is unencrypted         | ❌ False |
| STARTTLS is used            | ✔ True  |
| TLS is hop-by-hop           | ✔ Yes   |

TLS **terminates at each MTA**, not end-to-end.

---

### Services involved

* Postfix (smtp client/server)
* Sendmail
* Exim
* Exchange Edge Transport

---

## 5️⃣ Mail Delivery Agent (MDA)

### What it is

Responsible for **storing email in user mailbox**.

### Real examples

* Dovecot
* Cyrus
* Exchange Mailbox Service

### Flow

```
Receiving MTA → MDA → Mailbox
```

Mailbox locations:

* Maildir
* mbox
* Exchange DB

---

## 6️⃣ Mail Retrieval (IMAP / POP)

### How user reads mail

| Protocol | Port      |
| -------- | --------- |
| IMAP     | 143 / 993 |
| POP3     | 110 / 995 |

### Flow

```
MUA → IMAP/POP → Mailbox
```

---

## 7️⃣ Internal Company Mail Flow (Same Domain)

Example:

```
user1@company.com → user2@company.com
```

Flow:

```
Submission → Internal MTA → Internal MDA
```

✔ No MX lookup
✔ No internet involvement
✔ Very fast

---

## 8️⃣ Port Summary (Must Remember)

| Purpose               | Port      |
| --------------------- | --------- |
| SMTP Submission       | 587       |
| Secure SMTP           | 465       |
| Server-to-Server SMTP | 25        |
| IMAP                  | 143 / 993 |
| POP3                  | 110 / 995 |

---

## 9️⃣ Why Port 25 is Blocked for Users

* Prevents spam
* Prevents infected PCs sending mails
* Users must authenticate via submission server

**Port 25 is allowed between servers**, not end users.

---

## 🔟 One-Line Interview Summary (Final)

> “Users submit email to an SMTP submission service over port 587 or 465 using TLS and authentication. The mail server’s MTA then performs DNS MX lookup and transfers the message to the destination domain’s MTA over port 25 using STARTTLS. The receiving MTA hands the message to an MDA for mailbox storage, and users retrieve it via IMAP or POP.”

---

## ⭐ Key Real-World Notes (Senior Level)

* Submission server is a **role**, not a separate product
* Same MTA software handles submission and transfer
* TLS is used at multiple hops
* Internet-facing MTAs use spam filtering and policy checks
* Logs are critical (`/var/log/maillog`)

---

Buddy ❤️
This is **complete SMTP coverage for interviews**.
Next, if you want, we can do:

* SMTP troubleshooting scenarios
* Mail stuck in queue cases
* SPF / DKIM / DMARC in real flow
* Exchange vs Postfix comparison

Just say the word 👍
