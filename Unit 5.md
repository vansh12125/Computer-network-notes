
> **EXACT UNIT V SYLLABUS FROM THE UPLOADED PAGE:** **Application Layer:** File transfer, access and management, electronic mail, virtual terminals. **Internet and public networks.** This guide covers those headings in depth. FTP, FTAM, SMTP, POP3, IMAP, MIME, TELNET, NVT, SSH and Internet architecture are introduced as explanations or examples of the prescribed headings; supporting topics such as DNS, HTTP, URLs, public packet networks and security are clearly marked as additional context.

> **HOW TO READ EVERY CHAPTER:** First read **HINGLISH - real-life example**, then **COMPLETE ENGLISH - examination writing**, then study the **labelled diagram, step sequence, comparisons and common mistakes**. Part B has **10 full 20-mark model answers**; Part C has **40 short question-answers, numericals, protocols and revision practice**. Important questions are syllabus-based suggestions, **not verified university previous-year questions**.


# PART A - COMPLETE THEORY: HINGLISH FIRST, ENGLISH NEXT

## 1. Application Layer: What It Is and Why We Need It

### 1.1 Hinglish - student life example

Socho tum college assignment bana rahe ho. Tum Google par research karte ho, email se teacher ko PDF bhejte ho, college server se lecture file download karte ho, aur kabhi remote server par terminal open karte ho. In sab kaam mein Internet connection common hai, lekin **kaam alag-alag hain**: browser web page mangta hai, mail app message send karti hai, file client data copy karta hai, aur remote terminal commands bhejta hai.

Network ki lower layers packets pahunchane mein help karti hain. **Application Layer** decide karti hai ki application ka data kis format mein request karna hai, kya reply aayega, aur server-client ki conversation ke rules kya honge. Jaise restaurant mein waiter aur kitchen ko order ka format samajh aata hai: "one tea" ka matlab sab parties ko same hona chahiye. Waise protocol agreement ke bina do programs properly baat nahin kar sakte.

**Memory shortcut:** File = transfer/manage; Email = store-and-forward message; Terminal = remote commands; Internet = networks of networks.

### 1.2 Complete English - definition, position and services

The **Application Layer** is the uppermost layer (Layer 7) of the OSI reference model. It provides network services directly used by application software, such as file sharing, message exchange, remote login and information retrieval. It does **not** mean that an entire browser or email program is identical to the OSI layer: an application program uses application-layer protocols while also having user interfaces, local storage and other functions.

In the TCP/IP architecture, application protocols normally sit above TCP, UDP or another transport service, with responsibilities resembling OSI's Application, Presentation and Session layers often combined in actual software. A protocol specifies **message formats, request types, response meanings, sequencing and error conditions**.

Major services include (1) file transfer and remote file operations, (2) electronic mail exchange, (3) virtual-terminal/remote-login access, (4) naming and directory services, (5) web information retrieval and (6) application authentication and access-control mechanisms. The first three correspond directly to the syllabus; the others support the Internet/public-networks heading.

```text
                    USERS / PROGRAMS
   +-------------------------------------------------+
   | browser | mail client | file client | terminal   |
   +-------------------------------------------------+
                       APPLICATION LAYER
      HTTP / SMTP / IMAP / FTP / SSH / DNS / etc.
   +-------------------------------------------------+
   | TRANSPORT: TCP, UDP and other transport choices |
   +-------------------------------------------------+
   | INTERNET/NETWORK: IP and packet forwarding      |
   +-------------------------------------------------+
   | LINK + PHYSICAL: Ethernet / Wi-Fi / fibre etc.  |
   +-------------------------------------------------+
                         NETWORK
```

### 1.3 Main elements of an application protocol

1. **Message type:** for example, a request, response, command, reply or notification.
2. **Syntax:** how fields, line endings, headers and message bodies are represented.
3. **Semantics:** what each command or status code actually means.
4. **Timing and order:** when a client can issue a command and when it should wait.
5. **Authentication and authorisation:** how an identity is established and what operations are permitted.
6. **Error handling:** what to do after invalid commands, unexpected disconnection or denied access.
7. **State handling:** whether the server retains session state across requests.
8. **Security requirements:** confidentiality, integrity, replay resistance and safe credentials where appropriate.

**Example:** In FTP, the server can reply `220` when the service is ready and `230` after successful login. In SMTP, `250` signals acceptance of a requested action. The meaning is defined by each protocol, not by the TCP/IP transport layer itself.

### 1.4 Client-server, peer-to-peer and hybrid applications

**Client-server** applications involve client programs requesting services from server programs. A student's mail app contacting an email provider and an FTP client contacting a file server are common examples. Servers are often available continuously and accept many clients. **Peer-to-peer (P2P)** designs allow peers to serve and request resources from each other. **Hybrid** applications use central coordination with direct peer data exchange for some work. These describe application architectures, not the physical shapes of the network.

| Feature | Client-server | Peer-to-peer | Hybrid |
|---|---|---|---|
| Main service location | Central or distributed servers | Participating peers | Combination |
| Administration | Often centrally managed | Can be distributed | Mixed |
| Example | Mail service, website | Some file-sharing systems | Coordinated direct communication |
| Main challenge | Server availability, scale | Discovery, trust, availability | Complexity, coordination |

**Exam tip:** Start an Application Layer answer with its definition, the TCP/IP relationship, three syllabus services, diagram, and one real-life example.

## 2. File Transfer, Access and Management: Concepts and Functions

### 2.1 Hinglish - college assignment folder

Maan lo tumne apne laptop par `project.zip` banayi aur usse university ke server par upload karna hai. **Transfer** ka matlab file ko ek device se doosre device tak copy karna. But teacher ne bola: uploaded file ko rename bhi karo, directory banao, old version delete karo aur permission check karo. Yeh sab **file access and management** ka part hai. Sirf file transfer kar dena aur remote files manage karna same cheez nahi hai.

Aur socho file aadhi upload hui aur Wi-Fi disconnect ho gaya. A good system ko error detect karna, incomplete data handle karna, aur where supported resume/repair mein help karni chahiye. Agar access permission nahi hai, file download nahin honi chahiye. Isliye remote file protocols mein security aur control important hain.

### 2.2 Complete English - FTAM and overall objective

**File Transfer, Access and Management (FTAM)** is the name of a traditional OSI application-layer service/protocol framework intended to permit file-related operations between different computer systems. Its objectives include transferring files, accessing remote-file contents, and managing file attributes and resources across heterogeneous hosts. FTAM is historically important in OSI networking but is **not the dominant protocol used for Internet file transfers today**.

More generally, modern application-layer systems provide these categories of file services:

- **Transfer:** copy complete files between client and server; upload and download are the two directions.
- **Access:** read, write, append or retrieve portions of remote files, depending on the system and permissions.
- **Management:** create, rename, move, delete, list files/directories, inspect metadata and modify allowed attributes.
- **Control:** authenticate users, check authorisation, manage sessions, signal failures and report status.
- **Representation:** agree on file names, directory syntax, text/binary transfer modes and metadata.
- **Reliability:** detect errors, coordinate completion and optionally support retries or resumption.

FTAM terminology can be introduced when the syllabus says “file transfer, access and management,” while **FTP** is a particularly useful Internet example of transferring and managing files.

### 2.3 Operations and their meanings

| Operation | Meaning | Student example |
|---|---|---|
| Upload / put | Transfer local file to remote server | Submit assignment PDF |
| Download / get | Copy remote file to local computer | Download lecture notes |
| List | View directory entries | Display folder contents |
| Create directory | Make remote folder | `Semester5/Networks` |
| Rename | Change filename/path | `draft.pdf` to `final.pdf` |
| Delete | Remove permitted resource | Remove wrong submission copy |
| Read / write | Access remote content | View or update shared text |
| Get attributes | Inspect size, timestamps, permissions | Check uploaded-file details |

Different protocols support different subsets of these operations. A “download-only” web link is not equivalent to a full remote file-management service.

### 2.4 File-transfer architecture and sequence

```text
   STUDENT COMPUTER                                   UNIVERSITY SERVER
 +-------------------+                          +------------------------+
 | Local folder      |                          | Remote user directory  |
 | project.zip       |                          | /submissions/          |
 +---------+---------+                          +------------+-----------+
           |                                                 ^
           | 1. Connect and authenticate                     |
           |-----------------------------------------------> |
           | 2. Ask permission / choose destination          |
           |-----------------------------------------------> |
           | 3. Transfer file data                            |
           |===============================================> |
           | 4. Completion response / error                   |
           |<----------------------------------------------- |
           | 5. Optional list / rename / retrieve             |
           +<----------------------------------------------->+
```

A normal file-transfer cycle includes server discovery, connection setup, authentication, file selection, type/representation selection, data transmission, completion confirmation and session close. Errors can occur at any stage: wrong hostname, access denied, nonexistent file, full disk, disconnection, interrupted transmission or corrupted application data.

### 2.5 Metadata, access rights and file representation

**Metadata** is information *about* a file such as its name, size, modification timestamp, type and owner. Permissions such as read/write/execute vary by operating system and server. Remote file services must distinguish between “a user knows the URL or path” and “a user is authorised to access the content.”

For text files, converting end-of-line conventions or encodings may sometimes be useful; for archives, images, executables and encrypted files, converting bytes can damage content. **Binary transfer** preserves file bytes as transferred, making it the usual sensible option for arbitrary files. The exact semantics depend on the protocol.

### 2.6 Complete answer-writing outline

For a 20-mark answer on **File Transfer, Access and Management**, write definition of FTAM, three types of operations, architecture diagram, steps of remote operation, FTP as Internet example, permissions/security concerns, advantages, limitations and comparisons. Do not say “FTAM and FTP are the same protocol.”

## 3. FTP: File Transfer Protocol - Architecture and Working

### 3.1 Hinglish - canteen counter and delivery window

Maan lo hostel canteen mein ek counter **order lene** ke liye hai aur second window **food deliver** karne ke liye. Tum order counter par bolte ho, “one sandwich,” aur actual food doosri jagah se receive karte ho. Classic FTP mein bhi **control connection** commands ke liye aur **data connection** file/listing transfer ke liye separate rehte hain. Isi ko FTP ka *dual-connection design* kehte hain.

Agar tum folder list mangte ho, woh bhi data transfer ho sakta hai; agar file upload karte ho, content data channel par jaata hai. Login aur `RETR`/`STOR` jaise instructions control connection se jaate hain. FTP ke old design ko samajhna exam mein important hai.

### 3.2 Complete English - definition and protocol characteristics

**File Transfer Protocol (FTP)** is a standard application-layer protocol that uses TCP to transfer files and perform certain remote directory/file operations between an FTP client and an FTP server. Its conventional control connection is established to server **TCP port 21**. Classic FTP uses a **separate data connection** for file data or directory listings. FTP is a **stateful** protocol: the server maintains login, working-directory and transfer context during the session.

FTP specifies commands, three-digit reply codes, data-connection modes, transfer representations and session-control operations. The protocol historically developed for networks where encryption was not an initial default. **Plain FTP does not encrypt credentials or file contents**; use secure alternatives for sensitive transfers.

```text
                         FTP CLIENT                            FTP SERVER
                   +---------------------+              +--------------------+
                   | User commands / CLI |              | FTP command server |
                   | FTP protocol engine |              | TCP port 21        |
                   +----------+----------+              +----------+---------+
                              |   CONTROL: commands/replies     |
                              +=================================+
                              |                                 |
                   +----------+----------+              +-------+------------+
                   | Local file system    |              | Remote file system |
                   | FTP data process     |              | FTP data process   |
                   +----------+----------+              +-------+------------+
                              |   DATA: file bytes/listing      |
                              +=================================+
                          Separate TCP data connection (per transfer)
```

### 3.3 FTP commands with examples

| Command | Purpose | Example meaning |
|---|---|---|
| `USER` | Send username | `USER student` |
| `PASS` | Send password | Authenticate account |
| `PWD` | Show present remote directory | Display current location |
| `CWD` | Change remote directory | `CWD /notes` |
| `LIST` | Request directory listing | Show filenames/details |
| `RETR` | Retrieve/download a remote file | `RETR lecture.pdf` |
| `STOR` | Store/upload a file | `STOR assignment.pdf` |
| `DELE` | Delete remote file if permitted | `DELE old.pdf` |
| `MKD` | Make directory | `MKD semester5` |
| `RMD` | Remove directory if permitted | `RMD oldfolder` |
| `TYPE` | Choose data representation | `TYPE I` binary image type |
| `PASV` | Request passive mode | Server supplies data endpoint |
| `QUIT` | End session | Disconnect |

In textbook explanations, `RETR` and `STOR` are especially important. Server replies include `220` (service ready), `331` (username OK, password needed), `230` (user logged in), `150` (about to open data connection), `226` (closing data connection after completion) and `550` (requested action not taken, e.g. unavailable file). These are illustrative codes, not a requirement that every session always produces exactly the same sequence.

### 3.4 How an FTP download works - step by step

1. The client identifies the server and opens a **TCP control connection to port 21**.
2. The server welcomes the client, possibly with reply `220`.
3. Client sends `USER`, then `PASS` if required; server authenticates the session.
4. Client may send `CWD`, `PWD`, `TYPE` or `LIST` to choose the file and directory.
5. Client arranges an FTP data connection using **active** or **passive** mode.
6. Client sends `RETR lecture.pdf` over the control connection.
7. The server opens/uses the data connection and sends bytes of the requested file.
8. The data connection closes when that transfer is complete; the control connection usually stays open.
9. The server sends an appropriate completion response; client may request another file.
10. Client sends `QUIT` and closes the session.

```text
 CLIENT                                        FTP SERVER
   |---- TCP connect to :21 -------------------->|
   |<--------------------------- 220 ------------|
   |---- USER student -------------------------->|
   |<--------------------------- 331 ------------|
   |---- PASS ******** ------------------------->|
   |<--------------------------- 230 ------------|
   |---- TYPE I -------------------------------->|
   |<--------------------------- 200 ------------|
   |---- PASV ---------------------------------->|
   |<--------------------------- 227 / 229 ------|
   |==== establish separate DATA connection ====>|
   |---- RETR lecture.pdf ---------------------->|
   |<--------------------------- 150 ------------|
   |<========== lecture.pdf bytes ===============|
   |<--------------------------- 226 ------------|
   |---- QUIT ---------------------------------->|
```

### 3.5 Active FTP versus passive FTP

In **active FTP**, the client informs the server where the client will accept the data connection; historically the server initiates a data TCP connection from its port 20. In **passive FTP**, the server tells the client the address/port on which it is waiting and **the client initiates** the data connection to the server. Passive mode is often easier to work with when clients are behind NAT or restrictive firewalls, although actual results depend on network policy and configuration.

```text
ACTIVE MODE                                PASSIVE MODE
Client ------control------> Server:21      Client ------control------> Server:21
Client <------data--------- Server         Client ------data--------> Server:ephemeral
   server initiates data TCP                 client initiates data TCP
```

| Aspect | Active mode | Passive mode |
|---|---|---|
| Data TCP connection initiated by | Server | Client |
| Typical practical issue | Inbound client firewall/NAT | Server must expose selected ports |
| Control connection | Client to server port 21 | Client to server port 21 |
| Is it a different file format? | No | No |

### 3.6 FTP transfer modes, strengths and limits

FTP can handle text or binary data representations. The widely taught `TYPE A` refers to ASCII representation, while `TYPE I` is image/binary representation. **File download and upload do not mean the user must manually open a data connection**: the FTP client normally performs protocol negotiation.

