# AQUILA - SalesForce Integration Using JWT Authentication

<span style="color: rgb(0, 0, 0);">Salesforce requires secure communication protocols for authorization and data exchange between external applications and Salesforce orgs. This involves creating digital certificates, configuring external client apps, and establishing secure authentication methods. OpenSSL provides the cryptographic tools needed to generate private keys and self-signed certificates for secure communication over networks.</span>

#### <span style="color: rgb(53, 152, 219);">**Integration Overview**</span>

<span style="color: rgb(0, 0, 0);">This integration supports secure communication through:</span>

- <span style="color: rgb(0, 0, 0);">JWT (JSON Web Token) authentication using digital certificates</span>
- <span style="color: rgb(0, 0, 0);">OAuth authentication with external client apps</span>
- <span style="color: rgb(0, 0, 0);">Self-signed certificates and keystore management</span>

<span style="color: rgb(0, 0, 0);">Organizations can authorize Salesforce CLI commands and establish secure API connections using these authentication methods.</span>

#### <span style="color: rgb(53, 152, 219);">**Compatibility**</span>

- <span style="color: rgb(0, 0, 0);">Supports Salesforce CLI authorization via JWT Bearer Flow</span>
- <span style="color: rgb(0, 0, 0);">Compatible with macOS, Linux, and Windows operating systems</span>
- <span style="color: rgb(0, 0, 0);">Requires OpenSSL for certificate generation</span>
- <span style="color: rgb(0, 0, 0);">Requires Java keytool for keystore conversion (optional)</span>

---

#### **<span style="color: rgb(53, 152, 219);">Installing OpenSSL in your Log Collector</span>**

<span style="color: rgb(0, 0, 0);">OpenSSL is an open-source software library that provides tools and protocols for secure communication over networks. It helps encrypt data so that information like passwords, credit card numbers, and private messages stay secure when sent over the internet.</span>

##### <span style="color: rgb(0, 0, 0);">**Step 1:** </span>

**<span style="color: rgb(0, 0, 0);">In Linux:</span>**

<span style="color: rgb(0, 0, 0);">Install OpenSSL on your system:</span>

```
sudo apt install openssl
```

<span style="color: rgb(0, 0, 0);">[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/L3i0JFSepuB2q3xb-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/L3i0JFSepuB2q3xb-image.png)</span>

**<span style="color: rgb(0, 0, 0);">In Windows:</span>**

<span style="color: rgb(0, 0, 0);">Download the Installer:   
1\. Go to the [Shining Light Productions](https://slproweb.com/products/Win32OpenSSL.html) page and download the "Win64 OpenSSL Light" EXE installer (current version 3.x is recommended).</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/RexuTvIWyeUWsKjA-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/RexuTvIWyeUWsKjA-image.png)<span style="color: rgb(0, 0, 0);">2. Run the Installer: Execute the downloaded file and follow the on-screen instructions.  
3\. Installation Path: It is generally recommended to install in the default location, typically C:\\Program Files\\OpenSSL-Win64.  
4\. Configure System Environment Variable:  
Search for "Environment Variables" in the Windows search bar.  
Click "Edit the system environment variables".  
Click "Environment Variables".  
Under "System Variables", find and select Path, then click "Edit".</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/9T1gjYBZB4wtPsFn-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/9T1gjYBZB4wtPsFn-image.png)<span style="color: rgb(0, 0, 0);">Click "Edit" and add the path to the bin folder, for example: C:\\Program Files\\OpenSSL-Win64\\bin.  
Click OK on all windows to save.</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/Qh3yc64BUl50i6XK-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/Qh3yc64BUl50i6XK-image.png)

##### <span style="color: rgb(0, 0, 0);">**Step 2:** </span>

<span style="color: rgb(0, 0, 0);">Verify OpenSSL installation by running:</span>

- <span style="color: rgb(0, 0, 0);">macOS/Linux: `which openssl`</span>
- <span style="color: rgb(0, 0, 0);">Windows: `where openssl`</span>

<span style="color: rgb(0, 0, 0);">[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/CRikyNPifD9rMwNh-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/CRikyNPifD9rMwNh-image.png)</span>

