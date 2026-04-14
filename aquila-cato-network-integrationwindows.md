# AQUILA - Cato Network Integration(Windows)

Cato Networks provides a cloud-native SASE (Secure Access Service Edge) platform that converges networking and security into a global cloud service. The platform generates security and connectivity events that can be collected, analyzed, and monitored for network insights and threat detection. This integration enables centralized event collection from Cato Networks using the Cato CLI for visualization and analysis.

#### <span style="color: rgb(53, 152, 219);">**Integration Overview**</span>

This integration supports event collection through:

- Cato CLI (catocli) using the Events Feed API
- Automated event polling via Windows Service (NSSM)
- Python-based event collection script running as a background service

<p class="callout info">Events can be searched, observed, and visualized for security monitoring, connectivity analysis, and network performance tracking.</p>

##### <span style="color: rgb(53, 152, 219);">**Compatibility**</span>

- Supports event collection for Security and Connectivity event types
- Requires Python 3.6 or higher
- Compatible with Windows Server operating systems
- Requires valid Cato Network API token and Account ID
- Requires NSSM (Non-Sucking Service Manager) for Windows Service integration

##### <span style="color: rgb(53, 152, 219);">**Prerequisites**</span>

<p class="callout warning">Before configuring the Cato Network integration, ensure you have:</p>

- Python 3.6 or higher installed
- Python virtual environment configured at `C:\cato\venv`
- Cato CLI (catocli) installed
- Valid Cato Network API token
- Valid Cato Network Account ID
- NSSM (Non-Sucking Service Manager) downloaded

#### <span style="color: rgb(53, 152, 219);">**Python Installation**</span>

##### <span style="color: rgb(53, 152, 219);">**Windows Installation:**</span>

##### **Step 1:** Open your web browser and navigate to the [official Python downloads](https://www.python.org/downloads/ "https://www.python.org/downloads/") page:

##### **Step 2:** Locate the latest stable version of Python 3 (3.12 or newer is recommended) and choose the correct installer for your system type (64-bit or 32-bit).

##### **Step 3:** Click the installer link to download the .exe file.

##### **Step 4:** After downloading, locate the installer file (e.g., python-3.x.x-amd64.exe) and double-click it to start the installation.

##### **Step 5:** On the installer screen, configure the following options:

- Check **Install launcher for all users**
- Check **Add python.exe to PATH** to enable running Python from the Command Prompt

##### **Step 6:** Click **Install Now** to begin the installation process.

##### **Step 7:** Wait for the installation to complete, then click **Close** when the success message appears.

##### **Step 8:** Verify the installation:

- Open the Start menu, type **cmd**, and open Command Prompt
- Type `python --version` and press Enter to confirm the installed Python version

#### <span style="color: rgb(53, 152, 219);">**Creating Python Virtual Environment**</span>

A Python virtual environment isolates project dependencies and prevents conflicts with system-wide Python packages.

##### **Step 1:** Open Command Prompt or PowerShell.

##### **Step 2:** Create and Navigate to your project directory using the mkdir and cd command:

```
mkdir "C:\cato"
cd C:\cato
```

##### **Step 3:** Create a virtual environment:

```
python -m venv venv
```

##### **Step 4:** Activate the virtual environment:

- **Command Prompt:**```
    venv\Scripts\activate.bat
    ```

- **PowerShell:**```
    .\venv\Scripts\activate.ps1
    ```

##### **Step 5:** Once activated, the virtual environment name (e.g., (venv)) will appear in your terminal prompt.

##### **Step 6:** To deactivate the virtual environment, type:

```
deactivate
```

#### <span style="color: rgb(53, 152, 219);">**Cato CLI Installation**</span>

##### **Step 1:** Ensure your virtual environment is activated.

##### **Step 2:** Install the Cato CLI using pip:

```
pip3 install catocli
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/T8innKXGPj0E8aBZ-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/T8innKXGPj0E8aBZ-image.png)

##### **Step 3:** Verify the installation by running:

```
catocli --version
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/dbZLovD5Ic5UsJPu-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/dbZLovD5Ic5UsJPu-image.png)

#### <span style="color: rgb(53, 152, 219);">**Configuring Cato CLI for Local System Execution**</span>

To enable the Cato CLI to run as a Windows Service under the Local System account, modify the profile manager configuration.

##### **Step 1:** Navigate to the Cato CLI profile manager file:

```
C:\cato\venv\Lib\site-packages\catocli\Utils\profile_manager.py
```

##### **Step 2:** Open the file in a text editor (e.g., Notepad or Visual Studio Code).

##### **Step 3:** Locate the `__init__` constructor in the file:

```python
def __init__(self):
    self.cato_dir = Path.home() / '.cato'
    self.credentials_file = self.cato_dir / 'credentials'
    self.config_file = self.cato_dir / 'config'
    self.default_endpoint = "https://api.catonetworks.com/api/v1/graphql2"
```

##### **Step 4:** Replace `Path.home()` with `Path("C:/cato")`:

```python
def __init__(self):
    self.cato_dir = Path("C:/cato") / '.cato'
    self.credentials_file = self.cato_dir / 'credentials'
    self.config_file = self.cato_dir / 'config'
    self.default_endpoint = "https://api.catonetworks.com/api/v1/graphql2"
```

<p class="callout success">This change ensures the Cato CLI uses a fixed directory path instead of the user's home directory<span class="token token">,</span> which <span class="token token">is</span> essential <span class="token token">for</span> running <span class="token token">as</span> a system service<span class="token token">.</span></p>

##### <span class="token token">**Step 5:** Save and close the file.</span>

#### <span style="color: rgb(53, 152, 219);">**Validating Cato Network API Token and Account ID**</span>

##### **Step 1:** Configure the Cato CLI with your API token and Account ID:

```
catocli configure set --cato-token "your-api-token" --account-id "12345"
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/anuhQHhpEoQO1jgY-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/anuhQHhpEoQO1jgY-image.png)

<p class="callout info">Replace `"your-api-token"` with your actual Cato Network API token and `"12345"` with your Account ID.</p>

#### **<span class="token token" style="color: rgb(53, 152, 219);">Downloading and Installing NSSM</span>**

<p class="callout info"><span style="color: rgb(0, 0, 0);"><span class="token token">NSSM <span class="token token">(</span>Non<span class="token token">-</span>Sucking Service Manager<span class="token token">)</span> <span class="token token">is</span> a service helper tool that allows you to run <span class="token token">any</span> application <span class="token token">as</span> a Windows Service<span class="token token">.</span></span></span></p>

##### <span style="color: rgb(0, 0, 0);"><span class="token token"><span class="token token">**Step 1:** Download [NSSM](https://nssm.cc/download "https://nssm.cc/download") from the official website:</span></span></span>

##### <span style="color: rgb(0, 0, 0);"><span class="token token"><span class="token token">**Step 2:** Extract the downloaded ZIP file.</span></span></span>

##### <span style="color: rgb(0, 0, 0);"><span class="token token"><span class="token token">**Step 3:** Copy the appropriate `nssm.exe` file (32-bit or 64-bit based on your system) to the Cato integration directory:</span></span></span>

```
C:\cato\nssm.exe
```

#### <span style="color: rgb(53, 152, 219);">**<span class="token token"><span class="token token">Creating the Python Event Collection Script</span></span>**</span>

##### <span style="color: rgb(0, 0, 0);"><span class="token token"><span class="token token">**Step 1:** Create a Python script file at:</span></span></span>

```
C:\cato\my_script.py
```

##### **Step 2:** Paste this configuration script to my\_script.py that you created earlier:

```python
import os
import subprocess
import time
import gzip
import shutil
import logging
import sys
import msvcrt
from logging.handlers import RotatingFileHandler

# ================= CONFIG =================
BASE_DIR = r"C:\cato"

MARKER_FILE = os.path.join(BASE_DIR, "events-marker.txt")
LOCK_FILE = os.path.join(BASE_DIR, "cato-eventsfeed.lock")
LOG_FILE = os.path.join(BASE_DIR, "events.log")
WATCHDOG_FILE = os.path.join(BASE_DIR, "cato-eventsfeed.watchdog")

CATOCLI = r"C:\cato\venv\Scripts\catocli.exe"

INTERVAL = 600          # 10 minutes
MAX_RETRIES = 3
RETRY_DELAY = 10        # seconds

os.makedirs(BASE_DIR, exist_ok=True)