Advantages include standardised commands, cross-platform file exchange, directory management support, multiple-file transfers in capable clients and separation of control and data. Disadvantages include unsecured plain FTP authentication/content, additional data-connection complexity, firewall/NAT difficulties, and limited suitability for modern security-sensitive environments without protective measures.

### 3.7 Security comparison: FTP, FTPS and SFTP

| Protocol | Transport/approach | Security | Typical port |
|---|---|---|---|
| FTP | FTP over TCP, separate control/data | Plain by default | Control 21 |
| FTPS | FTP protected using TLS (explicit or implicit) | Encrypted when configured correctly | Usually 21 for explicit; 990 historically for implicit control |
| SFTP | SSH File Transfer Protocol over SSH | SSH encryption and authentication | Commonly TCP 22 |

**Critical distinction:** **SFTP is not FTP with SSL/TLS added**. It is a different protocol built on SSH. An organisation's exact ports can be changed, so listed numbers are conventional defaults, not absolute guarantees.

## 4. Remote File Access and Modern File Management

### 4.1 Hinglish - shared folder versus downloaded copy

Google Drive jaisa ek folder imagine karo. Agar tum file download karke local laptop par edit karte ho, woh ek copy hai; server automatically nahi jaanega ki tumne kya change kiya. Lekin remote file access mein program **directly remote resource read/write** kar sakta hai according to the protocol. Jaise tum lab ke shared network drive par project document open karte ho: file remotely stored ho sakti hai, lekin tumhe locally open hone jaisa experience mil sakta hai.

Iske liye system ko handle karna padta hai: kaun file edit kar raha hai, kis user ko permission hai, concurrent changes ka kya hoga, slow network par response kaise aayega aur disconnected devices ka data kaise synchronise hoga.

### 4.2 Complete English - access semantics

**Remote file access** means performing operations on files stored on another computer through a network service. The operation may be a full-file transfer or a smaller operation such as opening, reading, updating or listing a remote resource. Remote access models can involve **whole-file transfer**, **remote procedure requests**, **network file systems**, or **API-based object operations**. The protocol chosen determines supported operations, consistency, locking and metadata semantics.

Major design issues are:

1. **Naming and location:** translate logical remote paths or resource identifiers into actual files.
2. **Access control:** check account identity and permissions for each relevant operation.
3. **Concurrency:** avoid incorrect updates when two users modify the same resource.
4. **Consistency:** define when writes become visible to other readers.
5. **Caching:** improve speed, but prevent stale or incorrectly reused data.
6. **Fault handling:** partial writes, network loss, duplicate requests and server failure.
7. **Metadata:** handle size, timestamps, owner and directory organisation.
8. **Security:** protect remote access credentials, content and audit logs.

### 4.3 FTP versus network file sharing versus web downloads

| Feature | FTP/SFTP | SMB/NFS-type network file service | HTTP download |
|---|---|---|---|
| Main goal | Transfer and manage files | Use remote files through file-system operations | Retrieve a representation/resource |
| Typical interaction | GET/PUT-style commands | Open/read/write/close style | HTTP GET request |
| Directory support | Yes, with permissions | Yes | Only if server exposes a listing/API |
| Remote file editing | Usually transfer or protocol-specific access | Often supported | Not through basic GET alone |
| Security | Depends on FTP/FTPS/SFTP | Depends on version/configuration | HTTPS protects transport |

Some services wrap file-related capabilities into web APIs rather than exposing an FTP server or mounted network drive. **A web upload form** and **a file-management protocol** solve related but not identical tasks.

### 4.4 File integrity checks and permissions

If two remote copies must match, a sender or receiver may compute a cryptographic digest such as SHA-256. Comparing digests can reveal differences **when the expected digest itself comes from a trusted source**. A hash alone does not authenticate an untrusted server. Also distinguish **transport checks** (e.g. TCP checksum and reliable byte stream) from **application file validation** (e.g. expected hash, correct file name, authorised path, safe contents).

A server should validate paths and prevent unauthorised users from accessing resources outside approved directories. A student writing a network answer can mention **authentication, authorisation, directory access, file integrity, concurrency and recovery** as major file-management concerns.

## 5. Electronic Mail: Architecture, Components and Message Flow

### 5.1 Hinglish - post office with two addresses

Socho tumne Lucknow se Kanpur ke friend ko letter bhejna hai. Tum post office ke box mein letter deposit karte ho. Tumhe personally friend ke ghar jaakar letter deliver nahi karna padta. Post offices letter ko sort karke next branch tak bhejte hain. Friend ghar par nahi bhi ho, letter mailbox mein receive karke baad mein padh sakta hai. **Email bhi store-and-forward model** follow karti hai: sender ke server se receiver ke server tak transfer, phir receiver jab chahe mailbox access kare.

Important point: mail **bhejna** aur mail **padhna** different jobs hain. Usually SMTP sending ke liye, IMAP/POP3 mailbox retrieve/access ke liye use hote hain. Email address `student@example.edu` mein `example.edu` domain receiving mail servers locate karne mein help karta hai.

### 5.2 Complete English - definition

**Electronic mail (email)** is an application-layer service for composing, transmitting, delivering, storing and retrieving electronic messages across networks. Unlike a live voice call, traditional email is usually **asynchronous**: the sender and receiver need not be simultaneously connected. Internet email commonly uses **SMTP for submission and relay**, and **IMAP or POP3 for mailbox access**. Message content format is governed by Internet message standards, and **MIME** extends message representation to support attachments and varied media types.

Key components:

1. **Mail User Agent (MUA):** the email app/browser interface used to compose and read messages.
2. **Mail Submission Agent (MSA):** receives outgoing messages submitted by authenticated users.
3. **Mail Transfer Agent (MTA):** relays messages between mail servers using SMTP.
4. **Mail Delivery Agent (MDA):** deposits accepted messages into the appropriate recipient mailbox/storage.
5. **Mailbox / message store:** holds delivered messages for later retrieval.
6. **Mail access server:** offers IMAP, POP3 or a provider's web/application interface.
7. **DNS with MX records:** helps senders discover receiving mail infrastructure for a domain.

```text
  SENDER                                                  RECEIVER
+-----------+  SMTP SUBMISSION   +-----------+  SMTP RELAY   +-----------+
| Sender's  | -----------------> | Outgoing  | ------------> | Recipient |
| mail app  |                    | mail svc  |               | mail svc  |
+-----------+                    +-----------+               +-----+-----+
                                                                    |
                                                              Delivery into
                                                               mailbox store
                                                                    |
                                                                    v
                                                              +-----------+
                                                              | Mailbox   |
                                                              +-----+-----+
                                                                    |
                                                  IMAP / POP3 / Web  |
                                                                    v
                                                              +-----------+
                                                              | Receiver's|
                                                              | mail app  |
                                                              +-----------+
```

### 5.3 Step-by-step journey of one email

Suppose `aman@college.example` sends a PDF to `riya@school.example`.

1. Aman composes mail with To, Subject, body and attachment using a mail user agent.
2. The client submits the message to the configured submission service, usually through authenticated SMTP submission.
3. The sending system checks recipient domain information, often using DNS MX lookup for inter-domain delivery.
4. The responsible SMTP server opens a connection to the receiving mail service and attempts relay/delivery.
5. The receiving service may enforce size, identity, spam, malware and policy checks before accepting mail.
6. Once accepted, a delivery component places the message into Riya's mailbox, or another policy-directed location.
7. Riya's mail program accesses the mailbox through IMAP, POP3 or webmail.
8. Riya reads the message and may download the attached PDF. Delivery and read confirmation are distinct events and are not always available.

An SMTP server can temporarily queue email and retry when the destination service is unavailable. Therefore email is designed to tolerate many temporary disconnections, although **email delivery is not an unconditional guarantee**: messages can bounce, expire, be rejected or be filtered.

### 5.4 Email address and logical message structure

An email address is typically represented as `local-part@domain`, for example `riya@school.example`. The domain is important for routing mail, while the local part identifies the recipient within that domain's system. A message has **headers** and a **body**.

```text
From: Aman <aman@college.example>
To: Riya <riya@school.example>
Date: Tue, 17 Nov 2026 15:30:00 +0530
Subject: Computer Network Assignment
MIME-Version: 1.0
Content-Type: text/plain; charset=UTF-8

Hello Riya,
Please check the completed assignment.
```

Header names such as From, To, Date, Subject and Message-ID serve different purposes. A MIME message with an attachment typically uses a **multipart** structure containing a text part and one or more attachment parts. `Cc` identifies copied recipients and `Bcc` recipients are ordinarily not disclosed in the delivered message's visible recipient headers.

### 5.5 Envelope versus message header

A subtle but important distinction: **SMTP envelope addresses** used during transfer (`MAIL FROM`, `RCPT TO`) are not necessarily identical to the human-readable `From:` and `To:` headers inside the message. Servers use envelope recipient information for routing and delivery decisions. Headers are the message representation a recipient reads; policies and forwarding can legitimately make envelope/header addresses different.

**Exam warning:** SMTP does not automatically mean that the receiver has read the email. SMTP acceptance by a receiving server only tells us that a certain transport/delivery step succeeded.

## 6. SMTP - Sending and Relaying Electronic Mail

### 6.1 Hinglish - email handover to postal desk

Maan lo tum courier office mein parcel deposit karte ho. Employee address verify karta hai, parcel accept karta hai, phir next sorting centre ko forward karta hai. **SMTP (Simple Mail Transfer Protocol)** isi tarah mail transfer ki conversation ke rules define karta hai. Mail app SMTP server ko message submit kar sakti hai, aur mail servers SMTP se ek doosre ko message relay karte hain.

Ek order important hai: hello, sender, receiver, actual message, finished. Agar recipient invalid hai to system error bata sakta hai. Sender generally `DATA` ke baad message headers/body bhejta hai aur end mark karne ke liye special terminator use hota hai.

### 6.2 Complete English - definition and ports

**Simple Mail Transfer Protocol (SMTP)** is an application-layer protocol used primarily for **transmitting electronic mail** from clients to submission servers and between mail transfer agents. SMTP is command/response based and generally uses TCP. Conventional **TCP port 25** is used for server-to-server SMTP relay; **port 587** is widely used for authenticated message submission; **port 465** is used for submission with implicit TLS in widely deployed configurations. Port assignments and provider policies can vary.

SMTP normally begins with server greeting, followed by client identification (`EHLO`), envelope sender (`MAIL FROM`), envelope recipient(s) (`RCPT TO`), message content (`DATA`) and session termination (`QUIT`). SMTP extensions are advertised after `EHLO`, for example whether particular authentication and TLS options are supported.

```text
SENDER / CLIENT                                       SMTP SERVER
     |---------------- TCP connection ------------------->|
     |<----------------------------- 220 greeting --------|
     |---------------- EHLO client.example ------------->|
     |<----------------------------- 250 capabilities ----|
     |---------------- MAIL FROM:<a@x.example> --------->|
     |<----------------------------- 250 -----------------|
     |---------------- RCPT TO:<b@y.example> ----------->|
     |<----------------------------- 250 -----------------|
     |---------------- DATA ----------------------------->|
     |<----------------------------- 354 -----------------|
     |---------------- Message headers + body ---------->|
     |---------------- <CRLF>.<CRLF> -------------------->|
     |<----------------------------- 250 accepted --------|
     |---------------- QUIT ----------------------------->|
     |<----------------------------- 221 closing ---------|
```

### 6.3 SMTP commands and meanings

| Command | Use | Comment |
|---|---|---|
| `EHLO` | Identify client and request extensions | Modern normal greeting |
| `HELO` | Original simpler greeting | Does not negotiate extensions |
| `MAIL FROM` | Declare envelope sender | Used during mail transaction |
| `RCPT TO` | Declare one recipient | Can repeat for multiple recipients |
| `DATA` | Start sending message content | Server usually answers `354` |
| `RSET` | Reset current mail transaction | Connection can remain open |
| `NOOP` | No operation | Check server responsiveness |
| `QUIT` | End SMTP session | Server may reply `221` |
| `STARTTLS` | Request TLS upgrade when advertised/supported | Must be correctly enforced to provide intended protection |

### 6.4 Main SMTP response-code classes

- **2xx:** requested action accepted or completed, for example `250`.
- **3xx:** intermediate response requesting more information, for example `354` before message data.
- **4xx:** transient negative result; sender may retry according to policy.
- **5xx:** permanent negative result for the attempted action; typically not retried without change.

SMTP uses a **store-and-forward** approach. Sending servers may queue failed temporary deliveries and later retry. Servers can reject mail because recipient does not exist, policy denies mail, authentication is missing, size limits are exceeded, or temporary resources are unavailable. Exact retry timing is implementation-dependent.

### 6.5 Security limits and how modern mail protects transfer

Legacy SMTP did not make encryption a universal property. **TLS** can protect client-to-server or server-to-server hops when negotiated and verified correctly. **Authenticated submission** restricts who may submit mail through a service. Domain-authentication mechanisms such as **SPF, DKIM and DMARC** help receivers evaluate sending-domain authorisation and authenticity signals, but they do not themselves encrypt the email body or guarantee messages are harmless.

End-to-end content encryption (for example, with appropriate mail encryption tooling) is a separate concern from SMTP hop-level TLS. Spam filtering, attachment scanning and phishing detection are additional operational safeguards.

### 6.6 Sample email transfer: exam explanation

A student enters a recipient address, the MUA submits via SMTP to its submission server, an MTA resolves receiving mail service information, an SMTP dialogue transfers the message, and receiving infrastructure stores the accepted message. The recipient accesses stored mail later using IMAP/POP3 or webmail. This is the clearest explanation of **sending versus receiving** in a 20-mark answer.

## 7. POP3, IMAP and MIME - Receiving and Representing Email

### 7.1 Hinglish - mailbox ko kaise read karte hain?

Socho post office mein tumhare naam se ek locker hai. Ek option hai: tum letters locker se **download karke ghar par store** kar lo. Dusra option hai: locker server par hi organized rahe, tum mobile aur laptop dono se same inbox ke folders, read/unread status aur message list dekh sako. First approach ko samajhne ke liye **POP3**, second ke liye **IMAP** best examples hain. Dono ko “email bhejne ka protocol” bolna incorrect hai; sending ka main protocol SMTP hai.

Ab socho teacher ne email mein PDF, photo, audio file attach ki. Sirf simple plain text ke rules binary documents ko directly represent karne ke liye enough nahi the. **MIME** message body ko alag-alag sections mein arrange karne aur content types batane ki facility deta hai. Ye attachment file ko automatic safe nahi banata; type declaration aur content scanning alag matters hain.

### 7.2 Complete English - POP3

**Post Office Protocol version 3 (POP3)** is an application-layer protocol by which a mail client accesses messages stored in a server mailbox, historically with emphasis on downloading mail for local handling. Its conventional TCP port is **110**, while **POP3 over implicit TLS** commonly uses **995**. A client may be configured to leave messages on the server; POP3 does not require all messages to be deleted after download.

POP3 is often explained using three states:

1. **Authorisation:** client logs in/authenticates and server checks permission.
2. **Transaction:** client retrieves mailbox statistics, message identifiers, entire messages and optionally marks messages for deletion.
3. **Update:** when the session ends normally, the server processes pending changes such as marked deletions.