---

#### <span style="color: rgb(53, 152, 219);">**Creating a Private Key and Self-Signed Digital Certificate**</span>

<span style="color: rgb(0, 0, 0);">A digital certificate and the private key used to sign the certificate are needed to authorize an organization using the `org login jwt` command. While it is strongly advised to utilize a certificate issued by a certifying authority, you can use OpenSSL to generate a self-signed certificate to get started.</span>

<span style="color: rgb(0, 0, 0);">This process produces two files:</span>

- <span style="color: rgb(0, 0, 0);">**server.key** — The private key used when authorizing an org with the `org login jwt` command</span>
- <span style="color: rgb(0, 0, 0);">**server.crt** — The digital certificate uploaded when creating the required external client app</span>

##### <span style="color: rgb(0, 0, 0);">**Step 1:** </span>

<span style="color: rgb(0, 0, 0);">Open a terminal (macOS and Linux) or command prompt (Windows).</span>

##### <span style="color: rgb(0, 0, 0);">**Step 2:** </span>

<span style="color: rgb(0, 0, 0);">Create a directory to hold the generated files and navigate to it:</span>

```
mkdir /Users/jdoe/JWT
cd /Users/jdoe/JWT
```

##### <span style="color: rgb(0, 0, 0);">**Step 3:** </span>

<span style="color: rgb(0, 0, 0);">Create a private key and save it as server.key file:</span>

<p class="callout info"><span style="color: rgb(0, 0, 0);">Remember to change "**<span style="color: rgb(224, 62, 45);">&lt;your password&gt;</span>**" to the password of your choice. The password should be the same with the **server.pass.key** and **server.key**.</span></p>

- <span style="color: rgb(0, 0, 0);">server.pass.key command</span>

```
openssl genpkey -aes-256-cbc -algorithm RSA -pass pass:<your password> -out server.pass.key -pkeyopt rsa_keygen_bits:2048
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/gxAFICkvqAnbLuFb-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/gxAFICkvqAnbLuFb-image.png)

- <span style="color: rgb(0, 0, 0);">server.key command</span>

```
openssl rsa -passin pass:<your password> -in server.pass.key -out server.key
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/XYGkDs7rtCtOLrQ5-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/XYGkDs7rtCtOLrQ5-image.png)

##### <span style="color: rgb(0, 0, 0);">**Step 4:**</span>

<span style="color: rgb(0, 0, 0);"> Use the server.key file to create a certificate signing request and save it as server.csr:</span>

<p class="callout info"><span style="color: rgb(0, 0, 0);">When prompted, provide your organization’s details. Enter only the **Country Name, State or Province, Locality, and Organization Name**—you may leave all other fields blank.</span></p>

<p class="callout danger"><span style="color: rgb(0, 0, 0);">**Do not enter a password when generating the `server.csr`, as it may cause an authentication mismatch.**</span></p>

```
openssl req -new -key server.key -out server.csr
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/AT6jLZGT9RFXtGPD-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/AT6jLZGT9RFXtGPD-image.png)

##### <span style="color: rgb(0, 0, 0);">**Step 5:** </span>

<span style="color: rgb(0, 0, 0);">Create a self-signed digital certificate using the server.key and server.csr files:</span>

```
openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/Ou5PwZU64T6JlbTj-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/Ou5PwZU64T6JlbTj-image.png)

##### **Step 6:**  


<span style="color: rgb(0, 0, 0);">Clone the server.key file and save it as server.pem</span>

<p class="callout info">**Important step to successfully integrate into SIEM** </p>

```
cp server.key server.pem
```

---

#### <span style="color: rgb(53, 152, 219);">**Creating an External Client App in Your Salesforce Organization**</span>

<span style="color: rgb(0, 0, 0);">Salesforce CLI requires an external client app in the org that you're authorizing. An external client app is a packageable framework that enables a third-party application (Salesforce CLI) to integrate with Salesforce using APIs and security protocols. You must create your own external client app when authorizing the org with the `org login jwt` command.</span>