# ================= CUSTOM HANDLER =================
class CompressedRotatingFileHandler(RotatingFileHandler):
    def doRollover(self):
        """
        Override rotation to compress old logs
        """
        if self.stream:
            self.stream.close()
            self.stream = None

        # Rotate existing files
        for i in range(self.backupCount - 1, 0, -1):
            sfn = f"{self.baseFilename}.{i}.gz"
            dfn = f"{self.baseFilename}.{i+1}.gz"
            if os.path.exists(sfn):
                if i + 1 > self.backupCount:
                    os.remove(sfn)
                else:
                    os.rename(sfn, dfn)

        # Compress current log → .1.gz
        if os.path.exists(self.baseFilename):
            with open(self.baseFilename, 'rb') as f_in:
                with gzip.open(f"{self.baseFilename}.1.gz", 'wb') as f_out:
                    shutil.copyfileobj(f_in, f_out)

            os.remove(self.baseFilename)

        # Reopen fresh log file
        self.stream = self._open()

# ================= LOGGING =================
LOG_MAX_SIZE = 1000 * 1024 * 1024   # 1 GB
LOG_BACKUP_COUNT = 5             # keep 5 rotated logs

log_handler = CompressedRotatingFileHandler(
    LOG_FILE,
    maxBytes=LOG_MAX_SIZE,
    backupCount=LOG_BACKUP_COUNT
)

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(message)s",
    handlers=[
        log_handler
    ]
)

# ================= LOCK =================
def acquire_lock():
    lock_file = open(LOCK_FILE, 'w')
    try:
        msvcrt.locking(lock_file.fileno(), msvcrt.LK_NBLCK, 1)
        return lock_file
    except OSError:
        logging.error("Another instance is already running. Exiting.")
        sys.exit(1)

# ================= CORE =================
def run_cato():
    cmd = [
        CATOCLI,
        "query", "eventsFeed",
        "--run",
        "--print-events",
        f"--marker-file={MARKER_FILE}",
        "--runtime-limit=10",
        "--event-types=Security,Connectivity"
    ]

    for attempt in range(1, MAX_RETRIES + 1):
        try:
            logging.info(f"Running Cato CLI (Attempt {attempt})")

            result = subprocess.run(
                cmd,
                capture_output=True,
                text=True
            )

            if result.stdout:
                logging.info(result.stdout.strip())

            if result.stderr:
                logging.error(result.stderr.strip())

            if result.returncode == 0:
                logging.info("Cato CLI succeeded")
                return True
            else:
                logging.error(f"Failed with code {result.returncode}")

        except Exception as e:
            logging.exception(f"Exception: {e}")

        time.sleep(RETRY_DELAY)

    logging.error("Max retries reached")
    return False

# ================= WATCHDOG =================
def update_watchdog():
    try:
        with open(WATCHDOG_FILE, "w") as f:
            f.write(str(int(time.time())))
    except Exception as e:
        logging.exception(f"Watchdog update failed: {e}")

# ================= MAIN LOOP =================
def main():
    lock = acquire_lock()

    try:
        while True:
            start = time.time()

            try:
                success = run_cato()
                if success:
                    update_watchdog()
            except Exception as e:
                logging.exception(f"Unexpected crash in main loop: {e}")

            elapsed = time.time() - start
            sleep_time = max(0, INTERVAL - elapsed)

#            logging.info(f"Sleeping {sleep_time} seconds")
            time.sleep(sleep_time)

    finally:
        lock.close()

if __name__ == "__main__":
    main()