Representative commands: `USER`, `PASS`, `STAT`, `LIST`, `RETR`, `DELE`, `NOOP`, `QUIT`. POP3 responses include `+OK` for successful operations and `-ERR` for failures. Some extensions can add features, but the core POP3 model is deliberately simpler than IMAP.

```text
  CLIENT                                           POP3 SERVER
    |-------------- connect / authenticate ---------->|
    |<------------------------------------- +OK ------|
    |-------------- STAT ---------------------------->|
    |<------------------------------------- +OK 5 ...-|
    |-------------- LIST ---------------------------->|
    |<----------------------------- list of messages --|
    |-------------- RETR 2 -------------------------->|
    |<------------------------------ message number 2-|
    |-------------- QUIT ---------------------------->|
```

### 7.3 Complete English - IMAP

**Internet Message Access Protocol (IMAP)** enables clients to access and manage mailboxes maintained on a mail server. Its conventional TCP port is **143**, while **IMAP over implicit TLS** commonly uses **993**. IMAP is well suited to users who access the same account from multiple devices, because messages, folders and flags can remain stored and coordinated on the server.

IMAP supports selecting mailboxes, searching server-side messages, retrieving headers or selected message parts, managing flags such as Seen and Answered, and moving/copying messages using supported commands/extensions. Typical commands include `LOGIN` or a modern authentication command, `SELECT`, `FETCH`, `SEARCH`, `STORE`, `COPY`, `EXPUNGE` and `LOGOUT`.

```text
          +-------------------------+
          | EMAIL SERVER + MAILBOX  |
          | Inbox / Sent / Folders  |
          | Flags, message IDs      |
          +-----------+-------------+
                      ^
                IMAP access
           +----------+---------+
           |                    |
     +-----+------+      +------+------+
     | Mobile app |      | Laptop app  |
     | same inbox |      | same inbox  |
     +------------+      +-------------+
```

### 7.4 POP3 vs IMAP - detailed comparison

| Property | POP3 | IMAP |
|---|---|---|
| Primary idea | Retrieve messages from mailbox | Access/manage mailbox on server |
| Multi-device synchronisation | Limited in basic model | Designed for server-side mailbox state |
| Server-side folders | Not a core capability | Supported |
| Header/partial retrieval | More limited | Rich message-part access |
| Offline reading | Local downloads suit this well | Cached mail also permits offline reading |
| Common TCP port | 110 | 143 |
| Common implicit TLS port | 995 | 993 |
| Typical use | Simple local retrieval | Modern mobile/laptop multi-device mail |

A POP3 client can leave copies on the server, and IMAP clients can cache messages offline. Therefore avoid the oversimplifications “POP3 *always* deletes server messages” and “IMAP never downloads anything.”

### 7.5 MIME - Multipurpose Internet Mail Extensions

**MIME** extends the Internet message format so that email can represent non-ASCII text, multimedia attachments and multiple alternative body types. MIME is **a formatting standard**, not a separate mail-transfer protocol. SMTP still transports the message; MIME describes the composition/representation of its content.

Important MIME headers include:

- `MIME-Version` - indicates MIME format version.
- `Content-Type` - content media type, e.g. `text/plain`, `text/html`, `application/pdf` or `multipart/mixed`.
- `Content-Transfer-Encoding` - describes transfer-safe encoding where needed, e.g. `base64`.
- `Content-Disposition` - can indicate attachment presentation and filename.
- `boundary` parameter - separates parts inside multipart messages.

```text
                  EMAIL MESSAGE (MIME)
                 +------------------------+
                 | Common headers         |
                 | MIME multipart/mixed   |
                 +-----------+------------+
                             |
                    +--------+-------+
                    |                |
             +------+-----+    +-----+----------------+
             | Text body  |    | PDF attachment       |
             | text/plain |    | application/pdf      |
             +------------+    | encoded bytes        |
                               +----------------------+
```

### 7.6 How a PDF attachment is sent

The email client constructs a multipart MIME message, placing ordinary text in one part and the PDF content in a separate part with suitable headers and transfer encoding. The resulting MIME message is submitted using SMTP. At the receiver, the mail application parses the parts and displays the message or offers the PDF attachment for download. **Base64 enlarges data**, typically by roughly one third before line-break and other overhead; it is not encryption or compression.

**Numerical example:** A binary attachment of **300,000 bytes** becomes approximately **400,000 Base64 character bytes** before extra formatting, because each 3 bytes of binary input are represented by 4 Base64 characters (assuming the length is divisible by 3). Extra MIME headers and line wrapping make the total email larger.

### 7.7 SMTP, POP3, IMAP and MIME together

| Technology | Main purpose | One-line memory |
|---|---|---|
| SMTP | Send/relay messages | Mail *sending* |
| POP3 | Retrieve messages | Mail *download/access* |
| IMAP | Manage server mailbox | Mail *synchronisation/access* |
| MIME | Describe message content/attachments | Mail *representation* |

## 8. Virtual Terminals - TELNET and Network Virtual Terminal

### 8.1 Hinglish - remote college computer

Socho college laboratory ka Linux server doosre room mein hai. Tumhare paas sirf laptop hai, aur tum server par command run karna chahte ho: directory list, program compile, logs check. Tum physically server ke keyboard ke paas nahi ja rahe. Tum apne laptop par **terminal program** open karte ho, command type karte ho, aur result network ke through waapas dikh jata hai. Isko **remote terminal access** kehte hain.

Different computers ke keyboards, characters aur control keys alag ho sakte hain. Ek standard “virtual” terminal format rakhen to dono systems coordinate kar sakte hain. Historical **TELNET** ne isi concept ko popular kiya. Lekin TELNET network traffic ko normally encrypt nahi karta; aaj sensitive login ke liye **SSH** commonly preferred hai.

### 8.2 Complete English - virtual terminal definition

A **virtual terminal** is a logical, standardised terminal interface that allows a user or application to interact with a remote host through a network rather than through a physically attached terminal. It separates the local keyboard/screen implementation from the remote system's command interaction. The network service carries keystrokes or input data toward the server and presents server output to the user.

A remote-terminal system typically includes a terminal client, a terminal protocol, the network transport, and a server-side terminal/login service. It may also include authentication, terminal negotiation, character encoding, echoing policy and session control. The particular protocol defines these details.

```text
    STUDENT LAPTOP                                    REMOTE SERVER
 +--------------------+                          +----------------------+
 | Keyboard + screen  |                          | Login / shell / app  |
 | Terminal client    |                          | Terminal service     |
 +---------+----------+                          +----------+-----------+
           |     keystrokes / commands                     ^
           +----------------------------------------------->|
           |<-----------------------------------------------+
           v           result / screen output                |
     User sees response                                   Server executes
```

### 8.3 TELNET - purpose and TCP port

**TELNET** is a classic application-layer protocol that permits interactive terminal communication between a TELNET client and server over a network. Its conventional TCP port is **23**. It makes use of a **Network Virtual Terminal (NVT)** abstraction so that dissimilar host systems can negotiate a common representation of terminal data and functions.

TELNET has traditionally been associated with remote login, terminal emulation and text-based interaction. Its design allows negotiation of options, for example character echo behaviour. **Standard TELNET sends data in plaintext unless separately protected**, so passwords and commands are exposed to interception on untrusted networks. Modern secure administration normally uses SSH instead.

### 8.4 Network Virtual Terminal (NVT)

The **Network Virtual Terminal** is the standardised intermediary representation defined by TELNET. The local client translates user input and terminal behaviour into NVT-compatible form; the remote side interprets it and maps it to the host system's local behaviour. The reverse happens for output. This makes communications independent of many differences between terminal hardware/software.

```text
 REAL TERMINAL A       NVT REPRESENTATION       HOST TERMINAL B
+----------------+    +------------------+    +--------------------+
| Local keyboard | -> | Common character | -> | Remote host input  |
| / display      | <- | + control rules  | <- | / command output   |
+----------------+    +------------------+    +--------------------+
```

NVT historically describes a basic character-oriented virtual terminal, with an ASCII-based data representation and control conventions. TELNET's **IAC (Interpret As Command)** byte introduces option/control commands. Option negotiation includes command patterns such as `DO`, `DON'T`, `WILL` and `WON'T`. An interactive command stream and option negotiation may appear on the same connection.

### 8.5 TELNET connection and example dialogue

1. User enters a remote hostname or IP address in a TELNET client.
2. Client establishes a TCP connection to the server's TELNET service, commonly port 23.
3. The two endpoints may negotiate terminal characteristics/options.
4. Server may request account credentials.
5. Client sends typed commands/characters; server runs authorised tasks.
6. Server returns text output; client displays it locally.
7. User logs out or the network/host ends the session.

Example commands at a shell might be `pwd`, `ls` or `date`, but these are **remote operating-system commands**, not commands defined by the TELNET protocol itself.

### 8.6 TELNET and SSH compared

**Secure Shell (SSH)** is a secure remote-access protocol that encrypts connections and authenticates server identity when properly configured. Its conventional TCP port is **22**. Besides remote shells, SSH can support tunnelling and related file transfer through SSH-based tools such as SFTP.

| Feature | TELNET | SSH |
|---|---|---|
| Primary idea | Interactive remote terminal | Secure remote administration/session |
| Typical port | TCP 23 | TCP 22 |
| Encryption by default | No | Yes, when correctly configured |
| Remote host authentication | Weak in original design | Server host-key verification |
| Password confidentiality | Exposed over plain TELNET | Protected by encrypted channel |
| Modern recommended use | Legacy labs / restricted testing | Remote servers and administration |

**Exam distinction:** TELNET/NVT is a core classic virtual-terminal topic; SSH is a modern security comparison. **A command-line window opened locally is not automatically a virtual-terminal network session**.

## 9. The Internet: Architecture, Principles and Services

### 9.1 Hinglish - roads connected through cities

Maan lo Lucknow ka road network, Delhi ka road network aur Mumbai ka road network alag-alag local areas mein bana hai. National highways aur junctions inko connect kar dete hain. Tum ek city se doosri city tak ja sakte ho without knowing every small road beforehand. **Internet** bhi many independent networks ka collection hai jo common interconnection protocols ke through communicate karti hain.

Tumhara home Wi-Fi apna local network hai; ISP tumhe larger Internet se connect karta hai; ISPs aur backbone operators multiple networks ke beech traffic forward karte hain. Browser par website khulna Internet ka ek use hai; email, file transfer, game updates, video calls bhi Internet use kar sakte hain.

### 9.2 Complete English - Internet definition

The **Internet** is a global interconnected system of computer networks that use the **Internet Protocol suite (TCP/IP)** and related standards to exchange data. It is a *network of networks*, rather than a single centrally owned physical network. End devices communicate through local networks, access providers, routers and interconnected administrative networks.

Key concepts:

- **Hosts/end systems:** computers, mobiles, servers and connected devices that run applications.
- **Local-area network/access network:** Wi-Fi, Ethernet, cellular or other last-mile connectivity.
- **Internet Service Provider (ISP):** supplies Internet access and network transit services.
- **Routers:** forward IP packets toward destination networks based on routing information.
- **Autonomous System (AS):** administratively controlled collection of IP networks with a distinct routing policy in inter-domain routing.
- **Peering/transit:** ways distinct networks exchange or purchase connectivity.
- **DNS:** naming system for locating services via domain names and records.
- **TCP/UDP/IP:** protocol components providing addressing, forwarding and transport functions.

```text
  HOME / CAMPUS LAN             ACCESS ISP           OTHER NETWORKS
 +----------------+            +-----------+          +------------+
 | Phone          |--Wi-Fi-----| ISP       |----------| Backbone / |
 | Laptop         |--Ethernet--| routers   |----------| transit AS |--+
 | College device |            +-----------+          +------------+  |
 +----------------+                                                    |
                                                  +--------------------+
                                                  |
                                           +------+-------+
                                           | Cloud / web /|
                                           | mail servers |
                                           +--------------+
```

### 9.3 Internet versus World Wide Web

The **Internet** is the underlying global networking infrastructure and protocol-based interconnection. The **World Wide Web (WWW)** is an application/information system mainly accessed through web browsers and web protocols such as HTTP and HTTPS. Email, file transfer, DNS and many other services use the Internet without being identical to the Web.

| Internet | World Wide Web |
|---|---|
| Global interconnection of IP networks | Interlinked web resources accessed with browsers and web clients |
| Uses routing, addressing and transport | Uses URLs, HTTP(S), HTML and related web formats |
| Supports email, VoIP, FTP, web and more | One major application/service on the Internet |
| Exists independently of browser UI | Usually used through browsers or web-capable applications |

### 9.4 Packet journey from laptop to server

1. A student enters `https://university.example` in the browser.
2. Browser or operating system obtains network information, typically involving DNS resolution (possibly using cached results).
3. Browser initiates the relevant connection/transport exchange to the destination service.
4. Application data is packaged into transport packets/segments and IP packets.
5. The laptop sends packets to a local network, usually via a default gateway when the destination is remote.
6. Routers forward packets through one or more access/transit/provider networks.
7. The destination server processes application requests and sends responses.
8. Response traffic travels back through the network; the client reconstructs and presents the requested content.

Routes may be asymmetric: packets returning from a server do not necessarily traverse exactly the same routers as packets travelling toward it. The Internet provides **best-effort packet forwarding** at the IP layer; higher layers/applications handle other reliability or security requirements.

### 9.5 The TCP/IP model and Internet communication

At the Application layer, programs use HTTP, SMTP, FTP, DNS, SSH and related protocols. At the Transport layer, TCP and UDP (among other transports) serve application communication. At the Internet layer, IPv4 or IPv6 provide addressing and packet forwarding. At the Link layer, Ethernet/Wi-Fi and physical technologies move frames between adjacent systems. Layers coordinate to form communication between distant hosts.

```text
  APPLICATION   HTTP / FTP / SMTP / IMAP / TELNET / DNS
         |
  TRANSPORT     TCP / UDP / other transports
         |
  INTERNET      IPv4 / IPv6 / IP routing
         |
  LINK/PHYSICAL Ethernet / Wi-Fi / cellular / fibre
```

### 9.6 Public Internet, private networks and intranets

A **public Internet** connection allows communication across the global interconnection, subject to addressing, security policies and reachability. A **private network** may restrict access to users/devices in an organisation, or use private IP address space and gateways. An **intranet** uses Internet-derived technologies inside an organisation for internal information/services. An **extranet** exposes selected internal services to authorised external partners.

**Examples:** A college result portal accessible from anywhere may be a public-facing Internet service; the college's staff payroll site might be intranet-only; an authenticated vendor portal may behave like an extranet. Private-address hosts often use NAT when connecting outward through IPv4 gateways, but **NAT is not by itself a security policy or firewall**.

## 10. Internet Supporting Services - DNS, URL, HTTP and HTTPS

> **Additional context for the syllabus heading “Internet and public networks.”** These topics explain how students actually use Internet services. They are not separate named headings in the photographed Unit V outline.

### 10.1 Hinglish - contacts app versus phone number

Tum phone mein contact `Mummy` par tap karte ho, mobile number yaad nahi rakhte. A contact list maps friendly name to an actual dialling number. Internet mein **DNS** similar lookup help karta hai: domain names se network endpoints/records discover hote hain. Jab tum browser mein URL daalte ho, browser use protocol, host aur resource samajh kar request bhejta hai.

### 10.2 DNS - Domain Name System