##### <span style="color: rgb(0, 0, 0);">**Step 1:** </span>

<span style="color: rgb(0, 0, 0);">Log in to your Salesforce Organization.</span>

<p class="callout info"><span style="color: rgb(0, 0, 0);">Note: If the salesforce dashboard interface is in classic mode change it to lighting mode. </span></p>

- **<span style="color: rgb(0, 0, 0);">In the Upper Right Corner click the gear icon.</span>**

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/Kyqkx6MxXwwySPOm-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/Kyqkx6MxXwwySPOm-image.png)

- <span style="color: rgb(0, 0, 0);">**Select salesforce go.**</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/6srXkx2hmsPpTqtE-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/6srXkx2hmsPpTqtE-image.png)

##### <span style="color: rgb(0, 0, 0);">**Step 2:** </span>

<span style="color: rgb(0, 0, 0);">From the Quick Find box in Setup, enter **App Manager**, then click **App Manager**.</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/hXfeVemGvOeNRe6b-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/hXfeVemGvOeNRe6b-image.png)

##### <span style="color: rgb(0, 0, 0);">**Step 3:** </span>

<span style="color: rgb(0, 0, 0);">Click **New External Client App**.</span>

##### <span style="color: rgb(0, 0, 0);">**Step 4:** </span>

<span style="color: rgb(0, 0, 0);">Update the basic information as needed, such as the external client app name and your contact email address.</span>

<p class="callout info"><span style="color: rgb(0, 0, 0);">Note: The email address provided must be valid, as **Salesforce** will use it to communicate with your team regarding any updates or issues related to your application usage.</span></p>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/t7RF1MsnBgsJUWO0-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/t7RF1MsnBgsJUWO0-image.png)

##### <span style="color: rgb(0, 0, 0);">**Step 5:** </span>

<span style="color: rgb(0, 0, 0);">Under **API (Enable OAuth Settings)**, click **Enable OAuth**.</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/nPBfthwtSaUVQTVu-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/nPBfthwtSaUVQTVu-image.png)

##### <span style="color: rgb(0, 0, 0);">**Step 6:** </span>

<span style="color: rgb(0, 0, 0);">Under **App Settings**, in the **Callback URL** box, enter the URL below:</span>

