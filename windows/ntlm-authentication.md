# NTLM Authentication

<img src="../.gitbook/assets/file.excalidraw.svg" alt="NTLM Authentication flow" class="gitbook-drawing">

* **Client to Application Server (Negotiation Message)**:\
  The client sends a **negotiation message** containing the **username** to the application server.
* **Application Server to Client (Challenge Message)**:\
  The application server responds with a **challenge message** that includes a **random number (nonce)**.
* **Client to Application Server (Authentication Message)**:\
  The client uses the received **random number** to compute an **NTLM hash** and sends an **authentication message** back to the application server containing the **NTLM hash**.
* **Application Server to Domain Controller (Verify Message)**:\
  The application server forwards the **random number (nonce)**, **username**, and **NTLM hash** to the domain controller to verify the credentials.
* **Domain Controller to Application Server (Approve Message)**:\
  The domain controller checks the **NTLM hash** and responds to the application server with an **approve** or **deny** message based on the authentication outcome.
* **Application Server to Client**:\
  The application server informs the client whether the authentication was successful (approve) or unsuccessful (deny).