The **Domain Name System (DNS)** is a distributed, hierarchical naming system that stores and serves records relating domain names to information such as IP addresses and mail destinations. Common record types include **A** (IPv4 address), **AAAA** (IPv6 address), **MX** (mail-exchange destination), **NS** (authoritative name-server reference) and **CNAME** (alias). A domain name may produce multiple relevant records and changing addresses.

```text
   Browser wants 'portal.example'
              |
              v
       Local stub resolver
              |
              v
       Recursive DNS resolver
              |
        cache?  /  
              |
       DNS hierarchy as needed
              |
              v
         IP address answer
              |
              v
       Browser contacts server
```

A caching DNS resolver may return a valid cached answer, so every visit does not necessarily require new queries to root and authoritative servers. DNS naming should not be confused with the IP routing process itself: DNS returns information; IP and routing use addresses for packet delivery.

### 10.3 URL - Uniform Resource Locator

A **URL** identifies a resource and indicates how to access it. Example:

```text
   https://portal.example:443/notes/unit5.pdf?download=1
   |---|   |------------| |-| |---------------| |--------|
   scheme      hostname   port       path          query
```

The scheme indicates the access method, e.g. HTTPS; hostname identifies the service's name; port may be explicitly supplied or inferred by default; path identifies a resource and query parameters convey application-specific information. A fragment introduced by `#` may identify a section handled within the client/document; it is not ordinarily transmitted as part of an HTTP request target.

### 10.4 HTTP and HTTPS

**Hypertext Transfer Protocol (HTTP)** is an application-layer request/response protocol for accessing web resources. A client sends a method and target; the server returns a response with status and content/metadata. **HTTPS** means HTTP carried over a secure transport arrangement, normally TLS, which provides channel protection and server authentication when correctly configured. Classical default ports are **80 for HTTP** and **443 for HTTPS**, but services can use other ports.

Important methods include:

| Method | Typical meaning | Example |
|---|---|---|
| GET | Retrieve a resource representation | Download notes page |
| HEAD | Retrieve metadata headers without response body | Check size/type |
| POST | Submit data for processing | Submit registration form |
| PUT | Replace/create resource at specified target in many APIs | Upload a defined resource |
| PATCH | Apply partial modifications | Update profile field |
| DELETE | Request removal of target resource | Delete document |

Example response statuses: **200 OK**, **201 Created**, **301 Moved Permanently**, **302 Found**, **400 Bad Request**, **401 Unauthorized** (typically needs authentication), **403 Forbidden**, **404 Not Found**, **500 Internal Server Error** and **503 Service Unavailable**.

**Subtle note:** HTTP supports versions with different transport details: HTTP/1.1 and HTTP/2 commonly use TCP; HTTP/3 uses QUIC over UDP. Therefore “HTTP always uses TCP” is not universally correct today. This is supporting modern context rather than a central Unit V examination requirement.

### 10.5 Why HTTPS matters

Without protective transport, attackers on the path may read or alter sensitive application data. HTTPS with correct certificate validation encrypts the communication channel, helps detect tampering and authenticates the server's named identity. It does not prove that a website's business content is truthful or that its downloads are safe. Browser address-bar indicators alone are not a substitute for checking domain names and content authenticity.

## 11. Public Networks - Meaning, Types and Historical Context

### 11.1 Hinglish - shared transport systems

Maan lo tumhare college ki internal bus sirf students ke liye hai. Lekin state transport bus aam logon ko travel service deti hai. Networking mein kuch networks **private/organisation-specific** hote hain, aur kuch **public network operators** subscribers ko services provide karte hain. Subscriber ko network use karne ke liye ISP/mobile operator ya service provider ke through connection mil sakta hai.

Older networking textbooks mein public telephone networks aur public packet-switching networks ke examples milte hain. Aaj public Internet access, broadband, mobile data aur cloud interconnection bhi common discussion mein aate hain. Isliye “public network = Internet” keh dena technically incomplete hai.

### 11.2 Complete English - definition

A **public network** is a communications network or service made available to multiple customers/users by a telecommunications or network service provider under its operating terms. It may carry voice, data or other communications, and it may use circuit-switched or packet-switched technology. A public network is not automatically unencrypted, unrestricted or owned by the government; **public** generally describes service availability to customers rather than the secrecy of traffic.

Types and examples:

1. **Public Switched Telephone Network (PSTN):** traditional telephone service built largely around circuit-switching concepts, now increasingly connected with digital/packet voice infrastructure.
2. **Public Data Networks (PDNs):** provider-operated services for data communication, historically including packet-switched offerings such as X.25 networks.
3. **ISDN:** historical integrated digital telecommunications service offering multiple channels/services through a public telephone-network context, already introduced in Unit I.
4. **Public Internet access:** subscriber connectivity to the global Internet through fixed and mobile ISPs.
5. **Mobile/cellular networks:** provider-operated radio-access and core networks that deliver voice/data connectivity to subscribers.

### 11.3 Circuit-switched versus packet-switched public services

**Circuit switching** creates a logical/physical dedicated connection resource for the duration of a call in traditional designs. A conventional telephone call is the textbook illustration. **Packet switching** divides data into packets shared over network links; routers/switches forward traffic according to addressing and forwarding policy. Internet packet forwarding is the central example.

```text
CIRCUIT-SWITCHED IDEA                 PACKET-SWITCHED IDEA
Caller --[reserved path]-- Callee    Client --[router]--[router]-- Server
  Establish -> Talk -> Release          Packet 1, Packet 2, Packet 3
  Resources held for call              Shared paths / queues / forwarding
```

| Feature | Traditional circuit switching | Packet switching |
|---|---|---|
| Setup | Often required before communication | IP datagrams do not require per-flow circuit setup |
| Resource allocation | Reserved during active circuit | Shared and statistical multiplexing |
| Good at | Predictable established voice circuit | Bursty and variable data workloads |
| Main drawback | Inefficient when circuit is idle | Variable delay, queueing and possible loss |
| Example | Traditional PSTN call | Internet data network |

Modern telephone services are not purely circuit switched throughout; use these as **classical architectural comparisons**.

### 11.4 Internet versus public network versus private network

| Point | Internet | Public network | Private network |
|---|---|---|---|
| Meaning | Global IP interconnection | Provider network/service offered to customers | Restricted organisational/personal network |
| Scope | Global network of networks | Can be local, national or international | Usually organisation-specific |
| Technologies | IP plus many link/transport systems | PSTN, cellular, data networks, Internet access | Ethernet, Wi-Fi, VPN, private links |
| Access | Through connectivity providers/policies | According to service subscription/policy | Usually authorised users/devices |
| Example | Email between two ISPs | Telephone operator service | College staff LAN |

A private network can use the same TCP/IP protocols as the Internet, and a public network can provide Internet access. They are not mutually exclusive labels in every real network.

### 11.5 X.25, Frame Relay and ATM - brief historical enrichment

Some conventional computer-network textbooks discuss public packet services using **X.25**, which defined interfaces/procedures for reliable packet-switched wide-area service. **Frame Relay** provided a lighter WAN frame-relaying service, and **ATM (Asynchronous Transfer Mode)** carried fixed-size cells with virtual connections. These are useful historical comparisons with contemporary IP-based networking; they are **not separately named in the Unit V syllabus image**.

- X.25: more error-control functions inside the network, useful on historically less reliable links.
- Frame Relay: frame-based WAN service with reduced in-network error-recovery assumptions.
- ATM: fixed-size cells (53 bytes total: 5-byte header plus 48-byte payload), virtual path/channel concepts.

### 11.6 Public network security and reliability

Public availability does not guarantee privacy, authentication or predictable quality. Secure systems apply encryption, strong authentication, access control, firewalls, software updates, and sound operational monitoring. Availability may be improved using redundant links, independent providers, replication, load balancing and backup services. Actual guarantees depend on the provider's policies and service-level agreements.

## 12. Integrated Real-Life Case Study - College Submission and Faculty Review

### 12.1 Hinglish - one student workflow includes most of Unit V

Socho teacher ne Computer Network assignment submit karne ko kaha. Tum laptop se college portal open karte ho (**Internet, DNS, HTTPS**). Assignment ka ZIP server par upload karte ho (**file transfer**) aur submission folder verify karte ho (**file management**). Teacher ko email bhejte ho (**SMTP**), woh mobile aur desktop par inbox check karte hain (**IMAP**), aur PDF attachment **MIME** se handle hota hai. Server mein issue aaya to lab administrator remotely commands chalata hai (**SSH virtual-terminal concept**, while TELNET is the historical syllabus example). All these are distinct application-layer operations using the same overall networking infrastructure.

### 12.2 Complete English - architecture

```text
 Student laptop                                         College / Internet systems
 +--------------------+                        +----------------------------------+
 | Web browser        |--- HTTPS ------------->| Student submission web server    |
 | File transfer app  |--- SFTP / FTP -------->| File store / directory metadata |
 | Email client       |--- SMTP -------------->| Mail submission / relay service |
 | Email client       |<-- IMAP/POP3 ----------| Recipient mailbox service       |
 | Terminal emulator  |--- SSH / TELNET ------>| Remote administration host      |
 +--------------------+                        +----------------------------------+
              |                                         ^
              +--------- ISP, routers, IP --------------+
                         DNS service as needed
```

### 12.3 How the services remain separate

HTTP(S) handles web requests. FTP/SFTP handles transfer/file operations. SMTP submits or relays outgoing messages. IMAP/POP3 provides access to received messages. MIME represents multipart message content. TELNET/SSH supports remote interactive access, with SSH commonly used for security. DNS helps discover host/mail service information; IP networks carry packets across different interconnected networks.

The important conceptual point is **application protocol versus infrastructure**. IP alone cannot represent a mail inbox or an FTP upload command. SMTP alone cannot route packets across an ISP. Each layer and protocol contributes a different function.

## 13. High-Value Comparison Tables and Protocol Memory Map

### 13.1 Full comparison of syllabus-relevant protocols

| Service | Main purpose and interaction | Common port | Essential note |
|---|---|---|---|
| FTP | File transfer; commands and separate data TCP | 21 control | Plain FTP is not encrypted |
| FTAM | Historical OSI remote-file services | Not FTP port 21 | Different framework from FTP |
| SFTP | File transfer over SSH | 22 | Check SSH host identity |
| SMTP | Email submission and relay | 25, 587, 465 | Use correct TLS/auth policy |
| POP3 | Retrieve mailbox messages | 110; 995 TLS | Protect credentials |
| IMAP | Manage mailbox and folders on server | 143; 993 TLS | Useful for device synchronisation |
| MIME | Format email content and attachments | No own port | Encoding is not encryption |
| TELNET | Interactive virtual-terminal stream | 23 | Normally plaintext |
| SSH | Encrypted interactive remote access | 22 | Verify host keys |
| DNS | Domain and service information lookup | 53 classic | Naming alone is not authentication |
| HTTP | Web request and response | 80 default | Unencrypted if plain HTTP |
| HTTPS | Protected web requests | 443 default | Validate certificates |

### 13.2 Important contrasts for 5- and 10-mark answers

- **FTP vs FTAM:** FTP is an Internet application protocol for file transfer and operations; FTAM is an OSI file-service framework covering transfer, access and management.
- **FTP vs SFTP:** SFTP is SSH-based, not an extension obtained simply by adding TLS to FTP.
- **SMTP vs POP3/IMAP:** SMTP sends and relays; POP3 and IMAP provide mailbox access.
- **POP3 vs IMAP:** POP3 is simpler retrieval-oriented; IMAP supports server folders, flags and multi-device mailbox management.
- **MIME vs SMTP:** MIME represents content and attachments; SMTP carries messages across mail services.
- **TELNET vs SSH:** Both can support remote interaction, but SSH provides encrypted authenticated remote access.
- **NVT vs terminal emulator:** NVT is a standard conceptual format; terminal emulator is a client program/interface.
- **Internet vs WWW:** The Internet is infrastructure; WWW is one major Internet application.
- **Public vs private network:** Public service access and internal restricted access are administrative/operational distinctions, not guarantees of encryption.
- **Circuit vs packet switching:** A traditional dedicated path versus shared packet forwarding.

### 13.3 Memory chart - which service to choose?

```text
WHAT DO YOU NEED?
  |
  +-- Copy or manage files? ----> FTP / SFTP / FTAM concept
  |
  +-- Send an email? -----------> SMTP
  |
  +-- Read/sync mailbox? -------> IMAP (or POP3)
  |
  +-- Attach PDF to email? -----> MIME representation
  |
  +-- Open remote terminal? ----> TELNET (historical), SSH (secure)
  |
  +-- Find server by name? -----> DNS
  |
  +-- Open website/API? -------> HTTP / HTTPS
  |
  +-- Connect many networks? --> Internet + ISP + IP routers
```

# PART B - TEN COMPLETE MODEL ANSWERS FOR 20-MARK QUESTIONS

> **How to write a 20-mark answer:** Give (i) introduction and definition, (ii) a neat labelled diagram, (iii) key functions or components, (iv) operation in sequence, (v) advantages, disadvantages and comparison where applicable, (vi) a short conclusion and example. Suggested marks below are **practice allocations**, not an official examination marking scheme. The question selection is based on syllabus importance, not verified previous-year papers.

## Q1. Define the Application Layer. Explain its services, functions and role in computer networks. (20 marks)

### Hinglish understanding before you write

Maan lo tumhare laptop par 4 apps chal rahe hain: browser, email, file transfer client aur remote terminal. Neeche Internet aur TCP/UDP ka common networking support hai, but apps ko **alag rules** chahiye. Browser GET request samajhta hai, email program sender/receiver address aur SMTP commands, FTP client `RETR`/`STOR`, aur remote terminal keystrokes. Application Layer in protocols ki help se services ko define karti hai. Exam mein Layer 7 ka introduction aur concrete examples likho.

### Complete English model answer

**Introduction and definition.** The Application Layer is the seventh and highest layer of the OSI reference model. It defines how application software uses network communication services to perform user-facing tasks such as file transfer, electronic mail, remote terminal access and information retrieval. It establishes common rules for how participating application processes format requests, interpret replies and respond to errors. While it is closest to the user in the protocol model, it should not be confused with the entire graphical application program.

**Position and relationship with other layers.** The Application Layer relies on lower-layer communication facilities. Transport protocols such as TCP or UDP support process-to-process data exchange; IP supplies network addressing and packet forwarding; link and physical technologies carry traffic along connections. In the TCP/IP model, actual application implementations frequently combine work associated with the OSI Application, Presentation and Session layers.

```text
     USER / APPLICATION PROGRAM
                  |
     +--------------------------+
     | APPLICATION - OSI L7     | <-- File, mail, web, terminal services
     +--------------------------+
     | PRESENTATION - OSI L6    |
     +--------------------------+
     | SESSION - OSI L5         |
     +--------------------------+
     | TRANSPORT - OSI L4       | <-- TCP / UDP
     +--------------------------+
     | NETWORK - OSI L3         | <-- IP
     +--------------------------+
     | DATA LINK - OSI L2       |
     +--------------------------+
     | PHYSICAL - OSI L1        |
     +--------------------------+
```

**Major functions.** (1) A protocol provides a common vocabulary of message types and commands. (2) It defines message syntax and representation, such as headers, command names and data bodies. (3) It specifies response meanings, status codes and failure conditions. (4) It may support user authentication and resource authorisation. (5) It enables resource sharing and access, including files, directories and information services. (6) It helps coordinate application sessions and manage application-level state. (7) It can define application-specific reliability or recovery actions, such as retry policies or resuming a transfer where supported. (8) It supports interoperability between different software systems when they follow compatible standards.