```

##### **Step <span class="token token">3:</span>** Save the script <span class="token token">file</span><span class="token token">.</span>

#### <span style="color: rgb(53, 152, 219);">**<span class="token token">Installing the Cato Events Feed as a Windows Service</span>**</span>

<p class="callout info"><span class="token token">Use NSSM to install the Python script as a Windows Service that runs automatically in the background.</span></p>

##### <span class="token token">**Step 1:** Open PowerShell as Administrator and </span><span class="token token">Navigate to the Cato directory:</span>

```
cd C:\cato
```

##### **Step 2<span class="token token">:</span>** Run the NSSM installation command<span class="token token">:</span>

```
C:\cato\nssm.exe install CatoEventsFeed
```

<p class="callout info"><span class="token token">This opens the NSSM service installer GUI.</span></p>

##### **Step 1: Configuring the Service - Application Tab:**

##### **Path:** C:\\cato\\venv\\Scripts\\python.exe  
**Startup directory:** C:\\cato  
**Arguments:** C:\\cato\\my\_script.py

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/lmjFeXyxwLMPJ7Ce-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/lmjFeXyxwLMPJ7Ce-image.png)

##### **Step 2: Configuring the Service - Details Tab:**  
**Display name:** CatoEventsFeed  
**Description:** The platform generates security and connectivity events that can be collected, analyzed, and monitored for network insights and threat detection. This service enables centralized event collection from Cato Networks using the Cato CLI for visualization and analysis.

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/RykIOTi9dKSTbr1G-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/RykIOTi9dKSTbr1G-image.png)

##### **Step 4: Configuring the Service - Log on tab**  
select Local System account

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/Eh2Mzf7VDp8mQIfw-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/Eh2Mzf7VDp8mQIfw-image.png)

<p class="callout info">This allows the service to run <span class="token token">with</span> system<span class="token token">-</span>level privileges without requiring a specific user login<span class="token token">.</span></p>

##### **Step 5: Configuring the Service - Exit action tab**  
**delay restart if application runs for less than:** 5000

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/28zTZgYlIpLbeBec-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/28zTZgYlIpLbeBec-image.png)

<p class="callout info">This setting ensures the service waits <span class="token token">5</span> seconds before attempting a restart <span class="token token">if</span> the application crashes immediately after starting<span class="token token">.</span></p>

##### **<span class="token token">Step 6: Click <span style="color: rgb(45, 194, 107);">Install service</span> to complete the installation.</span>**

#### <span style="color: rgb(53, 152, 219);">**<span class="token token">Starting the Cato Events Feed Service</span>**</span>

##### **<span class="token token">Step 1: </span>**<span class="token token">Open PowerShell as Administrator and n</span><span class="token token">avigate to the Cato directory:</span>

```
cd C:\cato
```

##### **Step 2<span class="token token">:</span>** Start the service using NSSM<span class="token token">:</span>

```
.\nssm.exe start CatoEventsFeed
```

##### **Step 3<span class="token token">: </span>**Verify the service <span class="token token">is</span> running<span class="token token">:</span>

```
sc.exe query CatoEventsFeed
```

<p class="callout success">The output should show "<span style="color: rgb(0, 0, 0);">STATE<span class="token token">:</span> RUNNING</span>"<span class="token token">.</span></p>

#### <span style="color: rgb(53, 152, 219);">**<span class="token token">Managing the Cato Events Feed Service</span>**</span>

<span style="color: rgb(53, 152, 219);">**<span class="token token">To start the service:</span>**</span>

```
C:\cato\nssm.exe start CatoEventsFeed
```

<span style="color: rgb(53, 152, 219);">**<span class="token token">To restart the service:</span>**</span>

```
C:\cato\nssm.exe restart CatoEventsFeed
```

<span style="color: rgb(53, 152, 219);">**To check service status<span class="token token">:</span>**</span>

```
C:\cato\nssm.exe status CatoEventsFeed
```

#### <span style="color: rgb(53, 152, 219);">**Event Collection Settings**</span>

The integration collects the following event types from Cato Networks:

- <span style="color: rgb(53, 152, 219);">**Security Events**</span>: Threat detection, malware blocks, IPS alerts, and security policy violations
- <span style="color: rgb(53, 152, 219);">**Connectivity Events**:</span> Site connectivity changes, tunnel status, WAN link failures, and network performance issues

#### <span style="color: rgb(53, 152, 219);">**Configuration Parameters:**</span>

- <span style="color: rgb(53, 152, 219);">**Marker File**:</span> `C:\cato\events-marker.txt` - Tracks the last processed event to prevent duplicates
- <span style="color: rgb(53, 152, 219);">**Runtime Limit**:</span> 10 minutes per execution cycle
- <span style="color: rgb(53, 152, 219);">**Event Types**:</span> Security and Connectivity events
- <span style="color: rgb(53, 152, 219);">**Collection Interval**: <span style="color: rgb(0, 0, 0);">10</span></span> minutes (configurable in my\_script.py)

#### <span style="color: rgb(53, 152, 219);">**Log Events**</span>

Enable this option to collect Cato Network log events across all configured event types from your Cato SASE platform.

#### <span style="color: rgb(53, 152, 219);">**Logs Dataset**</span>

The `cato.events` dataset contains events collected from the Cato Networks Events Feed API. All Cato-specific event fields are available in the configured log files for detailed analysis, including:

- Event timestamps and identifiers
- Source and destination information
- Security threat classifications
- Network connectivity status
- Policy enforcement actions
- Geographic and site information