# AQUILA - Cato Network Integration(Linux)

Cato Networks provides a cloud-native SASE (Secure Access Service Edge) platform that converges networking and security into a global cloud service. The platform generates security and connectivity events that can be collected, analyzed, and monitored for network insights and threat detection. This integration enables centralized event collection from Cato Networks using the Cato CLI for visualization and analysis.

#### <span style="color: rgb(53, 152, 219);">**Integration Overview**</span>

This integration supports event collection through:

- Cato CLI (catocli) using the Events Feed API
- Automated event polling via systemd service
- File-based log collection with structured JSON output

<p class="callout info">Events can be searched, observed, and visualized for security monitoring, connectivity analysis, and network performance tracking.</p>

##### <span style="color: rgb(53, 152, 219);">**Compatibility**</span>

- Supports event collection for Security and Connectivity event types
- Requires Python 3.6 or higher
- Compatible with Linux, macOS, and Windows operating systems
- Requires valid Cato Network API token and Account ID
- Systemd service integration for Linux-based systems

##### <span style="color: rgb(53, 152, 219);">**Prerequisites**</span>

<p class="callout warning">Before configuring the Cato Network integration, ensure you have:</p>

- Python 3.6 or higher installed
- Python virtual environment configured
- Cato CLI (catocli) installed
- Valid Cato Network API token
- Valid Cato Network Account ID

#### <span style="color: rgb(53, 152, 219);">**Python Installation**</span>

##### <span style="color: rgb(53, 152, 219);">**Linux Installation:**</span>

##### **Step 1:** Open the Terminal and refresh package lists:

```
sudo apt update
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/KF6h5bY5QGCPa5Hs-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/KF6h5bY5QGCPa5Hs-image.png)

##### **Step 2:** Update installed packages:

```
sudo apt upgrade -y
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/dJLuZQGlfYpl9Yup-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/dJLuZQGlfYpl9Yup-image.png)

##### **Step 3:** Install Python (replace \[version number\] with your desired version):

```
sudo apt install python[version number]
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/eEB6el60eELJcHL5-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/eEB6el60eELJcHL5-image.png)

#### <span style="color: rgb(53, 152, 219);">**Creating Python Virtual Environment**</span>

A Python virtual environment isolates project dependencies and prevents conflicts with system-wide Python packages.

#### <span style="color: rgb(53, 152, 219);">**Linux/macOS:**</span>

##### **Step 1:** Open a Terminal.

##### **Step 2:** Navigate to your project directory using the cd command:

```
cd /path/to/your/project
```

##### **Step 3:** Create a virtual environment:

```
python3 -m venv venv
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/6m9xJWxlJoM4JZgX-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/6m9xJWxlJoM4JZgX-image.png)

##### **Step 4:** Activate the virtual environment:

```
source venv/bin/activate
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/8JwkcTJNarZXDjV8-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/8JwkcTJNarZXDjV8-image.png)

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

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/TDTJvkiIMQeZjJSX-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/TDTJvkiIMQeZjJSX-image.png)

##### **Step 3:** Verify the installation by running:

```
catocli --version
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/I7ClZSlQtUACeBRR-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/I7ClZSlQtUACeBRR-image.png)

#### <span style="color: rgb(53, 152, 219);">**Validating Cato Network API Token and Account ID**</span>

##### **Step 1:** Configure the Cato CLI with your API token and Account ID:

```
catocli configure set --cato-token "your-api-token" --account-id "12345"
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/4LRoTcRf5CQwm73z-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/4LRoTcRf5CQwm73z-image.png)

<p class="callout info">Replace `"your-api-token"` with your actual Cato Network API token and `"12345"` with your Account ID.</p>

#### <span style="color: rgb(53, 152, 219);">**Configuring the Cato Network Integration (Linux)**</span>

##### <span style="color: rgb(53, 152, 219);">**Creating a Dedicated User (Optional but Recommended):**</span>

<p class="callout warning">For security and isolation, create a dedicated system user to run the Cato event collection service.</p>

##### **Step 1:** <span style="color: rgb(22, 145, 121);">Optional</span> - Create a system user named (for this example) `testing-cato`:

```
sudo useradd -r -s /bin/false testing-cato
```

##### **Step 2:** Create required directories for data, logs:

```
sudo mkdir -p /var/lib/cato /var/log/cato
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/ZRqQkYqmlKfsJL4j-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/ZRqQkYqmlKfsJL4j-image.png)

##### **Step 3:** Set ownership of the directories to the dedicated user:

```
sudo chown -R testing-cato:testing-cato /var/lib/cato /var/log/cato
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/1afigOby8PUfoHUA-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/1afigOby8PUfoHUA-image.png)

#### <span style="color: rgb(53, 152, 219);">**Creating the Bash Wrapper Script:**</span>

<p class="callout info">The wrapper script manages event collection, logging, and ensures only one instance runs at a time.</p>

##### **Step 1:** Create the wrapper script file:

```
sudo nano /usr/local/bin/cato-eventsfeed.sh
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/b2P0ZvFyPtoVS4sx-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/b2P0ZvFyPtoVS4sx-image.png)

##### **Step 2:** Add the following content to the file:

<p class="callout danger">Important: Replace "testing-cato" in CATOCLI to the actual user</p>

```bash
#!/bin/bash
# Cato Networks Event Feed Wrapper Script
# Supports venv, logging, single instance, watchdog

MARKER_FILE="/var/lib/cato/events-marker.txt"
LOCK_FILE="/var/lock/cato-eventsfeed.lock"
LOG_FILE="/var/log/cato/events.log"
WATCHDOG_FILE="/var/run/cato-eventsfeed.watchdog"

CATOCLI="/home/testing-cato/venv/bin/catocli"

# Ensure only one instance runs at a time
exec 200>"$LOCK_FILE"
flock -n 200 || exit 1

# Run the feed once
$CATOCLI query eventsFeed \
    --run \
    --print-events \
    --marker-file="$MARKER_FILE" \
    --runtime-limit=10 \
    --event-types="Security,Connectivity" >> "$LOG_FILE" 2>&1

# Update watchdog file for systemd
date +%s > "$WATCHDOG_FILE"

# Notify systemd (if using sd_notify)
if [ -n "$NOTIFY_SOCKET" ]; then
    /bin/echo "WATCHDOG=1" | systemd-cat --identifier=cato-eventsfeed
fi
```

##### **<span class="token token">Step </span><span class="token token">3</span>**<span class="token token">**:** Save and </span><span class="token token">exit</span><span class="token token"> the </span><span class="token token">file</span> <span class="token token">(</span><span class="token token">Ctrl+X, </span><span class="token token">then</span><span class="token token"> Y, </span><span class="token token">then</span><span class="token token"> Enter</span><span class="token token">)</span><span class="token token">. </span>

##### **<span class="token token">Step </span><span class="token token">4</span>**<span class="token token">**:** Make the script executable:</span>

```
sudo chmod +x /usr/local/bin/cato-eventsfeed.sh
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/ZpbIlyeQDiAgLnwn-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/ZpbIlyeQDiAgLnwn-image.png)

##### <span class="token token">**Step 5:** Set ownership to the dedicated user:</span>

```
sudo chown testing-cato:testing-cato /usr/local/bin/cato-eventsfeed.sh
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/MZYxUVp3xeoPojV4-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/MZYxUVp3xeoPojV4-image.png)

#### <span style="color: rgb(53, 152, 219);">**Creating the Systemd Service File:**</span>

<p class="callout info"><span class="token token">The systemd </span><span class="token token">service</span><span class="token token"> ensures the event collection runs continuously and restarts automatically on failure.</span></p>

##### <span class="token token">**Step 1:** Create the service file:</span>

```
sudo nano /etc/systemd/system/cato-eventsfeed.service
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/0rREOoDP43ogMFqm-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/0rREOoDP43ogMFqm-image.png)

##### <span class="token token">**Step 2:** Add the following content to the file:</span>

```ini
[Unit]
Description=Cato Networks Event Feed Collector (production)
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
ExecStart=/usr/local/bin/cato-eventsfeed.sh
Restart=always
RestartSec=5
StartLimitBurst=10
StartLimitIntervalSec=60

# Watchdog: restart if hung
WatchdogSec=30s
NotifyAccess=all

# Use Python venv in PATH
Environment="PATH=/home/testing-cato/venv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin"

# Run as dedicated non-root user
User=testing-cato
Group=testing-cato
WorkingDirectory=/var/lib/cato

# Optional: memory and CPU limits
# MemoryMax=200M
# CPUQuota=50%

