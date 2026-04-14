# AQUILA - Cisco Meraki Integration

<span style="color: rgb(0, 0, 0);">Cisco Meraki provides a centralized cloud management platform for devices like MX Security Appliances, MR Access Points, and more. Its cloud-based architecture enables secure, scalable networks manageable from anywhere via the Meraki Dashboard or Mobile App. Each Meraki network generates events that can be collected and analyzed.</span>

---

### <span style="color: rgb(53, 152, 219);">**Integration Overview**</span>

<span style="color: rgb(0, 0, 0);">This integration supports event collection through:</span>

- <span style="color: rgb(0, 0, 0);">**Syslog** messages from Meraki devices</span>

<span style="color: rgb(0, 0, 0);">Events can be searched, observed, and visualized.</span>

---

### <span style="color: rgb(53, 152, 219);">**Compatibility**</span>

- <span style="color: rgb(0, 0, 0);">Supports event collection from **MX Security Appliances** and **MR Access Points** via syslog.</span>
- <span style="color: rgb(0, 0, 0);">**MS Switch** events are **not supported** and will not be recognized.</span>

---

#### <span style="color: rgb(53, 152, 219);">**Cisco Meraki Dashboard Configuration**</span>

##### <span style="color: rgb(53, 152, 219);">Syslog Setup:</span>  


<span style="color: rgb(0, 0, 0);">1. I**dentify Syslog-ng IP Address**</span>

<span style="color: rgb(0, 0, 0);">Access the log collector virtual machine and open a terminal. Run the following command to determine the IP address of the syslog-ng server:</span>

```
ifconfig -a
```

<p class="callout info"><span style="color: rgb(0, 0, 0);">Please take note of the IP address, as this will be referenced during the configuration.</span></p>

<span style="color: rgb(0, 0, 0);">2. **Install Syslog-ng**</span>

<span style="color: rgb(0, 0, 0);">Install syslog-ng along with its required dependencies using the following command:</span>

```
sudo apt-get install syslog-ng
```

<span style="color: rgb(0, 0, 0);">3. **Configure Syslog-ng**</span>

<span style="color: rgb(0, 0, 0);">Edit the syslog-ng configuration file:</span>

```
sudo nano /etc/syslog-ng/syslog-ng.conf
```

<p class="callout info"><span style="color: rgb(0, 0, 0);">Locate the following line:</span></p>

```
log { source(s_src); filter(f_crit); destination(d_console); };
```

<p class="callout warning"><span style="color: rgb(0, 0, 0);">Add the configuration below it, ensuring that Server\_IP\_Address and &lt;MERAKI\_IP\_ADDRESS&gt; are replaced with the appropriate values:</span></p>

```
# Define syslog source
source s_net { udp(ip(Server_IP_Address) port(5140)); };

# Create filter to match traffic (this filter will catch all syslog messages from the MX)
filter f_meraki { host("<MERAKI_IP_ADDRESS>"); };

# Define a destination for syslog messages
destination df_meraki { file("/var/log/cisco_meraki.log"); };

# Bundle the source, filter, and destination rules together
log { source(s_net); filter(f_meraki); destination(df_meraki); };
```

<span style="color: rgb(0, 0, 0);">4. **Restart Syslog-ng**</span>  
<span style="color: rgb(0, 0, 0);">After saving the configuration, restart the syslog-ng service to apply the changes:</span>

```
sudo /etc/init.d/syslog-ng restart
```

---

#### <span style="color: rgb(53, 152, 219);">**Configuring the Cisco Meraki Integration**</span>

<span style="color: rgb(0, 0, 0);">Once the syslog-ng server is configured, please proceed with the following steps in the Cisco Meraki dashboard:</span>

<span style="color: rgb(0, 0, 0);">1. **Log in to the Cisco Meraki dashboard.**</span>

<span style="color: rgb(0, 0, 0);">2. **Navigate to Network-wide &gt; Configure &gt; General.**</span>

