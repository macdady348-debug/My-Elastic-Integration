# AQUILA - Atlassian Jira Integration through Oauth 2.0(3LO) and API Key

#### <span style="color: rgb(53, 152, 219);">**What are API Token Scopes?**</span>

Scopes define what actions an API token is allowed to perform in Atlassian apps such as Jira and Confluence. They enhance security by limiting permissions to only what's needed (e.g., read-only access to audit logs). Always use scoped tokens for AQUILA integrations—unscoped tokens are deprecated for most apps and may not support fine-grained access. For audit logs (events like user actions, config changes, or security incidents), use the specific scopes listed below. Broader scopes may be needed for other integrations (e.g., content indexing) but stick to these for basic monitoring to minimize risk.

##### <span style="color: rgb(53, 152, 219);">**Prerequisites**</span>

1. Atlassian Admin Email Account
2. Atlest have a read rights linux privilege
3. Broswer (firefox,chrome)

---

#### <span style="color: rgb(53, 152, 219);">**Creating an API Token with Scopes**</span>

1. Log in to [https://id.atlassian.com/manage-profile/security/api-tokens](https://id.atlassian.com/manage-profile/security/api-tokens).
2. Select "Create API token with scopes".
3. Enter a descriptive name for the token (e.g., "AQUILA- Audit Logs Monitoring").

Choose an expiration date for the token (between 1 and 365 days; consider shorter for security).

1. Select the application Jira.
2. Select the scopes or permissions the token should have: 
    - For Jira (audit logs): <span style="color: rgb(0, 0, 0);"><span class="sc-iiUIRa jEMjmA">**Classic**</span>:</span> `manage:jira-configuration or <span style="color: rgb(0, 0, 0);"><strong>Granular</strong></span>: read:audit-log:jira,read:user:jira` to access /rest/api/3/auditing/record.
3. Click "Create".
4. Copy the token and save it securely. You cannot view it again after this step. If lost, generate a new one. Share only with trusted integrations like AQUILA—revoke if compromised.

---

#### <span style="color: rgb(53, 152, 219);">**<span data-teams="true">OAuth 2.0(3LO)apps</span>**</span>

<span data-teams="true">*OAuth 2.0 (3LO)* (also known as "three-legged OAuth" or "authorization code grants") apps. OAuth 2.0 (3LO) allows external applications</span>

<span data-teams="true"> and services to access Atlassian product APIs on a user's behalf. OAuth 2.0 (3LO) apps are created and managed in the [developer console](https://developer.atlassian.com/console/myapps/).</span>

---

##### <span style="color: rgb(53, 152, 219);">**<span data-teams="true">Enabling OAuth 2.0(3LO)</span>**</span>

<span data-teams="true">1. Select your profile icon in the top-right corner, and from the dropdown, select Developer console.</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/scaled-1680-/lVVBIvX9SFkYAF3G-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/lVVBIvX9SFkYAF3G-image.png)

<span data-teams="true">2. Select your app from the list (or create one if you don't already have one).</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/scaled-1680-/AKDu1oWN0QYGsdAR-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/AKDu1oWN0QYGsdAR-image.png)[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/scaled-1680-/4mXowETiREvBPsmB-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/4mXowETiREvBPsmB-image.png)

<span data-teams="true">3. Select Permissions in the left menu.</span>

<span data-teams="true">4. Next to the API you want to add, select Configure or Add.</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/scaled-1680-/KdSLfu058Xr8DEZd-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/KdSLfu058Xr8DEZd-image.png)

<p class="callout info"><span style="color: rgb(0, 0, 0);">**Select Classic or Granular scope Classic:manage:jira-configuration was selected in this example.**</span>  
</p>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/scaled-1680-/3qR0Dexu0wp9FzWJ-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/3qR0Dexu0wp9FzWJ-image.png)

<span data-teams="true">5. Select Authorization in the left menu.</span>

<span data-teams="true">6. Next to OAuth 2.0 (3LO), select Configure or Add.</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/scaled-1680-/gtrKQRAoCYfo2kMp-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/gtrKQRAoCYfo2kMp-image.png)

<span data-teams="true">7. Enter the Callback URL. Set this to any URL that is accessible by the app. When you implement OAuth 2.0 (3LO) in your app (see next section), </span>

