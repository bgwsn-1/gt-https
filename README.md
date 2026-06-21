# Growtopia HTTPS Redirect & Cache Server

A lightweight C++ HTTP/HTTPS redirect and caching server designed for Growtopia Private Servers (GTPS). It utilizes [httplib.h](./httplib.h) with OpenSSL support and `libcurl` to dynamically fetch assets from the official Growtopia CDN.

---

## 🌟 Key Features

1. **Dual Port Listener:** Listens on port `80` (HTTP) and `443` (HTTPS) simultaneously.
2. **Auto CDN Cache:** Intercepts client asset requests (e.g., `/cache/...`) and dynamically downloads missing assets from the official Growtopia CDN, saving them in the `cache/` directory.
3. **Asset Override:** Allows you to serve custom game assets (e.g., modified `.rttex` textures or audio) by placing them in the `override/` directory.
4. **Server Data Router:** Securely serves the [server_data.php](./assets/server_data.php) configuration file to the client.

---

## 🔑 How to Install the SSL Root Certificate

Since the Growtopia client uses the HTTPS protocol, you **must** install the root certificate to make your system trust the local HTTPS server. This prevents SSL handshake errors (Security/Handshake Error).

A `cert.crt` file is already provided for direct installation. Choose one of the methods below:

### Method A: Via Windows Explorer (GUI)
1. Double-click the `cert.crt` file.
2. In the Certificate Import Wizard window:
   * Click **Install Certificate...**
   * Select **Local Machine** as the store location, then click **Next** (requires Administrator privileges).
3. On the Certificate Store page:
   * Select **Place all certificates in the following store**.
   * Click **Browse...**, select **Trusted Root Certification Authorities**, and click **OK**.
4. Click **Next**, then **Finish**.
5. If a security warning dialog appears, click **Yes**.

### Method B: Via PowerShell (Command Line)
1. Open **PowerShell** as Administrator (Right-click the Start button -> select *Terminal (Admin)* or *PowerShell (Admin)*).
2. Run the following command to import the certificate directly:
   ```powershell
   Import-Certificate -FilePath ".\cert.crt" -CertStoreLocation Cert:\LocalMachine\Root
   ```

---

## 🚀 How to Run the Server

1. **Prepare the Certificates:**
   Ensure `cert.pem` and `key.pem` are placed in the [assets/](./assets) folder.
2. **Configure the Hosts File:**
   Redirect the official Growtopia domains to your local IP (`127.0.0.1`). Add the following lines to your Windows hosts file (`C:\Windows\System32\drivers\etc\hosts`):
   ```text
   127.0.0.1 www.growtopia1.com
   127.0.0.1 www.growtopia2.com
   ```
3. **Run the Application:**
   Run `GrowtopiaHTTPS.exe` as Administrator (this is required to bind to the privileged ports `80` and `443`).

---

## 📂 Directory Structure

* 📂 **[assets/](./assets)**: Contains SSL certificates (`cert.pem`, `key.pem`) and the [server_data.php](./assets/server_data.php) configuration file.
* 📂 **[cache/](./cache)**: Stores cached game assets downloaded from the CDN.
* 📂 **override/**: Put your custom modified assets here to override CDN files.