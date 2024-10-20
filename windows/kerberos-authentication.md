---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Kerberos Authentication

<img src="../.gitbook/assets/file.excalidraw (1).svg" alt="" class="gitbook-drawing">

### **AS-REQ**

* The client sends a request with its **username** and **timestamp** which usually encrypted with the **client key**. The **client key** derived from the password of the user and their username.&#x20;

***

### **AS-REP**

* A **session key** encrypted using the **client key**, the session key is shared between the client and the TGS.
* The **Ticket Granting Ticket (TGT)**, encrypted using the KDC key which derived from the krbtgt hash. The **TGT** contains information about the user, the domain, a timestamp, the IP address of the client and the session key&#x20;

***

### **TGS-REQ**

Now that the client has the TGT, it sends a request to the **Ticket Granting Service (TGS).**\
This request includes:

* The **TGT** and the requested service identifier**.**
* &#x20;**username** and **timestamp** which usually encrypted with the **session** **key**.

***

### **TGS-REP**

Before the TGS replies it checks the following

1. The TGT must have a valid timestamp.
2. The username from the TGS-REQ has to match the username from the TGT.
3. The client IP address needs to coincide with the TGT IP address.

if the verification was successful the TGS replies with a TGS-REP request which includes the following messages:

* Another **session key AKA Service Key**, specific to the communication between the client and the application server.
* A **Service Ticket** encrypted with the application server's private key. \
  The Service Ticket includes the following:
  * **Client’s username**.
  * **Client’s IP Address**.
  * **Validity Period**: When the service ticket expires.
  * **Service Session Key.**

***

### **AP-REQ**

The client finally contacts the **Application Server**, sending:

* The **Service Ticket.**
* An encrypted **timestamp**.

***

### AP-REP

The application server decrypts the Service ticket and verifies the username matches the one in the encrypted Service Ticket.

* if the verification was successful, the application server replies with a timestamp encrypted the **service session key**.