<p class="callout info">**<span data-teams="true" style="color: rgb(0, 0, 0);">The redirect\_uri must match this URL.</span>**</p>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/scaled-1680-/kZ2esEByR97f2Mfm-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/kZ2esEByR97f2Mfm-image.png)

<span data-teams="true">8. Click Save changes.</span>

<span data-teams="true">9. In <span style="color: rgb(0, 0, 0);">**Authorization URL generator**</span> Copy and Paste in the browser.</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/scaled-1680-/jufbJ3pRsu1tdHiz-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/jufbJ3pRsu1tdHiz-image.png)

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/scaled-1680-/iSLb89AftQMgWr9y-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/iSLb89AftQMgWr9y-image.png)

<span data-teams="true">10. Copy the URL and paste in text editor (notepad).</span>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/scaled-1680-/J4qUrEsy6hF7BCPI-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/J4qUrEsy6hF7BCPI-image.png)

<p class="callout info"><span style="color: rgb(0, 0, 0);">**Example of a copied URL (the boxed section contains the authorization code).**</span></p>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/scaled-1680-/UkbRlih2M4xs1pM7-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/UkbRlih2M4xs1pM7-image.png)

---

##### <span data-teams="true"> **<span style="color: rgb(53, 152, 219);">Exchange authorization code for access token</span>**</span>

<p class="callout info"><span data-teams="true" style="color: rgb(0, 0, 0);">**Paste this Curl command into terminal.**</span></p>

```c
curl --request POST \
  --url 'https://auth.atlassian.com/oauth/token' \
  --header 'Content-Type: application/json' \
  --data '{"grant_type": "authorization_code","client_id": "YOUR_CLIENT_ID","client_secret": "YOUR_CLIENT_SECRET","code": "YOUR_AUTHORIZATION_CODE","redirect_uri": "https://YOUR_APP_CALLBACK_URL"}'
 
```

Change all fields:

- `client_id`: (*required*) Set this to the <span style="color: rgb(0, 0, 0);">**Client ID**</span> for your app. Find this in <span style="color: rgb(0, 0, 0);">**Settings**</span> for your app in the [developer console](https://developer.atlassian.com/console/myapps/ "https://developer.atlassian.com/console/myapps/").
- `client_secret`: (*required*) Set this to the <span style="color: rgb(0, 0, 0);">**Secret**</span> for your app. Find this in <span style="color: rgb(0, 0, 0);">**Settings**</span> for your app in the [developer console](https://developer.atlassian.com/console/myapps/ "https://developer.atlassian.com/console/myapps/").
- `code`: (*required*) Set this to the authorization code received from the initial authorize call (described above).
- `redirect_uri`: (*required*) Set this to the callback URL configured for your app in the [developer console](https://developer.atlassian.com/console/myapps/ "https://developer.atlassian.com/console/myapps/").

<p class="callout info"><span style="color: rgb(0, 0, 0);">**<span data-teams="true">If successful, this call returns an access token similar to this:</span>**</span></p>

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
  "access_token": <string>,
  "expires_in": <expiry time of access_token in second>,
  "scope": <string>
}
 
```

---

##### <span style="color: rgb(53, 152, 219);">**Make calls to the API using the access token Get the `cloudid` for your site**</span>

Your app now has an access token that it can use to authorize requests to the APIs for the Atlassian site. To make requests, do the following:

1. <span style="color: rgb(0, 0, 0);">**Get the <span class="sc-htoDjs eujlDE">`cloudid`</span> for your site.**</span>
2. Construct the request URL using the <span class="sc-htoDjs eujlDE">`cloudid`</span>.
3. Call the API, using the access token and request URL.

---

##### <span style="color: rgb(53, 152, 219);">**Get the <span class="sc-htoDjs eujlDE">`cloud_id`</span> for your site**</span>

Make a GET request to [https://api.atlassian.com/oauth/token/accessible-resources<span aria-label="Follow" class="css-1wits42" role="img"><svg height="24" role="presentation" viewbox="0 0 24 24" width="24"><g fill="currentColor" fill-rule="evenodd"><path d="M11.031 7A1.03 1.03 0 0010 8.036a1.05 1.05 0 001.044 1.045l3.121.014.014 3.121a1.05 1.05 0 001.045 1.044 1.03 1.03 0 001.036-1.035l-.019-4.161a1.053 1.053 0 00-1.045-1.045L11.035 7h-.004z"></path><path d="M13.364 8.292l-7.072 7.071a1.002 1.002 0 000 1.415c.39.39 1.024.39 1.415 0l7.071-7.071A1.002 1.002 0 0014.071 8a1 1 0 00-.707.292z"></path></g></svg></span>](https://api.atlassian.com/oauth/token/accessible-resources) passing the access token as a bearer token in the header of the request. For example:

<p class="callout warning"><span style="color: rgb(0, 0, 0);">**Replace "ACCESS\_TOKEN" with your newly generated token.**</span></p>

```c
curl --request GET \
  --url https://api.atlassian.com/oauth/token/accessible-resources \
  --header 'Authorization: Bearer ACCESS_TOKEN' \
  --header 'Accept: application/json'

```

This will retrieve the sites that have scopes granted by the token (see [Check site access for the app](https://developer.atlassian.com/cloud/jira/platform/oauth-2-3lo-apps/#siteaccess) below for details). Find your site in the response and copy the <span class="sc-htoDjs eujlDE">`id`</span>. This is the <span class="sc-htoDjs eujlDE">`cloud_id`</span> for your site.

<p class="callout info"><span style="color: rgb(0, 0, 0);"> **sample output: a Jira site:**</span></p>

```json
[
  {
    "id": "1324a887-45db-1bf4-1e99-ef0ff456d421",
    "name": "Site name",
    "url": "https://your-domain.atlassian.net",
    "scopes": [
      "write:jira-work",
      "read:jira-user",
      "manage:jira-configuration"
    ],
    "avatarUrl": "https:\/\/site-admin-avatar-cdn.prod.public.atl-paas.net\/avatars\/240\/flag.png"
  }
]

```

---

---

#### <span style="color: rgb(53, 152, 219);">**Creating the Bash Wrapper Script:**</span>

<p class="callout info"><span style="color: rgb(0, 0, 0);">**The wrapper script manages event collection, logging, and ensures only one instance runs at a time.**</span></p>

1. **Create required directories for data, logs:**

<p class="callout info"><span style="color: rgb(0, 0, 0);">**Create a directory named `jira` (for this example):**</span></p>

```
mkdir jira
```

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/scaled-1680-/8hPJBMDmhrLARrmY-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/8hPJBMDmhrLARrmY-image.png)

**2. Create the wrapper script file:**

```
sudo nano /usr/local/bin/jira_audit.sh
```

**3. Add the following content to the file:**

<p class="callout warning"><span style="color: rgb(0, 0, 0);">**Replace the file path `FLAT_FILE` as well as `CLOUD_ID`, `USER_EMAIL`, and `API_KEY`, with their actual values.**</span></p>

```bash
#!/bin/bash

set -euo pipefail

# ----------------------------
# Config
# ----------------------------
FLAT_FILE="/home/testing-jira/jira/flattened.json"
LOCK_FILE="/var/lock/jira-events.lock"
CLOUD_ID="PASTE_YOUR_CLOUD_ID"
USER_EMAIL="PASTE_YOUR_EMAIL"
API_KEY="PASTE_YOUR_API_KEY"

# Ensure only one instance runs at a time
exec 200>"$LOCK_FILE"
flock -n 200 || exit 1

# ----------------------------
# Call Jira API
# ----------------------------
RESPONSE=$(curl -s --request GET \
  --url "https://api.atlassian.com/ex/jira/$CLOUD_ID/rest/api/3/auditing/record" \
  --user "$USER_EMAIL:$API_KEY" \
  --header "Accept: application/json")

# ----------------------------
# Validate JSON
# ----------------------------
if ! echo "$RESPONSE" | jq . >/dev/null 2>&1; then
  echo "Error: API did not return valid JSON"
  exit 1
fi

# ----------------------------
# Flatten directly to file
# ----------------------------
echo "$RESPONSE" | jq -c '
  .offset as $offset
  | .total as $total
  | (.records // [])[]
  | .offset = $offset
  | .total = $total
' > "$FLAT_FILE"

# ----------------------------
# Count records
# ----------------------------
count=$(echo "$RESPONSE" | jq '(.records // []) | length')

echo "Flattened $count records into $FLAT_FILE"
```

<span class="token token">4. **Make the script executable:**</span>

```
sudo chmod +x /usr/local/bin/jira_audit.sh
```

<span class="token token">5. **Set ownership to the dedicated user**:</span>

<p class="callout warning"><span style="color: rgb(0, 0, 0);">**<span class="token token">Replace the "user:group" in this example both `testing-jira:testing-jira`.</span>**</span></p>

```
sudo chown testing-jira:testing-jira /usr/local/bin/jira_audit.sh
```

---

#### <span style="color: rgb(53, 152, 219);">**Creating the Systemd Service File:**</span>

<p class="callout info"><span style="color: rgb(0, 0, 0);">**<span class="token token">The systemd </span><span class="token token">service</span><span class="token token"> ensures the event collection runs continuously and restarts automatically on failure.</span>**</span></p>

**<span class="token token">1. Create the service file:</span>**

```
sudo nano /etc/systemd/system/jira-audit.service
```

**<span class="token token">2. Add the following content to the file:</span>**

<p class="callout warning"><span style="color: rgb(0, 0, 0);">**<span class="token token">Replace WorkingDirectory file path.</span>**</span></p>

```ini
[Unit]
Description=Jira Audit Log Fetcher
After=network.target
Wants=network-online.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/jira_audit.sh

# Run as your user
User=testing-cato
WorkingDirectory=/home/testing-cato/jira

# Logging
StandardOutput=journal
StandardError=journal

# Security (optional but recommended)
NoNewPrivileges=true

[Install]
WantedBy=multi-user.target

```

---

#### <span style="color: rgb(53, 152, 219);">**Create a Systemd Timer to handle looping and run automatically in the background:**</span>

A systemd timer is a feature of <span class="hover:entity-accent entity-underline inline cursor-pointer align-baseline"><span class="whitespace-normal">systemd</span></span> used to schedule tasks to run automatically at specific times or intervals.

**<span class="token token">1. Create the timer file:</span>**

```
sudo nano /etc/systemd/system/jira-audit.timer
```

**<span class="token token">2. Add the following content to the file:</span>**

```bash
[unit]
Description=Run Jira Audit every 10 minutes

[Timer]
OnBootSec=60
OnUnitActiveSec=600
Persistent=true
Unit=jira-audit.service

[Install]
WantedBy=timers.target

```

---

---

#### <span style="color: rgb(53, 152, 219);">**<span class="token token">Configuring Log Rotation</span>**</span>

<span class="token token">To manage log file sizes and prevent disk space issues, configure log rotation for Jira Audit Logs.</span>

##### <span class="token token">**Step 1:** Create a logrotate configuration file:</span>

```
sudo nano /etc/logrotate.d/jira_audit
```

##### <span class="token token">**Step 2:** Add the following configuration to the file:</span>

<p class="callout warning"><span style="color: rgb(0, 0, 0);">**Replace `/home/your_user/` with your actual path, and update `user_owner` and `user_group` to match the correct user and group.**</span></p>

```
/home/your_user/jira/flattened.json {
    daily
    rotate 7
    missingok
    notifempty

    copytruncate

    compress
    compressoptions -1
    delaycompress

    dateext
    dateformat -%Y%m%d-%H%M%S

    create 0640 user_owner user_group
}
```

---

#### <span style="color: rgb(53, 152, 219);">**Enabling and Starting the Service**</span>

**Step 1:** **Reload systemd to recognize the new service file:**

```
sudo systemctl daemon-reload
```

**Step 2: Enable the service to start automatically on boot:**

```
sudo systemctl enable --now jira-audit.timer
```

**Step 3: Verify the service is running:**

```
sudo systemctl list-timers
```

<p class="callout info">**<span style="color: rgb(0, 0, 0);">You should see something like this.</span>**</p>

[![image.png](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/scaled-1680-/Ny6BCgorzIHNd8Tw-image.png)](https://cytechint-docs-bookstack.s3.amazonaws.com/uploads/images/gallery/2026-04/Ny6BCgorzIHNd8Tw-image.png)

---

##### <span style="color: rgb(53, 152, 219);">**Monitoring Live Logs:**</span>

To view real-time logs from the jira-audit service:

```
journalctl -u jira-audit -f
```

<p class="callout info"><span style="color: rgb(0, 0, 0);">**Please provide the following information to CyTech.**</span></p>

- <span style="color: rgb(0, 0, 0);">**Jira User Identifier**:</span> Your Atlassian email address (must be linked to an admin account as noted above).
- <span style="color: rgb(0, 0, 0);">**Jira API Token**:</span> The scoped token created above.
- <span style="color: rgb(0, 0, 0);">**Cloud\_ID**:</span> The ID retrieved from accessible-resources api call