<span style="color: rgb(0, 0, 0);">3. **Click Add a syslog server.**</span>

<span style="color: rgb(0, 0, 0);">4. **Populate the required fields as follows:**</span>

- <span style="color: rgb(0, 0, 0);">Server Address: Syslog server IP address</span>
- <span style="color: rgb(0, 0, 0);">Port: 5140</span>
- <span style="color: rgb(0, 0, 0);">Protocol: UDP</span>

<span style="color: rgb(0, 0, 0);">5. Under Roles, enable**:**</span>

- <span style="color: rgb(0, 0, 0);">Switch Event Log</span>
- <span style="color: rgb(0, 0, 0);">Wireless Air Marshal Events</span>
- <span style="color: rgb(0, 0, 0);">Wireless Flow</span>

<p class="callout info"><span style="color: rgb(0, 0, 0);">Optional: Configuration Verification</span></p>

<span style="color: rgb(0, 0, 0);">To verify successful log ingestion, access the syslog server and run:</span>

```
cd /var/log/
ls
```

<p class="callout success"><span style="color: rgb(0, 0, 0);">If the file **cisco\_meraki.log** is present, the configuration has been successfully applied and logs are being received.</span></p>

---

### **<span style="background-color: rgb(255, 255, 255); color: rgb(53, 152, 219);">Log Rotation Configuration</span>**

<span style="color: rgb(0, 0, 0);"><span style="background-color: rgb(255, 255, 255);">To manage log growth and prevent disk space issues, please configure log rotation as follows:  
</span><span style="background-color: rgb(255, 255, 255);">Create a logrotate configuration file:</span>**<span style="background-color: rgb(255, 255, 255);">  
</span>**</span>

```
sudo nano /etc/logrotate.d/meraki
```

**<span style="color: rgb(0, 0, 0);">Add the following content:</span>**

```
/var/log/cisco_meraki.log {
    daily
    missingok
    rotate 1
    compress
    delaycompress
    notifempty
    create 0640 root root
    postrotate
        # Optional commands, such as reloading syslog services
        # /etc/init.d/syslog-ng reload
    endscript
}
```

---

### <span style="color: rgb(53, 152, 219);">**Log Events**</span>

<span style="color: rgb(0, 0, 0);">Enable this option to collect Cisco Meraki log events across all applications configured for the selected log stream.</span>

---

### <span style="color: rgb(53, 152, 219);">**Logs Dataset**</span>

- <span style="color: rgb(0, 0, 0);">The `cisco_meraki.log` dataset contains events collected from the configured syslog server.</span>
- <span style="color: rgb(0, 0, 0);">All Cisco Meraki specific syslog fields are available under the `cisco_meraki.log` field group for detailed analysis.</span>

<span style="color: rgb(0, 0, 0);">*<span class="TextRun SCXW71272603 BCX0" data-contrast="auto" lang="EN-US" xml:lang="EN-US"><span class="NormalTextRun SCXW71272603 BCX0">If you need further </span><span class="NormalTextRun SCXW71272603 BCX0">assistance</span><span class="NormalTextRun SCXW71272603 BCX0">, kindly contact our support at </span></span><span style="color: rgb(53, 152, 219);">**<span class="TextRun SCXW71272603 BCX0" data-contrast="none" lang="EN-US" xml:lang="EN-US"><span class="NormalTextRun SCXW71272603 BCX0">support@cytechint.com</span></span>**</span><span class="TextRun SCXW71272603 BCX0" data-contrast="auto" lang="EN-US" xml:lang="EN-US"><span class="NormalTextRun SCXW71272603 BCX0"> for prompt </span><span class="NormalTextRun SCXW71272603 BCX0">assistance</span><span class="NormalTextRun SCXW71272603 BCX0"> and guidance.</span></span><span class="EOP SCXW71272603 BCX0" data-ccp-props="{}"></span>*</span>