**Important Application Layer services.** **File transfer and management** allow users to upload, download, rename, list and delete permitted files. FTP is a classic Internet example, while FTAM refers to an OSI file-service framework. **Electronic mail** supports asynchronous message communication through SMTP relay and mailbox access through IMAP/POP3; MIME describes attachments. **Virtual-terminal services** enable remote access to host terminals, with TELNET as a historical example and SSH as a secure alternative. **Internet information services**, including DNS and HTTP/HTTPS, support name resolution and web application communication.

**Example of integrated working.** A student opens an online submission portal using HTTPS, transfers a document, receives a confirmation email and later accesses the university server using a secure terminal. The networks may be connected through an ISP and several routers, but the application protocol selected determines whether traffic represents a web request, email or remote terminal command.

**Advantages.** Application protocols standardise communication, enable sharing of remote services, support geographical distribution, permit software from different developers to interoperate, and make error reporting understandable to users and programs.

**Limitations and design concerns.** Protocol designers must consider authentication, confidentiality, compatible encoding, server availability, access control, scalable state handling, malicious inputs and failures. No single application protocol covers all user tasks.

**Conclusion.** The Application Layer connects useful user activities with lower-level networking machinery. It turns a communication channel into meaningful services such as file exchange, email and remote login. Its protocols are essential for modern Internet-based applications.

## Q2. Explain File Transfer, Access and Management (FTAM) with operations, architecture and practical examples. (20 marks)

### Hinglish understanding before you write

File server se download karna alag kaam hai, aur server par file rename karna, list dekhna, remote content read/write karna alag kaam. Inko saath mein **file transfer, access and management** kaha jata hai. FTAM classical OSI model ka term hai. Exam mein 3 categories clearly alag karo: copy, contents access, directory/metadata management.

### Complete English model answer

**Introduction.** File Transfer, Access and Management refers to an application-layer facility for handling files across computer systems. The **FTAM** standard was developed in the OSI framework to support file operations across heterogeneous hosts, where local operating systems might store files using different formats and conventions. It is historically significant. FTP, SFTP, network file systems and web APIs illustrate different contemporary solutions to related remote-file problems, but they are not the same protocol as FTAM.

**Objectives.** The broad objectives are to allow a client to locate a file, authenticate to a server, transfer file data, access remote information, inspect metadata and perform permitted management operations. The service aims to hide some differences in operating-system implementations while enforcing the remote system's security policy.

```text
 +------------------------------+   Application requests   +------------------------------+
 | CLIENT SYSTEM                | -----------------------> | SERVER SYSTEM                |
 | Student / file application   |                          | Remote file service          |
 | Local file system            | <----------------------- | Remote file system           |
 +------------------------------+    Results / content     +------------------------------+
          |                                                          |
          +------------------ TCP/IP or OSI service -------------------+
```

**(A) File transfer operations.** An **upload** sends a local file to a remote server; a **download** receives a remote file into local storage. Transfer services may allow the user to select text/binary representation, detect missing files, confirm completion and restart interrupted operations where the protocol and server support it. Example: submitting `Networks_Assignment.pdf` to a college server.

**(B) File access operations.** File access permits a client to obtain data stored remotely. Depending on the design, a system may provide full-file retrieval, read-only access, partial reads, appending or modification. An application opening a network-shared document illustrates remote access. The exact consistency and locking guarantees depend on the implementation.

**(C) File management operations.** Management includes creating directories, listing names, changing the current directory, renaming, moving or deleting files, inspecting attributes and changing permitted settings. Such commands may fail because a file does not exist, another operation is active, or the user lacks authorisation.

**Sequence of operation.** First the client determines the remote server address. Next it establishes a connection or service association. Authentication identifies the user and authorisation checks the requested operation. The client selects a file or folder, chooses an operation, and exchanges request/reply messages. The server performs the action, returns status information, and the session is finally closed or retained for additional operations.

**Important design issues.** Heterogeneous hosts can differ in file naming, text representation and permission models. Two users may update one resource concurrently, raising locking and consistency problems. Network failure can interrupt a write. Caching can speed up reads but produce stale copies. A robust server must restrict file paths and verify the caller's permissions; it should not trust filenames supplied by untrusted clients.

**Example and distinction.** FTP can implement file copy and commands such as `LIST`, `RETR`, `STOR` and `DELE`. SFTP offers remote file operations through SSH. SMB/NFS-style network file services expose a more file-system-oriented interface. **FTAM is the OSI conceptual standard**, whereas these Internet and network services are distinct implementations of related goals.

**Advantages.** Remote file services make it easier to share resources, cooperate across departments, centralise backups, administer files and reduce manual copying. **Disadvantages** include dependence on network availability, possible latency, server load, security risks, data consistency complexity and administrative permission management.

**Conclusion.** File Transfer, Access and Management is a central Application Layer topic because useful networked file work requires more than simply moving bytes. It also needs resource identification, access rules, metadata, operations and error handling.

## Q3. Explain FTP in detail. Discuss its architecture, control/data connections, commands and active/passive modes. (20 marks)

### Hinglish understanding before you write

FTP ka biggest highlight hai **do TCP connections**: one for commands and one for data. Canteen order counter aur delivery counter example yaad karo. Active mein data connection server initiate karta hai, passive mein client. `RETR` download aur `STOR` upload yaad karna important hai.

### Complete English model answer

**Definition and purpose.** The File Transfer Protocol (FTP) is a classic Internet application-layer protocol for copying files and performing selected operations on files and directories stored on a remote server. It uses TCP and a structured command/reply conversation. Its conventional server control port is TCP 21.

**Architecture.** FTP separates a persistent **control connection** for commands and replies from a **data connection** used for file contents or directory listings. The control connection begins when the client connects to the FTP server, and it commonly stays open across multiple transfers. A new data connection may be established for a transfer, depending on the session and implementation.

```text
    FTP CLIENT                                                   FTP SERVER
+-------------------+                                       +-------------------+
| Protocol interpreter|------ CONTROL TCP :21 ------------>| Protocol interpreter|
| command/reply      |<------------------------------------| authentication     |
+-------------------+                                       +-------------------+
| Data process       |<====== DATA TCP connection ========>| Data process      |
| Local storage      |                                      | Remote storage    |
+-------------------+                                       +-------------------+
```

**Connection establishment and login.** The client opens the control connection. The server may return code `220`, meaning service ready. The client issues `USER`; the server may request `PASS`. After successful authentication (`230` in a typical exchange), the client can inspect directories and transfer permitted files. FTP commands are usually plain-text commands with numeric response codes.

**Important commands.** `USER` supplies a username; `PASS` supplies a password; `PWD` reports the current directory; `CWD` changes directory; `LIST` requests a listing; `RETR filename` retrieves/downloads a file; `STOR filename` stores/uploads a file; `DELE` deletes a permitted file; `MKD` and `RMD` manage directories; `TYPE` selects representation; `QUIT` ends the session. These commands are interpreted by the FTP server rather than by the destination operating-system shell.

**Illustrative download working.** (1) Client connects to TCP 21. (2) Server greets client. (3) Client authenticates. (4) Client chooses a directory and binary representation if needed. (5) Client and server establish a data connection. (6) Client sends `RETR notes.pdf` through the control channel. (7) Server sends file bytes on the data channel. (8) A completion reply signals the transfer result; the data connection is released. (9) The control session may handle additional file operations before `QUIT`.

**Active mode.** The client provides a data-side endpoint where it can accept a connection, and the server initiates the data connection. Historically the server side used TCP port 20 as the data-source port. This approach can conflict with inbound client firewalls or network address translation.

**Passive mode.** The client asks the server to enter passive mode; the server indicates a listening data endpoint. The client initiates the data connection toward the server. This is commonly easier from client devices behind NAT/firewalls, though the server must correctly expose its selected data port.

**File representations.** FTP supports different representations, including ASCII/text and binary/image modes. Binary mode should normally be used for PDFs, compressed archives, images and arbitrary binary data because textual conversion could alter file contents.

**Advantages.** FTP standardises file exchange, supports upload and download, enables directory listing and management, works across many platforms and is conceptually straightforward to script. **Limitations.** Ordinary FTP provides no built-in encryption for login credentials and content. A separate data-channel design can be difficult with modern firewall environments, and errors or access policies complicate large transfers.

**Security and alternatives.** FTPS protects FTP communication using TLS when properly configured. SFTP is a different SSH-based file transfer protocol. A student must not claim SFTP simply means “secure FTP on port 21”; SSH-based SFTP conventionally uses port 22.

**Conclusion.** FTP demonstrates how an Application Layer protocol defines its own control messages, transfer processes and state while using TCP for byte transport. Its dual-connection architecture and command set are its most important examination features.

## Q4. Explain electronic mail architecture and describe the complete journey of an email from sender to receiver. (20 marks)

### Hinglish understanding before you write

Email ko postal system se compare karo. Student mail app se message compose karta hai, sending server accept karta hai, SMTP server-to-server transfer hota hai, receiving server mailbox mein store karta hai, receiver later IMAP/POP3 se read karta hai. Mail transfer aur mailbox access same nahi hain.

### Complete English model answer

**Introduction.** Electronic mail, or email, is a distributed Application Layer service enabling users to compose, send, receive, store, manage and retrieve digital messages. It primarily uses an asynchronous **store-and-forward** model: sender and recipient need not be online at the same moment. An email system is made of user interfaces, transfer agents, delivery components, mailbox stores and access protocols.

**Components.** A **Mail User Agent (MUA)** is the interface used for composing and reading emails. A **Mail Submission Agent (MSA)** accepts messages submitted by authenticated users. A **Mail Transfer Agent (MTA)** transfers messages between mail servers, typically using SMTP. A **Mail Delivery Agent (MDA)** deposits messages into a mailbox. A **mailbox store** retains incoming messages, and **IMAP/POP3 servers** provide client access. **DNS MX records** assist the sending system in identifying mail-exchange infrastructure for the recipient domain.

```text
  USER A                                                        USER B
+--------+     SMTP        +---------+  SMTP relay  +---------+  IMAP/POP3 +--------+
|  MUA   | --------------> | MSA/MTA | -----------> | MTA/MDA | ----------> |  MUA   |
+--------+                 +---------+              +----+----+             +--------+
                                                         |
                                                         v
                                                     +-------+
                                                     |MAILBOX|
                                                     +-------+
                       DNS MX lookup helps locate receiving service
```

**Message composition.** The sender creates an email with recipient address, subject, body and optional attachments. A typical address has a local part and a domain, such as `student@college.example`. The message contains headers (e.g. From, To, Date, Subject) and body. MIME formatting may represent a multipart email containing ordinary text and attached documents.

**Submission.** The email client connects to the sending provider's mail submission service, typically using authenticated SMTP submission with transport security. The service validates permissions and accepts the message for processing. User submission is operationally distinct from unrestricted Internet server relay.

**Routing and transfer.** A sending MTA determines where the recipient domain receives mail, commonly consulting DNS MX information. It opens an SMTP dialogue with receiving infrastructure and sends the envelope sender/recipient and message content. The receiving system may check accepted domains, recipient validity, size policy and security filters. It returns appropriate acceptance or rejection status.

**Delivery.** After an accepting mail service processes the message, the message is stored in a recipient mailbox or redirected by policy. If a destination is temporarily unavailable, an outgoing MTA may queue and retry later. If delivery remains impossible, a failure notice may be generated. Neither submission nor SMTP relay guarantees the recipient actually opened the message.

**Access and reading.** The recipient connects via a mail client using IMAP, POP3 or webmail. IMAP allows server-side folders and message-state synchronisation across devices; POP3 uses a simpler retrieval-oriented model. The client displays message headers, body and any MIME-decoded attachments.

**Benefits.** Email is inexpensive across large distances, supports asynchronous work, makes document attachments possible, creates written communication records, and allows one sender to contact multiple recipients. **Problems** include phishing, spam, malware attachments, accidental disclosure, address spoofing, message-size limits, delayed delivery and security-policy failures.

**Security measures.** Authenticated submission, encrypted SMTP/IMAP access, account protection, spam controls, domain-authentication systems and safe attachment handling reduce risks. Protection of transfer links should be distinguished from end-to-end encryption of message content.

**Conclusion.** Electronic mail is a textbook example of a coordinated set of Application Layer services. SMTP delivers messages between systems; IMAP/POP3 enable mailbox access; MIME explains complex message content. Its store-and-forward design separates sending from reading.

## Q5. Describe SMTP in detail with commands, reply codes, message transmission and security. (20 marks)

### Hinglish understanding before you write

SMTP post-office desk jaisa hai. `EHLO` introduction, `MAIL FROM` kisne bheja, `RCPT TO` kisko bhejna, `DATA` mein actual message aur `QUIT` conversation end. 2xx success, 4xx temporary problem, 5xx permanent problem. SMTP ko inbox retrieval protocol mat likhna.

### Complete English model answer

**Definition.** Simple Mail Transfer Protocol (SMTP) is a command/response Application Layer protocol used to submit and relay electronic mail over TCP networks. It supports sending mail from a client to a submission service and transporting mail between mail transfer agents. It differs from POP3 and IMAP, which are designed for accessing mailboxes.

**TCP ports and network role.** Server-to-server mail relay traditionally uses TCP port 25. Authenticated message submission commonly uses TCP port 587, and implicit-TLS submission commonly uses port 465. Actual deployment may use nonstandard ports and service policies. SMTP's ability to queue messages makes it compatible with the store-and-forward email model.

**Command-and-reply design.** After a client connects, a server sends an opening greeting (often `220`). The client identifies itself with `EHLO` and learns supported extensions. The sender announces an envelope sender with `MAIL FROM` and one or more envelope recipients with `RCPT TO`. `DATA` begins content transfer after the server responds with an intermediate status such as `354`. A line containing only a dot terminates the message data in the traditional SMTP stream representation (represented as `<CRLF>.<CRLF>`). The server then accepts or rejects the transaction, and `QUIT` ends the connection.

```text
       SMTP CLIENT                                 SMTP SERVER
           |<--------------------------------- 220 ready
           |----------- EHLO sender.example ------->|
           |<--------------------------------- 250 info
           |----------- MAIL FROM:<a@x.example> -->|
           |<--------------------------------- 250 OK
           |----------- RCPT TO:<b@y.example> ---->|
           |<--------------------------------- 250 OK
           |----------- DATA --------------------->|
           |<--------------------------------- 354 input
           |----------- headers + message body --->|
           |----------- final <CRLF>.<CRLF> ------>|
           |<--------------------------------- 250 accepted
           |----------- QUIT --------------------->|
           |<--------------------------------- 221 goodbye
```

**Important SMTP commands.** `EHLO` is the normal greeting for extended SMTP; `HELO` is the older basic greeting. `MAIL FROM` and `RCPT TO` identify envelope participants. `DATA` transfers content. `RSET` cancels/reset the current mail transaction. `NOOP` requests no operation; `QUIT` closes the session. `STARTTLS` requests a secure channel upgrade when supported and configured. Authentication is provided through appropriate SMTP extensions and policies, not through arbitrary unprotected password sharing.

**Reply-code meaning.** A response beginning with **2** normally indicates success, **3** indicates more information needed, **4** means a temporary negative response that may be retried, and **5** means a permanent negative response for the requested action. Examples include `220` ready, `250` OK/accepted, `354` start input, `421` service unavailable, `550` mailbox or policy refusal, and `221` closing.