```
http://localhost:1717/OauthRedirect
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/I0ff8v0nj2n3BHUb-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/I0ff8v0nj2n3BHUb-image.png)

<p class="callout warning"><span style="color: rgb(0, 0, 0);">If port 1717 is already in use, specify an available port and update your sfdx-project.json file:</span></p>

```
"oauthLocalPort" : "1919"
```

##### <span style="color: rgb(0, 0, 0);">**Step 7:** </span>

<span style="color: rgb(0, 0, 0);">In the **OAuth Scopes** section, select these scopes:</span>

- <span style="color: rgb(0, 0, 0);">**Manage user data via APIs** (api) - Gives you access to user data.</span>
- <span style="color: rgb(0, 0, 0);">**Manage user data via Web browsers** (web) - Allows an external application to authenticate users and access their Salesforce data through a web-based interface. It enables secure, user-consented access to data within a browser, typically used during OAuth flows.</span>
- <span style="color: rgb(0, 0, 0);">**Perform requests at any time (refresh\_token, offline\_access)** - Permits you to get an OAuth access token.</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/R2hCQT5U6OBsYJBv-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/R2hCQT5U6OBsYJBv-image.png)

##### <span style="color: rgb(0, 0, 0);">**Step 8:** </span>

<span style="color: rgb(0, 0, 0);">(Required for JWT) In the **Flow Enablement** section, select **Enable Client Credentials Flow** and **Enable JWT Bearer Flow**.</span>

- <span style="color: rgb(0, 0, 0);">**Enable Client Credentials Flow** - Allows your app to exchange its client credentials for an access token. And be able to access the credential Client ID.</span>
- <span style="color: rgb(0, 0, 0);">**Enable JWT Bearer Flow** - A secure, server-to-server authentication method used to integrate external applications with Salesforce without requiring manual user login.</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/ZGUvtPXQ3OVi3JeW-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/ZGUvtPXQ3OVi3JeW-image.png)

##### <span style="color: rgb(0, 0, 0);">**Step 9:** </span>

<span style="color: rgb(0, 0, 0);">(Required for JWT) Click **Upload Files** and upload your digital certificate file (<span style="color: rgb(224, 62, 45);">**server.crt**</span>).</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/EcdC04TK8734E0rN-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/EcdC04TK8734E0rN-image.png)

##### **Step 10:**

<span style="color: rgb(0, 0, 0);">In **Security** section check the following:</span>

- <span style="color: rgb(0, 0, 0);">Require secret for Web Server Flow</span>
- <span style="color: rgb(0, 0, 0);">Require secret for Refresh Token Flow</span>
- <span style="color: rgb(0, 0, 0);">Enable Refresh Token Rotation</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/ZLhwfSBBy0jmOgSJ-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/ZLhwfSBBy0jmOgSJ-image.png)

##### <span style="color: rgb(0, 0, 0);">**Step 11:** </span>

<span style="color: rgb(0, 0, 0);">Click **Create** and</span><span style="color: rgb(0, 0, 0);"> **Edit** to configure additional settings.</span>

##### <span style="color: rgb(0, 0, 0);">**Step 12:** </span>

<span style="color: rgb(0, 0, 0);">(Required for JWT) Click the **Policies** tab and configure the following:</span>

- <span style="color: rgb(0, 0, 0);">Open **OAuth(Open Authorization) Policies**</span>
- <span style="color: rgb(0, 0, 0);">In the **Plugin Policies** section, set **Permitted Users** to **Admin approved users are pre-authorized**</span>
- <span style="color: rgb(0, 0, 0);">**In OAuth Start URL** use your organization base URL example: (https://fun-dream-996.my.salesforce.com/)</span>
- <span style="color: rgb(0, 0, 0);">Click **OK**</span>
- <span style="color: rgb(0, 0, 0);">In the **App Policies** section, select the profiles and permission sets that are pre-authorized to use this external client app</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/NuHfJZuvWFJeXBqW-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/NuHfJZuvWFJeXBqW-image.png)

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/6YuVtsg7UDV6XJQX-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/6YuVtsg7UDV6XJQX-image.png)

##### <span style="color: rgb(0, 0, 0);">**Step 14:** </span>

<span style="color: rgb(0, 0, 0);">In the **OAuth Flows and External Client App Enhancements** section Enable Client Credentials Flow** and add username**.**</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/k3Ct7Vxw8p1x9H1b-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/k3Ct7Vxw8p1x9H1b-image.png)

##### <span style="color: rgb(0, 0, 0);">**Step 15:** </span>

<span style="color: rgb(0, 0, 0);">In the **App Authorization** section, under **OAuth(Open Authorization) Policies**, click **Expire refresh token after a specific time**.</span>

##### <span style="color: rgb(0, 0, 0);">**Step 16:** </span>

<span style="color: rgb(0, 0, 0);">Configure token expiration settings:</span>

- <span style="color: rgb(0, 0, 0);">**Refresh Token Validity Period**: Enter 365</span>
- <span style="color: rgb(0, 0, 0);">**Refresh Token Validity Unit**: Select **Day(s)**</span>

##### <span style="color: rgb(0, 0, 0);">**Step 17:** </span>

<span style="color: rgb(0, 0, 0);">In the **Session Timeout in Minutes** box, enter **1-5**.</span>

##### <span style="color: rgb(0, 0, 0);">**Step 18:** </span>

<span style="color: rgb(0, 0, 0);">Click **Save**.</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/iAk3VzRM7w1Mmcsz-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/iAk3VzRM7w1Mmcsz-image.png)

<p class="callout success"><span style="color: rgb(0, 0, 0);">Your external client app is now ready to use.</span></p>

---

##### **<span style="color: rgb(53, 152, 219);">How to Find Client ID (Consumer Key)</span>**

- <span style="color: rgb(0, 0, 0);">type **external** in quick find search bar and click **external client app manager**</span>
- <span style="color: rgb(0, 0, 0);">under **External Client App Name** locate the app you created earlier and click it.</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/erObpFuyEh346HHr-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/erObpFuyEh346HHr-image.png)

- <span style="color: rgb(0, 0, 0);">under settings tab click **OAuth Settings** then you can the view your **client key** and **client secret** after the verification process.</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/JBmH0nQqrZgONVQx-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/JBmH0nQqrZgONVQx-image.png)

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/sqLlW3FdmcDRQHYY-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/sqLlW3FdmcDRQHYY-image.png)

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/scaled-1680-/RH4kDudKRkqCoRJQ-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-02/RH4kDudKRkqCoRJQ-image.png)

---

#### <span style="color: rgb(53, 152, 219);">**Converting server.crt to JKS Keystore File**</span>

<span style="color: rgb(0, 0, 0);">To use the certificate with Salesforce, convert it to a Java KeyStore (JKS) format.</span>

##### <span style="color: rgb(0, 0, 0);">**Step 1:**</span>

<p class="callout info"><span style="color: rgb(0, 0, 0);">**Important step to successfully integrate into SIEM** </span></p>

<span style="color: rgb(0, 0, 0);">Clone the server.key file and save it as server.pem.</span>

<span style="color: rgb(0, 0, 0);">In Linux</span>

```
cp server.key server.pem
```

<p class="callout warning"><span style="color: rgb(0, 0, 0);">Note: Step 2 - 4 are **Optional - (use for salesforce to salesforce integration)**</span></p>

##### <span style="color: rgb(0, 0, 0);">**Step 2:**</span>

<span style="color: rgb(0, 0, 0);">Create a PKCS12 keystore file (minimum password length: 6 characters):</span>

```
openssl pkcs12 -export -in server.crt -inkey server.pem -out keystore.p12
```

##### <span style="color: rgb(0, 0, 0);">**Step 3:**</span>

<span style="color: rgb(0, 0, 0);">Convert the PKCS12 file to JKS format:</span>

```
keytool -importkeystore -srckeystore keystore.p12 -srcstoretype pkcs12 -destkeystore servercert.jks -deststoretype JKS
```

<span style="color: rgb(0, 0, 0);">You will be prompted to create a password. Remember this password for future use.</span>

##### <span style="color: rgb(0, 0, 0);">**Step 4:** </span>

<span style="color: rgb(0, 0, 0);">Change the default alias (Salesforce doesn't support alias "1"):</span>

```
keytool -keystore servercert.jks -changealias -alias 1 -destalias <name of certificate>
```

---

##### <span style="color: rgb(53, 152, 219);">**Required fields for JWT Authentication Integration:**</span>

- ##### `JWT Authentication Client Key Path (full file folder path of server.pem)`
    
    
    - <span style="color: rgb(0, 0, 0);">ex: Users/jdoe/JWT/server.pem</span>
- ##### `Username (email address)`
    
    
    - <span style="color: rgb(0, 0, 0);">example format: ADMIN-3dvj@force.com</span>
- ##### `Client ID (Consumer Key)`
    
    
    - example format: 3MVxxxxxtCx.CV6cbh7fSpKs\_5iexxxxxxxxxxxxxxxxxxxxxxxxxxxZKBaepcxlJUhO1
- ##### `Instance URL`  
    
    
    
    - <span style="color: rgb(0, 0, 0);">example format: https://company.my.salesforce.com</span>

##### Provide this required fields to <span style="color: rgb(53, 152, 219);">[**CyTech Support**](mailto:support@cytechint.com).</span>

---

<span style="color: rgb(186, 55, 42);">Reference Link:</span>

<span style="color: rgb(186, 55, 42);"> <span style="color: rgb(53, 152, 219);"> [Create an External Client App in Your Org | Salesforce DX Developer Guide | Salesforce Developers](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_auth_eca.htm)</span></span>

---

<span style="color: rgb(0, 0, 0);">*If you need further assistance, kindly contact our support at <span style="color: rgb(53, 152, 219);">[**support@cytechint.com**](mailto:support@cytechint.com)</span> for prompt assistance and guidance.*</span>