[Install]
WantedBy=multi-user.target
```

<p class="callout warning">Note: In Environment path, replace the path of the actual path of your python virtual path "PATH=/home/**<span style="color: rgb(53, 152, 219);">testing-cato/venv</span>/**bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin". Change User and Group for non-root user as well.</p>

##### <span class="token token">**Step 3:** Save and exit the file (Ctrl+X, then Y, then Enter).</span>

#### <span style="color: rgb(53, 152, 219);">**<span class="token token">Configuring Log Rotation</span>**</span>

<span class="token token">To manage log file sizes and prevent disk space issues, configure log rotation for Cato event logs.</span>

##### <span class="token token">**Step 1:** Create a logrotate configuration file:</span>

```
sudo nano /etc/logrotate.d/cato-events
```

##### <span class="token token">**Step 2:** Add the following configuration to the file:</span>

```
/var/log/cato/events.log {
    hourly
    rotate 24
    missingok
    notifempty

    copytruncate

    compress
    compressoptions -1
    delaycompress

    dateext
    dateformat -%Y%m%d-%H%M%S

    create 0640 testing-cato testing-cato
}
```

<p class="callout info">This configuration:  
- Rotates logs hourly  
- Keeps the last 24 rotated log files  
- Compresses old log files to save disk space  
- Creates new log files with appropriate permissions</p>

##### **Step 3:** Save and exit the file (Ctrl+X, then Y, then Enter).

#### **<span style="color: rgb(53, 152, 219);">Configuring Hourly Log Rotation</span>**

By default, logrotate runs daily. To ensure more frequent log rotation for high-volume Cato event logs, configure the logrotate timer to run hourly.

##### <span style="color: rgb(53, 152, 219);">**Creating the Timer Override File:**</span>

##### **Step 1:** Create the systemd override directory:

```
sudo mkdir -p /etc/systemd/system/logrotate.timer.d
```

##### **Step 2:** Create the override configuration file:

```bash
sudo nano /etc/systemd/system/logrotate.timer.d/override.conf
```

##### **Step 3:** Add the following configuration to the file:

```ini
[Timer]
OnCalendar=
OnCalendar=hourly
AccuracySec=1m
Persistent=true
```

<p class="callout info">This configuration:  
- Clears the default daily schedule with the empty 'OnCalendar'=  
- Sets the timer to run hourly  
- Ensures the timer runs within 1 minute of the scheduled time  
- Catches up on missed runs if the system was offline</p>

##### **Step 4:** Save and exit the file (Ctrl+X, then Y, then Enter).

#### <span style="color: rgb(53, 152, 219);">**Reloading and Restarting the Timer**</span>

##### **Step 1:** Reload the systemd daemon to recognize configuration changes:

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
```

##### **Step 2:** Restart the logrotate timer to apply the new schedule:

```
sudo systemctl restart logrotate.timer
```

##### **Step 3:** Verify the timer is active and scheduled correctly:

```
sudo systemctl status logrotate.timer
```

##### **Step 4:** Check the next scheduled run time:

```
systemctl list-timers logrotate.timer
```

The output should show the timer scheduled to run hourly.

#### <span style="color: rgb(53, 152, 219);">**Enabling and Starting the Service**</span>

##### **Step 1:** Reload systemd to recognize the new service file:

```
sudo systemctl daemon-reload
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/Tu7V23sh2jsdjsNA-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/Tu7V23sh2jsdjsNA-image.png)

##### **Step 2:** Enable the service to start automatically on boot:

```
sudo systemctl enable cato-eventsfeed
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/79JRLWTK8bNIR1yJ-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/79JRLWTK8bNIR1yJ-image.png)

##### **Step 3:** Start the service:

```
sudo systemctl start cato-eventsfeed
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/Dudlo9JFOl1nIPH7-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/Dudlo9JFOl1nIPH7-image.png)

##### **Step 4:** Verify the service is running:

```
sudo systemctl status cato-eventsfeed
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/scaled-1680-/zeBYSJRErF63eghq-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-03/zeBYSJRErF63eghq-image.png)

<p class="callout success">The status should show *active (running)* in green text.</p>

##### <span style="color: rgb(53, 152, 219);">**Monitoring Live Logs:**</span>

To view real-time logs from the Cato event feed service:

```
journalctl -u cato-eventsfeed -f
```

Press **Ctrl+C** to stop viewing the live logs.

##### <span style="color: rgb(53, 152, 219);">**Event Collection** **Settings**</span>

The integration collects the following event types from Cato Networks:

- **Security Events**: Threat detection, malware blocks, IPS alerts, and security policy violations
- **Connectivity Events**: Site connectivity changes, tunnel status, WAN link failures, and network performance issues

##### <span style="color: rgb(53, 152, 219);">**Configuration Parameters:**</span>

- **Marker File**: `/var/lib/cato/events-marker.txt` - Tracks the last processed event to prevent duplicates
- **Lock File**: `/var/lock/cato-eventsfeed.lock` - Ensures only one instance of the script runs at a time
- **Log File**: `/var/log/cato/events.log` - Stores collected events in JSON format
- **Runtime Limit**: 10 minutes per execution cycle
- **Event Types**: Security and Connectivity events

##### <span style="color: rgb(53, 152, 219);">**Log Events**</span>

Enable this option to collect Cato Network log events across all configured event types from your Cato SASE platform.

##### <span style="color: rgb(53, 152, 219);">**Logs Dataset**</span>

The `cato.events` dataset contains events collected from the Cato Networks Events Feed API. All Cato-specific event fields are available in the `/var/log/cato/events.log` file for detailed analysis, including:

- Event timestamps and identifiers
- Source and destination information
- Security threat classifications
- Network connectivity status
- Policy enforcement actions
- Geographic and site information