**Envelope versus message header.** SMTP's envelope fields are used for transaction delivery. The header fields visible in a mail reader need not exactly match the envelope addresses. This is particularly relevant for forwarding, mailing lists and bounce handling.

**Mail queue and reliability.** When the receiving server is unavailable, a transmitting MTA may queue mail and retry delivery according to configured policies. A temporary error is not necessarily an immediate bounce. Delivery can still fail permanently, so SMTP cannot guarantee that a human recipient has seen the email.

**Security.** Traditional SMTP was designed without a universal encrypted channel. TLS can protect supported links; authentication can restrict message submission. SPF, DKIM and DMARC contribute domain-level identity assessment but do not themselves encrypt an email's contents. User security still requires strong accounts, careful attachment handling and anti-phishing awareness.

**Advantages and limitations.** SMTP is widely interoperable, supports routing and relay, allows asynchronous delivery and can serve large-scale email systems. It is not a mailbox retrieval protocol, cannot by itself prevent spam or phishing, and may be subject to queuing delays, rejection and configuration errors.

**Conclusion.** SMTP is the central protocol for Internet mail transfer. Its structured handshake, envelope commands, data phase and status codes make it an excellent example of application-level protocol design.

## Q6. Explain POP3, IMAP and MIME. Compare the working of POP3 and IMAP with suitable examples. (20 marks)

### Hinglish understanding before you write

Student ke email inbox ko locker samjho. POP3 mein focus mail retrieve karne par hai; IMAP mein server mailbox ka state, folders aur multiple devices par sync important hai. MIME email content ke andar PDF/image jaise parts represent karta hai. POP3, IMAP aur MIME ko SMTP ki jagah mat likhna. Diagram mein mail server se two devices ka connection banao.

### Complete English model answer

**Introduction.** Electronic mail is not managed by a single protocol. The message is submitted and transferred using SMTP, while client applications access received mail through **POP3** or **IMAP**. The **MIME** standard permits messages to represent attachments and alternative body types. These three facilities solve different but complementary problems.

**POP3 definition and operation.** Post Office Protocol version 3 is a relatively simple mailbox-access protocol associated with downloading messages from a server. The conventional TCP port is 110, and implicit-TLS POP3 commonly uses port 995. A POP3 client opens a connection, authenticates, inspects mailbox message information, retrieves selected messages and terminates the session. Its three standard conceptual phases are **Authorisation**, **Transaction** and **Update**. Representative commands are `USER`, `PASS`, `STAT`, `LIST`, `RETR`, `DELE` and `QUIT`. The server answers with positive or negative status replies such as `+OK` and `-ERR`. Actual clients may leave messages on the server or delete them according to account settings; server deletion is not an unavoidable characteristic of POP3.

**IMAP definition and operation.** Internet Message Access Protocol supports richer interaction with messages and folders that remain on the mail server. Its conventional TCP port is 143, and implicit-TLS IMAP commonly uses port 993. A client authenticates, selects a mailbox, requests message information, marks flags, searches, copies or moves messages using supported commands, and logs out. Server-side mailbox state helps keep mobile, web and desktop mail applications consistent. Important commands include `SELECT`, `FETCH`, `SEARCH`, `STORE`, `COPY`, `EXPUNGE` and `LOGOUT`.

```text
                   +-------------------------+
                   |  CENTRAL MAIL STORAGE   |
                   | INBOX / SENT / FOLDERS  |
                   +------------+------------+
                                |
                     +----------+---------+
                     |                    |
                 IMAP access           IMAP access
                     |                    |
                +----+----+          +----+-----+
                | Phone   |          | Laptop   |
                +---------+          +----------+

     SMTP delivers mail TO store; IMAP/POP3 access mail FROM store.
```

**Comparison.** POP3 is retrieval-oriented and traditionally simple. IMAP is server-state-oriented, offering folders, flags, multi-device use, and more selective access to message contents. Both can be configured to use secure connections. POP3 can retain server copies, and IMAP can cache messages for offline reading. Therefore simplistic statements like “POP3 always deletes everything” are misleading.

**MIME definition.** Multipurpose Internet Mail Extensions extends email representation for character encodings, attachments, multimedia bodies and multipart composition. It uses headers such as `MIME-Version`, `Content-Type`, `Content-Transfer-Encoding` and `Content-Disposition`. A `multipart/mixed` message can contain a plain-text body and a PDF attachment as separate parts separated by a defined boundary. Binary data may be represented with Base64, but Base64 is **encoding, not encryption**.

```text
EMAIL MESSAGE
+----------------------------------------------+
| From, To, Subject and MIME headers           |
+----------------------------------------------+
| MIME multipart/mixed                         |
|    Part A: text/plain introduction           |
|    Part B: application/pdf attachment        |
+----------------------------------------------+
```

**Working example.** An instructor submits a message with an assignment PDF. SMTP transfers the message to the recipient's mail system. MIME allows the receiving application to distinguish text from the PDF attachment. The recipient's phone accesses mailbox state through IMAP; another user with a simple desktop client may retrieve messages using POP3.

**Advantages and limitations.** POP3 is easy to understand and often convenient for local retrieval. IMAP is well suited to organised mailboxes and synchronisation, but richer state and server storage require more coordination. MIME enables widespread multimedia email but creates size overhead, and its attachment representation must not be mistaken for malware protection.

**Conclusion.** POP3 and IMAP provide the receiving side of mail access, while MIME defines message content. Together with SMTP, they form the core of the traditional Internet email system.

## Q7. What is a virtual terminal? Explain TELNET, Network Virtual Terminal (NVT), its working and comparison with SSH. (20 marks)

### Hinglish understanding before you write

Maan lo tum college lab ke server ke saamne physically nahi ho. Tum laptop par terminal khol kar server ka shell use kar sakte ho. TELNET remote text interaction ka traditional protocol hai. Different host terminal formats ko common form dene ke liye NVT ka use hua. Password protection ke liye today SSH better example hai.

### Complete English model answer

**Introduction and definition.** A virtual terminal is a logical representation of a computer terminal that allows a user to communicate interactively with a remote computer through a network. The user sees a local interface for entering commands and viewing results, while the commands are actually interpreted by a remote login service or program. The key concept is that user interaction can be separated from physical proximity to the host computer.

**TELNET.** TELNET is a traditional Application Layer protocol for interactive remote-terminal access. It commonly runs over TCP port 23 and carries user inputs, server outputs and protocol-control messages. It is an older protocol widely discussed in computer-networking textbooks. TELNET is usually not appropriate for transmitting sensitive credentials because its standard communication is plaintext unless protected by some separate mechanism.

**Architecture.** A local terminal client captures keystrokes and sends them through a network connection. The server-side TELNET service maps the incoming representation to the remote host's expected terminal behaviour. Application or operating-system output is translated back into terminal data and delivered to the user's display.

```text
 +------------------+      NETWORK      +---------------------+
 | Local keyboard & |                  | Remote TELNET        |
 | screen + client  |<---------------->| server / terminal   |
 | emulation        |                  | adapter + shell      |
 +------------------+                  +---------------------+
           |                                       |
   user enters 'ls'                     host executes command
           |<----------- output -------------------|
```

**Network Virtual Terminal (NVT).** The NVT is a conceptual standard representation introduced by TELNET to enable systems with different native terminal conventions to communicate. The local terminal's characters and control operations are mapped into a common network form; the receiving side translates the form for the destination. It is associated with a basic ASCII-oriented terminal representation and defined control conventions. Thus NVT serves as an intermediary format rather than requiring every system to understand every other system's local terminal type.

**Option negotiation.** TELNET supports special protocol commands that negotiate terminal operation. The Interpret As Command byte (IAC) introduces control/option messages. Negotiation forms include `DO`, `DON'T`, `WILL` and `WON'T`. An endpoint can request or refuse an option, such as certain echo behaviour. This flexibility helped accommodate heterogeneous terminal systems.

**Working sequence.** (1) The user launches a terminal client and specifies the server. (2) A TCP connection is made to the TELNET service. (3) The endpoints exchange any required option-negotiation messages. (4) The server presents a login or service prompt. (5) The user sends text/commands. (6) The server performs permitted operations and returns output. (7) The user terminates the remote session or the host closes it.

**Uses.** Historical uses include accessing remote minicomputers, shared university machines, interactive text systems and network devices. TELNET may still appear in controlled educational environments, and simple TCP testing has sometimes used terminal-like clients. These uses should not be interpreted as reasons to expose unprotected TELNET on the public Internet.

**TELNET versus SSH.** SSH (Secure Shell) provides secure remote interactive access with encryption and server authentication when configured correctly. SSH conventionally uses TCP port 22. TELNET generally lacks comparable built-in protection. Both can carry a remote terminal-like interaction, but SSH is the modern preferred tool for remote administration. SFTP is an associated SSH-based file-transfer protocol and is separate from TELNET.

**Advantages.** Virtual terminals reduce the need to travel to a remote computer, enable centralised management, allow interactive troubleshooting and bridge different terminal types. **Limitations.** Remote sessions depend on network availability and server permission. TELNET is insecure on untrusted networks; remote shell access must also be carefully authorised and logged.

**Conclusion.** The virtual-terminal concept enables interactive network access to distant computers. TELNET and NVT are historically important standardisation mechanisms, while SSH supplies the security required for most current remote administration.

## Q8. Define Internet. Explain its architecture, TCP/IP working and major services with a labelled diagram. (20 marks)

### Hinglish understanding before you write

Internet ek single big computer ya ek company ka network nahi hai; yeh *networks of networks* hai. Student ke phone se college website tak data Wi-Fi router, ISP, aur different routers ke through jaata hai. Application Layer request format define karti hai, IP destination addressing deta hai, routers forwarding karte hain. WWW Internet ki ek service hai, Internet itself nahi.

### Complete English model answer

**Definition.** The Internet is a worldwide interconnected system of computer networks using the Internet Protocol suite and related technical standards to exchange information. It joins independent networks operated by households, educational institutions, businesses, service providers and governments. Because it connects networks managed by many organisations, the Internet is often called a **network of networks**.

**Major components.** End systems or hosts, including phones, laptops and servers, run applications. Local/access networks connect them to their first router or gateway. Internet Service Providers offer access to interconnected regional and global networking infrastructure. Routers forward IP packets using routing tables. Separate administrative networks, often called Autonomous Systems, exchange reachability information and traffic by policy. DNS helps applications locate named services.

```text
    UNIVERSITY / HOME LAN                PROVIDERS                 DESTINATION
+----------------------+           +-----------------+         +------------------+
| Laptop + mobile      |--Wi-Fi---->| Access ISP      |-------->| Transit / cloud  |
| Ethernet / wireless  |           | Edge routers    |         | network / server |
+----------------------+           +--------+--------+         +------------------+
                                            |
                                     Other ISPs / ASes
                                            |
                                       Global Internet
```

**TCP/IP protocol architecture.** The Application layer includes protocols such as HTTP, SMTP, FTP and DNS. The Transport layer includes TCP and UDP; TCP offers an ordered, reliable byte stream, while UDP supplies datagram transport without built-in delivery guarantees. The Internet layer uses IPv4/IPv6 for logical addresses and packet forwarding. The Link/Physical layers provide technology for local frame transmission, such as Ethernet, Wi-Fi and fibre connections.

**How a web request travels.** When a student enters a domain name, the client obtains relevant network addressing information, often using DNS. It establishes the necessary transport/protocol state, forms an application request and sends data into the network stack. IP packets traverse a local gateway and possibly many ISP/backbone routers. The destination server processes the request, creates a response, and transmits it back. The browser interprets returned data and presents the result.

**Major Internet services.** The World Wide Web uses HTTP/HTTPS to retrieve web resources. Electronic mail uses SMTP for transfer and IMAP/POP3 for mailbox access. File transfer uses protocols such as FTP and SFTP. Remote administration can use SSH. Domain naming commonly uses DNS. Additional services include online learning, video communication, cloud applications, online transactions and collaboration.

**Internet versus World Wide Web.** The Internet is the underlying network infrastructure. The Web is an application system of resources, URLs, browsers and HTTP-based interaction running over that infrastructure. Email and remote shell connections demonstrate that not every Internet service is a web page.

**Characteristics.** Internet infrastructure is heterogeneous, packet-switched, scalable and decentralised in administration. Packets can traverse multiple independent networks. IP delivery is best-effort; delays, loss and congestion can happen. Transport and application protocols may add appropriate reliability and security mechanisms.

**Benefits.** The Internet enables rapid communication, information sharing, distance education, electronic commerce, software delivery, business collaboration and access to remote services. **Challenges** include cyberattacks, privacy loss, misinformation, uneven access, service outages, malicious software and network congestion.

**Conclusion.** The Internet provides the shared network foundation for Application Layer services. It does not replace FTP, SMTP, HTTP or similar protocols; it enables these services to exchange data across interconnected networks worldwide.

## Q9. What are public networks? Explain their types and compare public/private networks and circuit/packet switching. (20 marks)

### Hinglish understanding before you write

Public transport bus multiple customers ke liye service hai; college ki internal shuttle limited members ke liye. Public telecom networks ka similar broad idea hai. PSTN historical telephone connection example, public data networks packet service example, mobile networks aur ISP Internet access modern examples. Public ka meaning automatically “no password” ya “free” nahi.

### Complete English model answer

**Introduction.** A public network is a communication network or telecommunications service made available by an operator to a broad set of customers or subscribers subject to its access policies and commercial or public-service terms. Such networks may support voice, data or multimedia. They can use diverse media and switching technologies and may interconnect with private networks or the global Internet.

**Major types.** **(1) Public Switched Telephone Network (PSTN):** the conventional public telephone system, historically based on circuit switching and telephone exchanges. **(2) Public Data Network (PDN):** a provider-operated system designed for data service, historically including packet-switched X.25 networks. **(3) ISDN:** an integrated digital communications service discussed in traditional telecommunications courses. **(4) Public Internet access network:** fixed broadband and ISP infrastructure providing access to global IP services. **(5) Cellular network:** provider-operated wireless access and core networks for subscriber mobile communications.

```text
                 PUBLIC NETWORK PROVIDER
        +---------------------------------------+
        | switching / routing / service system  |
        |                                       |
        |  Customer A ---- core ---- Customer B |
        |          \               /           |
        |           Internet access             |
        +---------------------------------------+
           |                                 |
     Subscriber line                    Mobile / business link
```

**Circuit switching.** A traditional circuit-switched system establishes a communication path or allocated circuit resources before data exchange. After setup, a call uses the established resource until release. It can provide a relatively predictable circuit service but may waste reserved capacity when the participants are silent. The PSTN call is the textbook example, though modern voice infrastructures have evolved.

**Packet switching.** In a packet-switched network, data is divided into packets sharing transmission links and forwarding resources. Packets contain addressing/control information and are forwarded by network devices. This approach is efficient for bursty traffic and permits statistical multiplexing, but it may suffer congestion, packet loss, queueing and variable delay. Internet IP packet forwarding is a key example.

**Public versus private networks.** A private network is intended for a restricted group, such as a college lab or an organisation's internal office network. Access is controlled by organisation policy rather than a general subscriber offer. Public and private systems can use similar technology, and private systems may connect to public ones through secured gateways or VPN services.

| Basis | Public network | Private network |
|---|---|---|
| Access | Provided to subscribers/customers | Restricted organisational users |
| Management | Service provider | Organisation or owner |
| Example | ISP access, phone service | College staff internal LAN |
| Security | Depends on provider and end-to-end measures | Depends on internal controls |
| Size | Local through global | Small or geographically distributed |

**Public network versus Internet.** A public telephone provider can offer network services that are not identical to the Internet. The Internet is a global interconnection of IP networks; a subscriber-facing public access service can be one of the ways to reach it. A private enterprise network can likewise be connected to the Internet.

**Advantages.** Public networks support access at scale, shared infrastructure cost, nationwide or international communication, mobility and readily available customer connectivity. **Disadvantages/challenges** may include shared-resource congestion, subscription costs, provider outages, regulatory requirements, security exposure and uncertain performance beyond the provider's own network.

**Historical context.** Older data-network technologies such as X.25, Frame Relay and ATM illustrate how public data services evolved. X.25 emphasised network error-management in older WAN environments, Frame Relay simplified frame-based service, and ATM used 53-byte fixed-size cells. They should be introduced as historical context, not confused with the Internet's IP packet format.

**Conclusion.** Public networks connect subscribers through shared provider infrastructure. Understanding public/private access models and circuit/packet switching explains why different telecommunications services have different cost, efficiency, delay and management characteristics.

## Q10. Explain how file transfer, electronic mail, virtual terminals and the Internet work together in a practical network. Compare the major protocols. (20 marks)

### Hinglish understanding before you write

Ek complete college scenario likho: file upload, email notification, remote server login, browser/Internet. Har step ka **alag protocol aur purpose** clearly show karo. Is answer mein Unit V ka full revision ho jayega aur 20-mark answer mein architecture diagram ko major weight do.

### Complete English model answer

**Introduction.** A practical computer network commonly provides several Application Layer services simultaneously. File transfer moves or manages documents, email delivers asynchronous messages, virtual terminals permit interactive remote access, and the Internet provides interconnection between devices and organisational networks. The four headings are related but have distinct protocol responsibilities.

**Architecture and example.** Consider a college that provides an assignment submission service, an electronic-mail system, an instructor download area and a remotely administered Linux server. Students access these services through campus LANs or an ISP. The network includes switches, local routers, service gateways and Internet connectivity.

```text
                   CAMPUS / INTERNET APPLICATION SERVICES

   STUDENT LAPTOP                          COLLEGE OR HOSTED SERVICES
 +------------------+                   +------------------------------+
 | Browser          |--- HTTP(S) ------>| Web portal                   |
 | File client      |--- FTP/SFTP ----->| File repository              |
 | Mail client      |--- SMTP -------->| Mail submission/relay        |
 | Mail client      |<-- IMAP/POP3 ---->| Mailbox store                |
 | Terminal client  |--- SSH/TELNET --->| Remote server / shell        |
 +--------+---------+                   +------------------------------+
          |                                             ^
          +---------- Wi-Fi / ISP / IP routers ----------+
                        DNS name discovery as needed
```

**First service: file transfer, access and management.** A student prepares `assignment.pdf` locally and uploads it to an authorised repository. Transfer software must choose the destination, authenticate, move bytes and report completion. A file-management service may then allow a directory listing, rename or deletion. FTP illustrates the classic two-connection model: commands travel through a TCP control connection and file bytes through a separate data connection. SFTP is a secure SSH-based alternative, while FTAM is a historically important OSI file-services framework.

**Second service: electronic mail.** The student composes a notification to the instructor. SMTP submits/relays the message, and receiving mail infrastructure stores it in a mailbox. The instructor uses IMAP or POP3 to access the mail later. MIME represents the email body and attached PDF. Email is asynchronous, so the sender and instructor need not be online together. Transport encryption and user authentication are required to protect sensitive messages in appropriate deployments.

**Third service: virtual terminal.** Suppose the file repository stops working. An authorised technician opens a remote terminal session and views service status or logs. TELNET is the classic virtual-terminal protocol and uses the NVT concept to standardise terminal interaction. SSH provides encryption and server authentication, making it the more suitable modern protocol for remote system management. Authentication and least-privilege permissions are essential.

**Fourth service: Internet and public networks.** Students outside campus connect through their ISP and access the college resources through a chain of IP routers. The Internet carries packets across independent networks. DNS helps locate named service hosts, while the selected application protocol defines the operation the user performs. A public ISP network offers subscriber access; the college's internal LAN may remain private.

**Protocol comparison.** FTP/SFTP primarily handles files; SMTP sends mail; IMAP/POP3 accesses mail; MIME defines message content; TELNET/SSH supports remote sessions; HTTP(S) accesses web resources; DNS resolves service names; IP handles addressing and forwarding. These functions are complementary, not interchangeable. For example, MIME cannot replace SMTP and SSH cannot replace a mail transfer agent.

**Security and management issues.** Each service should authenticate users appropriately, restrict file and terminal privileges, protect sensitive transport, log relevant activity and recover from errors. Plain FTP and TELNET present significant confidentiality risks. Mail services need controls against spam and spoofing. Public Internet access introduces variable path performance and exposure to malicious traffic.

**Benefits.** Integrated network services support collaboration, flexible remote access, centralised resources, quick information exchange and efficient administration. **Limitations** include dependency on connectivity, interoperability challenges, server failures and the need for careful access/security controls.

**Conclusion.** Unit V demonstrates that one common network can support many different application functions. The Application Layer protocol determines what users can do, while the lower-layer Internet and provider infrastructure supplies the transport environment.

# PART C - QUESTIONS, SHORT ANSWERS, EXERCISES AND REVISION

## 14. Forty Important 2-Mark Questions with Answers

**1. What is the Application Layer?**  
The Application Layer is the top layer of the OSI model that specifies network services and communication rules used by applications, including file transfer, email and remote terminal access.

**2. What are the two main classes of Application Layer interaction?**  
Common architectures are client-server, where clients request server services, and peer-to-peer, where participants can both request and provide resources. Hybrid architectures also exist.

**3. Define file transfer.**  
File transfer is copying file contents from one networked system to another while handling protocol requests, permissions, status and completion.

**4. What is FTAM?**  
File Transfer, Access and Management is a historical OSI Application Layer framework designed for file transfer, remote content access and file-management operations across systems.

**5. Is FTAM the same as FTP?**  
No. FTAM belongs to the OSI file-service framework, while FTP is a separate Internet protocol commonly used for remote file transfer and selected management commands.

**6. What is FTP?**  
File Transfer Protocol is an Application Layer protocol using TCP for transferring files and executing selected remote directory/file operations between client and server.

**7. Why does FTP use two connections?**  
Classic FTP separates the control connection for commands/replies from a data connection that carries file contents or directory listings.

**8. State FTP's conventional control port.**  
The standard FTP server control port is TCP 21. The data connection uses its own separate endpoint according to active/passive mode.

**9. What is the purpose of `RETR`?**  
`RETR filename` requests that an FTP server transfer the specified remote file to the client.

**10. What is the purpose of `STOR`?**  
`STOR filename` requests that the FTP server store uploaded file data under the specified remote filename, subject to permissions.

**11. Distinguish active and passive FTP in one line.**  
In active FTP the server initiates the data connection to the client; in passive FTP the client initiates the data connection to a server-advertised port.

**12. What is SFTP?**  
SFTP is an SSH-based file transfer protocol providing remote file operations over a protected SSH channel; it is distinct from FTP/FTPS.

**13. What is FTPS?**  
FTPS is FTP protected with TLS, provided the required TLS negotiation, certificate validation and data protection are correctly configured.

**14. Define electronic mail.**  
Electronic mail is an asynchronous network communication service used to compose, transmit, store and retrieve digital messages.

**15. What is SMTP?**  
Simple Mail Transfer Protocol is the Application Layer protocol chiefly used to submit and relay outgoing Internet email.

**16. What are common SMTP ports?**  
TCP 25 is traditionally used for server-to-server SMTP relay; 587 for authenticated submission; and 465 for implicit-TLS submission in many deployments.

**17. What is a Mail User Agent?**  
A Mail User Agent is the email client or interface through which a user composes, submits, reads and organises messages.

**18. What is a Mail Transfer Agent?**  
A Mail Transfer Agent processes and transfers email messages between mail servers, commonly using SMTP.

**19. What is store-and-forward communication?**  
A message can be received and stored at an intermediate server before being forwarded or delivered later, so both endpoints need not be online simultaneously.

**20. What is POP3?**  
Post Office Protocol version 3 is a relatively simple email mailbox retrieval protocol associated with downloading messages from a server.

**21. What is IMAP?**  
Internet Message Access Protocol allows a mail client to access and manage server-side mailboxes, including folders, flags and messages.

**22. POP3 versus IMAP?**  
POP3 emphasises simple message retrieval; IMAP is better suited to synchronised access and organisation of mailboxes across devices.

**23. What is MIME?**  
Multipurpose Internet Mail Extensions defines message-representation features for attachments, multimedia content, character encodings and multipart email bodies.

**24. Does MIME encrypt attachments?**  
No. MIME organises/encodes message content. Base64 used in MIME is an encoding mechanism, not encryption.

**25. What is the purpose of an MX record?**  
A DNS MX record specifies mail exchange information used to locate the receiving mail infrastructure for a domain.

**26. What is the SMTP `DATA` command?**  
It asks the server to accept the message headers/body after SMTP envelope details have been supplied; a positive intermediate reply such as `354` typically precedes the content.

**27. What do SMTP 4xx and 5xx replies indicate?**  
4xx replies represent temporary negative responses that may be retried; 5xx replies represent permanent negative responses for the attempted action.

**28. Define virtual terminal.**  
A virtual terminal is a standardised logical interface enabling a user to interact with a remote computer through a network rather than a directly attached terminal.

**29. Define TELNET.**  
TELNET is a traditional Application Layer remote-terminal protocol, normally using TCP port 23, that carries interactive terminal data and option negotiation.

**30. What is NVT?**  
Network Virtual Terminal is TELNET's standardised intermediary representation for terminal data/control, enabling communication among systems with different native terminal conventions.

**31. What are TELNET's IAC and negotiation commands?**  
IAC introduces TELNET protocol-control messages; negotiation uses command names such as `DO`, `DON'T`, `WILL` and `WON'T`.

**32. Why is SSH usually preferred to TELNET?**  
SSH provides encryption and server authentication when correctly used, whereas ordinary TELNET generally sends passwords and commands in readable plaintext.

**33. What is the Internet?**  
The Internet is a global system of interconnected IP networks operated by many organisations and communicating using the TCP/IP protocol suite.

**34. What is an ISP?**  
An Internet Service Provider supplies customers with network connectivity and services that can provide access to the global Internet.

**35. Internet versus WWW?**  
Internet is the underlying network of networks; WWW is an application/information service built on that network using URLs, browsers and web protocols.

**36. What is DNS?**  
Domain Name System is a distributed hierarchical naming system that supplies records such as IP-address, alias and mail-exchange information for domain names.

**37. What is a URL?**  
A Uniform Resource Locator identifies a resource and describes how it can be accessed, for example using an HTTPS scheme, hostname and path.

**38. What is a public network?**  
A public network is a communications service made available to customers/subscribers by a provider, subject to its access policies and service terms.

**39. What is PSTN?**  
Public Switched Telephone Network is the traditional public telephone system, historically explained using circuit-switched telephone connections.

**40. Distinguish packet and circuit switching.**  
Circuit switching traditionally establishes reserved connection resources for a call. Packet switching divides data into packets and shares network links among many communications.

## 15. Important 5-Mark Questions - Answer Blueprints

1. **Write five Application Layer functions.** Define the layer; describe protocol rules, remote access, file services, email and interoperability; add protocol examples.
2. **FTP architecture.** Draw client/server blocks, control TCP to port 21, separate data connection; explain their distinct responsibilities.
3. **FTP commands.** Explain `USER`, `PASS`, `CWD`, `LIST`, `RETR`, `STOR`, `QUIT` and their roles in login and transfer.
4. **Active versus passive FTP.** Draw both data directions; explain NAT/firewall implications and why passive mode is common.
5. **FTAM operations.** Separate transfer, access and management; write one practical example of each.
6. **Email components.** Define MUA, MSA, MTA, MDA, mailbox and access service; show one-line flow.
7. **SMTP working.** Explain `EHLO`, `MAIL FROM`, `RCPT TO`, `DATA`, `QUIT` with a short exchange.
8. **POP3 versus IMAP.** Compare server state, folder support, multi-device access and conventional ports.
9. **MIME.** Give definition, `Content-Type`, multipart, Base64 and PDF attachment example.
10. **Virtual terminal and NVT.** Define terminal abstraction and draw local/NVT/remote mapping.
11. **TELNET.** Include definition, port, login steps and plaintext security limitation.
12. **TELNET versus SSH.** Compare encryption, server authentication, ports and use cases.
13. **Internet architecture.** Define network of networks; show host -> LAN -> ISP -> backbone -> server.
14. **Internet versus WWW.** Explain infrastructure/service difference with HTTP and email examples.
15. **Public networks.** State meaning and examples such as PSTN, public packet data services and ISP access.
16. **Circuit versus packet switching.** Draw two simple diagrams and compare resource use and delay.
17. **DNS service.** Describe domain lookup, A/AAAA/MX records, cache and browser use.
18. **HTTP versus HTTPS.** Define request/response and explain secure transport and conventional ports.
19. **Remote file management risks.** Explain permissions, concurrent edits, integrity checking and interrupted transfers.
20. **Email security problems.** Discuss spoofing, spam, phishing, attachments, TLS and authentication.

## 16. Important 10-Mark Questions - Expanded Writing Patterns

**A. FTP working and architecture (10 marks).** Write FTP definition (1), two-connection diagram (2), seven step flow (4), commands (2), security note (1). Do not draw the data connection as the exact same channel as the control connection.

**B. Email architecture and SMTP (10 marks).** Define email and asynchronous store-and-forward (2), draw sender MUA -> SMTP -> receiving MTA -> mailbox -> IMAP/POP3 -> recipient (3), describe journey (4), security (1).

**C. SMTP versus POP3 versus IMAP (10 marks).** Introduction (1), explain each purpose with ports (3), six-row comparison table (4), example with sender/receiver (2).

**D. MIME message format (10 marks).** Define MIME (2), explain common headers (2), multipart diagram (3), show attachment flow and limitations (3).

**E. TELNET/NVT with diagram (10 marks).** Define virtual terminal and TELNET (2), draw NVT mapping (3), explain login/interaction (3), TELNET limitations and SSH comparison (2).

**F. Internet and public networks (10 marks).** Define Internet (2), architecture diagram (3), TCP/IP layers and services (3), contrast private/public and WWW (2).

**G. File transfer, access and management (10 marks).** Definition/FTAM (2), three categories with examples (4), architecture (2), permissions/failure design issues (2).

**H. Public network switching (10 marks).** Public-network definition (2), types (2), circuit and packet diagrams (2), comparison/strengths and limits (4).

**I. FTP, FTPS and SFTP (10 marks).** Define each (3), compare how each protects data and handles connections (4), use cases and security caution (3).

**J. Application Layer services with college example (10 marks).** Definition and protocol diagram (2), file/mail/virtual-terminal explanations (6), Internet infrastructure connection (2).

## 17. Solved Application-Oriented Numericals and Practical Exercises

> **Important:** Unit V is principally a theory/protocol unit. These are extra practice questions to build numerical reasoning and protocol interpretation; no promise is made that the university will ask calculations from these headings. Unless otherwise stated, calculations ignore protocol headers, retransmissions and congestion delays.

### Exercise 1 - file-transfer minimum serialization time

**Question:** How long does it take to transmit a 10 MB file over a 20 Mbps link, ignoring overhead? Use decimal units.

**Solution:** File size = $10 \times 10^6$ bytes = $80 \times 10^6$ bits. Rate = $20 \times 10^6$ bits/s. Time = bits/rate = $80/20 = 4$ seconds. **Answer: 4 s**, under the simplifying assumptions.

### Exercise 2 - smaller file on slower connection

**Question:** Transfer a 750 kB file over a 5 Mbps link using decimal units.

**Solution:** $750{,}000 \times 8 = 6{,}000{,}000$ bits. Time = $6{,}000{,}000 / 5{,}000{,}000 = 1.2$ seconds. **Answer: 1.2 s**.

### Exercise 3 - upload time with binary units

**Question:** A PDF is 24 MiB. Link rate is 8 Mbps. Estimate the serialization time.

**Solution:** Size in bits = $24 \times 2^{20} \times 8 = 201{,}326{,}592$ bits. Time = $201{,}326{,}592 / 8{,}000{,}000 \approx 25.17$ seconds. **Answer: approximately 25.17 s**, excluding overhead.

### Exercise 4 - Base64 size for attachment

**Question:** A 300,000-byte attachment is Base64-encoded. Approximate encoded size before line wrapping.

**Solution:** Base64 represents every 3 input bytes using 4 characters. Because 300,000 is divisible by 3, size = $300{,}000/3 \times 4 = 400{,}000$ bytes. **Answer: 400,000 bytes** plus MIME/line overhead.

### Exercise 5 - Base64 exact padding formula

**Question:** What is the Base64 encoded length of 10 bytes?

**Solution:** General formula $4\lceil n/3\rceil$; $4\lceil 10/3\rceil = 4 \times 4 = 16$ characters, including padding. **Answer: 16 characters**.

### Exercise 6 - useful throughput

**Question:** A 12 MB file takes 15 seconds to transfer. What is the average useful payload rate (decimal units)?

**Solution:** $12 \times 10^6$ bytes = $96 \times 10^6$ bits. Throughput = $96/15 = 6.4$ Mbps. **Answer: 6.4 Mbps**.

### Exercise 7 - unit trap

**Question:** Is 8 MB/s equal to 8 Mbps?

**Solution:** No. **B** is byte and **b** is bit. One byte = 8 bits. $8$ MB/s corresponds to about **64 Mbps**, using decimal units.

### Exercise 8 - number of transferred data blocks

**Question:** A 5,000-byte application file is sent in chunks of at most 1,200 bytes. How many chunks are needed, ignoring overhead?

**Solution:** $\lceil 5000/1200\rceil = \lceil4.1667\rceil = 5$ chunks. Four carry 1,200 bytes; last carries 200 bytes. **Answer: 5 chunks**.

### Exercise 9 - SMTP recipient transaction

**Question:** A sending SMTP server supplies one `MAIL FROM` and three accepted `RCPT TO` commands before `DATA`. Does this necessarily mean three separate message-data bodies are sent?

**Answer:** No. One SMTP mail transaction can declare multiple recipients and transfer one message data body for that transaction. Downstream delivery or splitting may occur according to server handling.

### Exercise 10 - SMTP temporary failure

**Question:** Receiving server returns an SMTP `451` response. Should the sender treat it as definite permanent rejection?

**Answer:** No. A **4xx** reply is a temporary negative completion response; a responsible sender can queue/retry according to its mail policy. Final delivery is still not guaranteed.

### Exercise 11 - which protocol solves which action?

**Question:** A student must (i) send mail, (ii) read the same inbox on phone and laptop, (iii) upload an archive, (iv) securely run a server command. Choose protocols.

**Answer:** (i) SMTP; (ii) IMAP; (iii) SFTP or another authorised file-upload service; (iv) SSH. The services are distinct even when using the same Internet connection.

### Exercise 12 - MIME plus SMTP distinction

**Question:** A mail client creates two MIME parts and passes the result to SMTP. Which technology carries the message between mail servers?

**Answer:** SMTP carries/relays the message. MIME describes the message parts; it does not independently connect mail servers.

### Exercise 13 - FTP mode identification

**Question:** The server replies to a passive-mode request with a listening data endpoint, and the client opens a new TCP connection to it. Is it active or passive FTP?

**Answer:** **Passive FTP**, because the client initiates the TCP data connection toward the server endpoint.

### Exercise 14 - classify FTP commands

**Question:** Identify which FTP commands retrieve, upload, list and change working directory.

**Answer:** `RETR` retrieves; `STOR` uploads/stores; `LIST` lists; `CWD` changes directory.

### Exercise 15 - name resolution versus application request

**Question:** Browser needs the server's IP address and then a web document. Which services may be used?

**Answer:** DNS supplies name-related records, then HTTP/HTTPS requests the web resource. IP forwards packets; DNS does not itself fetch the web page.

### Exercise 16 - transfer time with goodput

**Question:** Effective payload throughput is 10 Mbps. How long should a 25 MB decimal file take, ignoring additional startup delay?

**Solution:** $25\times 10^6 \times 8 = 200 \times 10^6$ bits. Time = $200/10=20$ seconds. **Answer: 20 s**.

### Exercise 17 - MIME expansion comparison

**Question:** A raw 6 MB attachment becomes about 8 MB after Base64. Is this compression?

**Answer:** No. The data size has increased by about $\frac{8-6}{6}\times100=33.33\%$. Base64 is a transport-safe encoding, not a compression or encryption algorithm.

### Exercise 18 - same server, two distinct services

**Question:** A college server hosts SSH on TCP 22 and SFTP via SSH. Does SFTP require an FTP control connection on TCP 21?

**Answer:** No. SFTP is a different protocol using SSH; a conventional SSH-based SFTP deployment does not require an FTP control connection.

### Exercise 19 - HTTP status interpretation

**Question:** Web server returns `404` while DNS succeeded and a TCP/TLS session is established. What does `404` mean?

**Answer:** The server could be contacted, but the target resource was not found according to the application response. It is not a DNS failure or proof that the Internet connection is down.

### Exercise 20 - the complete unit in five decisions

**Question:** For each activity, select the main topic: upload an assignment, send notice email, read inbox on two devices, execute remote commands, connect ISP networks.

**Answer:** File transfer/management; SMTP electronic mail; IMAP mailbox access; SSH/TELNET virtual-terminal service; Internet/public networking with IP routing.

## 18. Essential Protocol Ports and Numbers - Revision Table

| Service | Conventional transport/port | Function |
|---|---|---|
| FTP control | TCP 21 | Control commands |
| Classic FTP active-data convention | TCP 20 on server side historically | Data transfer in classic active cases |
| SSH / SFTP | TCP 22 | Secure remote shell/files |
| TELNET | TCP 23 | Traditional virtual terminal |
| SMTP relay | TCP 25 | Mail server relay |
| DNS | UDP/TCP 53 in conventional operation | Naming queries/responses |
| HTTP | TCP 80 for conventional HTTP/1.1, HTTP/2 deployments | Web traffic |
| POP3 | TCP 110 | Mailbox retrieval |
| IMAP | TCP 143 | Mailbox access |
| SMTP submission | TCP 587 | User mail submission |
| IMAP implicit TLS | TCP 993 | Secured mailbox access |
| POP3 implicit TLS | TCP 995 | Secured mailbox retrieval |
| SMTP implicit-TLS submission | TCP 465 | Secure mail submission |
| HTTPS | 443; TCP for HTTP/1.1/2 and UDP for HTTP/3/QUIC | Protected web traffic |

**Remember:** Port numbers are normal conventions, not a guarantee of what every organisation's deployment uses. Plain FTP and TELNET are not secure merely because a password prompt appears.

## 19. Twenty Common Exam Mistakes and Corrections

1. **Wrong:** Application Layer only means a graphical app. **Correct:** It specifies application protocols/services used by programs.
2. **Wrong:** FTP and FTAM are identical. **Correct:** FTAM is historical OSI file-service terminology; FTP is a distinct Internet protocol.
3. **Wrong:** All FTP information travels through TCP port 21. **Correct:** FTP uses a separate data connection for transfer/listings.
4. **Wrong:** Active FTP means client initiates data TCP. **Correct:** Server initiates the classic active-mode data TCP.
5. **Wrong:** SFTP is FTP with an SSL certificate. **Correct:** SFTP is SSH-based; FTPS is FTP protected using TLS.
6. **Wrong:** FTP passwords are safe because the server authenticates. **Correct:** Plain FTP sends them without transport encryption.
7. **Wrong:** SMTP receives mail on the phone. **Correct:** SMTP transfers outgoing mail; IMAP/POP3 support mailbox access.
8. **Wrong:** Email sender and receiver must be online together. **Correct:** Email is designed for store-and-forward asynchronous transfer.
9. **Wrong:** A successful SMTP reply means the recipient read the message. **Correct:** It indicates an SMTP action/result, not human reading.
10. **Wrong:** An email visible `From:` field is always identical to SMTP envelope sender. **Correct:** They are distinct fields used for different purposes.
11. **Wrong:** POP3 always removes the message from the server. **Correct:** Clients may be configured to retain server copies.
12. **Wrong:** IMAP cannot be used offline. **Correct:** Clients may cache messages while keeping the server as authoritative mailbox storage.
13. **Wrong:** MIME is a mail sending protocol. **Correct:** MIME is a message formatting/representation system.
14. **Wrong:** Base64 protects the secrecy of PDF attachments. **Correct:** Base64 is reversible encoding, not encryption.
15. **Wrong:** TELNET provides safe encrypted remote login. **Correct:** Ordinary TELNET is plaintext; SSH is preferred for secure administration.
16. **Wrong:** Commands like `ls` are TELNET protocol instructions. **Correct:** They are remote shell/OS commands transported during a session.
17. **Wrong:** The Internet and WWW mean the same thing. **Correct:** The Web is an application running over the Internet.
18. **Wrong:** DNS sends your browser's complete web document. **Correct:** DNS supplies naming records; HTTP/HTTPS transfers web resources.
19. **Wrong:** Public network means a government-owned free network. **Correct:** It generally means operator service available to customers/subscribers.
20. **Wrong:** A router forwards packets based on an SMTP or FTP command. **Correct:** It mainly forwards based on network-layer routing/addressing information.

## 20. Seven-Day Revision Plan for Unit V

| Day | Read and understand in Hinglish | Write/practise in English |
|---|---|---|
| 1 | Application Layer and FTAM | Definitions, 3 file-operation categories, Q1/Q2 |
| 2 | FTP dual connection and commands | Draw control/data diagrams, active vs passive; Q3 |
| 3 | Email architecture and SMTP | Draw complete mail flow; SMTP commands; Q4/Q5 |
| 4 | POP3, IMAP, MIME and attachments | POP3/IMAP comparison; MIME tree; Q6 |
| 5 | Virtual terminals, TELNET, NVT, SSH | Draw NVT; TELNET flow; Q7 |
| 6 | Internet, DNS/HTTP context and public networks | ISP/router diagram, packet vs circuit; Q8/Q9 |
| 7 | Full Unit V integration | Write Q10 and two other 20-mark answers without notes |

**Three-pass method:** (1) Read Hinglish and explain it to an imaginary junior student. (2) Read Complete English and underline keywords. (3) Close the PDF and reproduce a labelled diagram plus introduction, main points and conclusion. For exam speed, memorise **purpose + diagram + sequence + advantages/limitations** rather than a single paragraph.

## 21. Unit V Glossary - Important Terms

| Term | Easy exam meaning |
|---|---|
| Application Layer | Networking services/protocols used by software applications |
| Client | Program requesting a remote service |
| Server | Program providing the service to clients |
| Protocol | Agreed message format, meaning, order and rules |
| FTAM | Traditional OSI file transfer, access and management framework |
| FTP | Internet protocol for file transfer and related management |
| Control connection | FTP TCP channel for commands and replies |
| Data connection | FTP TCP channel for file/listing transfer |
| `RETR` | FTP command to retrieve file |
| `STOR` | FTP command to upload/store file |
| Active FTP | Server initiates the data TCP connection |
| Passive FTP | Client initiates the data TCP connection |
| FTPS | FTP with TLS protection |
| SFTP | SSH File Transfer Protocol |
| Metadata | Information about a file, such as size and timestamps |
| MUA | Mail User Agent (mail client) |
| MSA | Mail Submission Agent |
| MTA | Mail Transfer Agent |
| MDA | Mail Delivery Agent |
| SMTP | Protocol for sending/relaying email |
| POP3 | Simple mail retrieval protocol |
| IMAP | Server-side mailbox access/management protocol |
| MIME | Format for multipart/multimedia email content |
| Base64 | Text-safe encoding used for some binary content |
| MX | DNS record identifying mail exchange destination |
| Mailbox | Storage for delivered mail messages |
| SMTP envelope | Transfer-time MAIL FROM and RCPT TO information |
| Virtual terminal | Remote interactive terminal abstraction |
| TELNET | Traditional remote-terminal protocol |
| NVT | Network Virtual Terminal standard representation |
| IAC | Interpret As Command marker in TELNET |
| SSH | Secure Shell remote-access protocol |
| Internet | Global interconnection of IP networks |
| ISP | Internet Service Provider |
| Router | Device forwarding IP packets across networks |
| DNS | Domain Name System |
| URL | Resource location/access identifier |
| HTTP | Hypertext Transfer Protocol |
| HTTPS | Protected HTTP communication |
| PSTN | Public Switched Telephone Network |
| PDN | Public Data Network |
| Intranet | Internal organisation network using Internet technologies |
| Packet switching | Forwarding independent data packets over shared links |
| Circuit switching | Traditional established communication circuit/path |
| Store-and-forward | Receiving/storing before later transfer/delivery |
| Authentication | Verifying a claimed identity |
| Authorisation | Determining permitted actions/resources |

## 22. One-Page Final Examination Checklist

- [ ] I can write a complete definition of Application Layer and its services.
- [ ] I can distinguish FTAM, FTP, FTPS and SFTP correctly.
- [ ] I can explain file transfer, file access and file management separately.
- [ ] I can draw **FTP control and data connections** without notes.
- [ ] I can explain **FTP active vs passive** and know `RETR` and `STOR`.
- [ ] I can draw **complete email architecture** showing MUA, SMTP, MTA, mailbox and mail access.
- [ ] I can list SMTP commands and a complete command-response dialogue.
- [ ] I know common SMTP, FTP, POP3, IMAP, TELNET, SSH ports.
- [ ] I understand **SMTP vs POP3 vs IMAP** in accurate English.
- [ ] I can draw a **MIME multipart email** with text + attachment.
- [ ] I can explain TELNET, NVT, terminal option negotiation and security limitations.
- [ ] I can compare TELNET and SSH in at least six points.
- [ ] I can draw Internet architecture showing LAN, ISP, routers and server.
- [ ] I can explain **Internet vs WWW** and DNS's role.
- [ ] I can define public networks with PSTN, PDN, ISP and cellular examples.
- [ ] I can compare classical circuit and packet switching accurately.
- [ ] I can write **two full 20-mark answers** neatly with diagram, steps, benefits and conclusion.

**End of Unit V.** These notes deliberately follow the wording of the uploaded NBCA-501 syllabus. The practice questions and point allocations are educational recommendations, **not confirmed university past questions**. The supporting Internet topics are enrichment, not replacements for the exact Unit V headings.
