# Cloudflare Workshop Lab

A step-by-step guide to deploying Cloudflare Pages and configuring DNS Security via the Dashboard.

---

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Part A: Cloudflare Pages](#part-a--cloudflare-pages)
- [Part B: Zero Trust DNS Security](#part-b--zero-trust-dns-security-via-browser-doh)
- [Part C: ZTNA with Remote Browser Isolation](#part-c--ztna-with-remote-browser-isolation)
- [Troubleshooting](#troubleshooting)

---

## Prerequisites

### What You Need Before Starting

| Item | Description | Required |
|------|-------------|----------|
| **Cloudflare Account** | Sign up free at https://dash.cloudflare.com/sign-up | ✅ Required |
| **Web Browser** | Chrome, Edge, or Firefox (latest version) | ✅ Required |
| **Internet Connection** | Stable connection, able to access `*.cloudflare.com` | ✅ Required |
| **GitHub Account** | For deploying Pages via GitHub integration | ⭐ Recommended |

### Access Verification

1. Open https://dash.cloudflare.com/ → You should be able to log in
2. Open https://one.dash.cloudflare.com/ → You should see the Zero Trust Dashboard

---

## Part A — Cloudflare Pages

### A1) Prepare Files for Deployment

**Goal:** Understand the file structure to be deployed

#### Sample files in this repository:

```
pages-site/
└── index.html    ← Single-page website file
```

#### Contents of `pages-site/index.html`:

A simple HTML page that displays a confirmation message when deployment is successful.

---

### A2) Deploy Pages via GitHub (Recommended)

**Goal:** Deploy a static website by connecting to GitHub

#### Steps:

**Step 1: Fork this repository**

1. Open GitHub and go to this repository:
   ```
   https://github.com/pongpisit/cloudflare-gui-workshop-lab
   ```

2. Click the **"Fork"** button (top right corner)

3. Select your account as the destination

4. Wait for the fork to complete

**Step 2: Create a Pages project on Cloudflare**

5. Open Cloudflare Dashboard: https://dash.cloudflare.com/

6. Go to **Workers & Pages** (left sidebar)

7. Click **"Create"** > Select **"Pages"**

8. Select **"Connect to Git"**

9. Click **"Connect GitHub"** (if not already connected)

10. Authorize Cloudflare to access your GitHub account

**Step 3: Select repository and configure settings**

11. Select repository: `cloudflare-gui-workshop-lab` (the one you forked)

12. Configure build settings:

    | Setting | Value |
    |---------|-------|
    | **Project name** | `my-workshop-site` (or any name you prefer) |
    | **Production branch** | `main` |
    | **Framework preset** | `None` |
    | **Build command** | *(leave empty)* |
    | **Build output directory** | `pages-site` |

13. Click **"Save and Deploy"**

14. Wait 1-2 minutes for the deployment to complete

#### ✅ Verification:

- [ ] You see "Success" or "Deployment complete" message
- [ ] You receive a URL like `https://my-workshop-site.pages.dev`
- [ ] Opening the URL shows the web page

#### 🧪 Test:

1. Click the URL you received (or copy and open in a new tab)
2. You should see a web page with the message "Cloudflare Pages (GUI-only) — Lab Site"

#### 📸 Screenshot placeholder:

```
[Image: Connect to Git page selecting repository]
[Image: Build settings configuration page]
[Image: Deployment success page with URL]
[Image: Successfully deployed web page]
```

#### 🔄 Rollback:

1. Go to Workers & Pages
2. Click on the Pages project name
3. Go to Settings > Delete project
4. Type the project name to confirm

---

### A3) Deploy Pages via Direct Upload (Alternative)

**Use this method if:** You cannot use GitHub or want to upload files directly

#### Steps:

1. Download the `pages-site/index.html` file from this repository to your computer

2. Go to Cloudflare Dashboard > **Workers & Pages**

3. Click **"Create"** > Select **"Pages"**

4. Select **"Upload assets"** (or "Direct Upload")

5. Set project name: `my-workshop-site`

6. Drag and drop the `index.html` file into the upload area
   
   Or click "Select files" and choose the file

7. Click **"Deploy site"**

#### ✅ Verification:

- [ ] You receive a `*.pages.dev` URL
- [ ] Opening the URL shows the web page

---

## Part B — Zero Trust DNS Security (via Browser DoH)

### B1) Access Zero Trust Dashboard

**Goal:** Access the Cloudflare Zero Trust Dashboard

#### Steps:

1. Open your browser and go to:
   ```
   https://one.dash.cloudflare.com/
   ```

2. Log in with your Cloudflare account

3. **If this is your first time:** The system will prompt you to create a Zero Trust organization
   - Set team name: e.g., `my-workshop-org`
   - Select plan: **Free** (for the workshop)
   - Fill in the information as requested

4. After creation, you will see the Zero Trust Dashboard

#### ✅ Verification:

- [ ] You can see the Zero Trust Dashboard
- [ ] You can see the left sidebar menu: Networks, Traffic policies, Insights, etc.

#### 📸 Screenshot placeholder:

```
[Image: Zero Trust Dashboard main page]
```

---

### B2) Create a DNS Location (to get DoH endpoint)

**Goal:** Create a DNS Location and get the DoH URL for use in your browser

#### Steps:

1. In the Zero Trust Dashboard, click **"Networks"** in the left sidebar

2. Click **"Resolvers & Proxies"**

3. Click **"DNS Locations"**

4. Click **"Add a location"**

5. Enter the following:
   - **Name:** `Workshop-Browser-DoH`
   - Other options: use default values

6. Click **"Add location"** or **"Save"**

7. **Important:** After creation, **note down the DoH endpoint** displayed by the system

   Format:
   ```
   https://xxxxxx.cloudflare-gateway.com/dns-query
   ```

   > ⚠️ The `xxxxxx` value will be different for each account. Copy your actual value.

#### ✅ Verification:

- [ ] You can see the DNS Location in the list
- [ ] You have noted down the DoH URL

#### 📸 Screenshot placeholder:

```
[Image: Add DNS Location page]
[Image: Created DNS Location with DoH URL]
```

#### 🔄 Rollback:

1. Go to Networks > Resolvers & Proxies > DNS Locations
2. Click on the location you want to delete
3. Click Delete

---

### B3) Create a DNS Policy (Block Malware)

**Goal:** Create a policy to block malicious websites

#### Steps:

1. In the Zero Trust Dashboard, click **"Traffic policies"** in the left sidebar

2. Click **"Firewall policies"**

3. Click the **"DNS"** tab

4. Click **"Add a policy"**

5. Enter the following:

   **Name section:**
   - **Name:** `Block Malware (Workshop Demo)`

   **Traffic section (Build an expression):**
   
   | Field | Value |
   |-------|-------|
   | **Selector** | `Security Categories` |
   | **Operator** | `in` |
   | **Value** | Select `Malware` (or `All security risks` for broader coverage) |

6. **Action section:**
   - Select **"Block"**

7. Click **"Create policy"** or **"Save"**

#### ✅ Verification:

- [ ] The policy appears in the DNS policies list
- [ ] Status is "Active" or "Enabled"

#### 📸 Screenshot placeholder:

```
[Image: Add DNS Policy page - Traffic expression section]
[Image: Add DNS Policy page - Action section]
[Image: DNS policies list after creation]
```

#### 🔄 Rollback:

1. Go to Traffic policies > Firewall policies > DNS
2. Click on the policy you want to modify
3. Click Disable or Delete

---

### B4) Configure Browser to Use DoH Endpoint

**Goal:** Configure your browser to use Cloudflare Gateway DNS instead of regular DNS

#### For Google Chrome:

1. Open Chrome and go to:
   ```
   chrome://settings/security
   ```
   
   Or: Settings > Privacy and security > Security

2. Scroll down to find the **"Use secure DNS"** section

3. Enable it (toggle on)

4. Select **"With Custom"** or **"Customized"**

5. Paste the DoH URL you noted from step C2:
   ```
   https://xxxxxx.cloudflare-gateway.com/dns-query
   ```

6. Press Enter or click outside the input field

#### For Microsoft Edge:

1. Open Edge and go to:
   ```
   edge://settings/privacy
   ```
   
   Or: Settings > Privacy, search, and services

2. Scroll down to find the **"Security"** section

3. Find **"Use secure DNS to specify how to lookup the network address for websites"**

4. Enable it and select **"Choose a service provider"**

5. Select **"Enter custom provider"**

6. Paste the DoH URL:
   ```
   https://xxxxxx.cloudflare-gateway.com/dns-query
   ```

#### For Mozilla Firefox:

1. Open Firefox and go to:
   ```
   about:preferences#privacy
   ```
   
   Or: Settings > Privacy & Security

2. Scroll down to find the **"DNS over HTTPS"** section

3. Select **"Max Protection"** or **"Increased Protection"**

4. In the "Choose provider" dropdown, select **"Custom"**

5. Paste the DoH URL:
   ```
   https://xxxxxx.cloudflare-gateway.com/dns-query
   ```

6. Click **"OK"** or close the settings page

#### ✅ Verification:

- [ ] Secure DNS / DoH is configured
- [ ] Cloudflare Gateway DoH URL is entered

#### 📸 Screenshot placeholder:

```
[Image: Chrome - Security settings page with DoH URL]
[Image: Edge - Privacy settings page with DoH URL]
[Image: Firefox - DNS over HTTPS settings]
```

#### 🔄 Rollback:

- **Chrome/Edge:** Disable "Use secure DNS" or select a different provider
- **Firefox:** Select "Off" or "Default Protection"

---

### B5) Test Blocking with Test Domains

**Goal:** Verify that the DNS policy is working correctly

#### Cloudflare Test Domains:

Cloudflare provides special domains for testing category blocking without visiting actual malicious websites:

| Category | Test Domain |
|----------|-------------|
| Malware | `malware.testcategory.com` |
| Phishing | `phishing.testcategory.com` |
| Spam | `spam.testcategory.com` |
| Spyware | `spyware.testcategory.com` |
| Cryptomining | `cryptomining.testcategory.com` |
| New Domains | `newdomains.testcategory.com` |

#### Test Steps:

1. Open a new browser tab

2. Type in the URL bar:
   ```
   malware.testcategory.com
   ```

3. Press Enter

4. **Expected result:**
   - You see a **Cloudflare Block Page** (page indicating the site is blocked)
   - Or the browser shows an error "This site can't be reached"

#### ✅ Verification:

- [ ] You see a block page or error
- [ ] You cannot access `malware.testcategory.com`

#### 📸 Screenshot placeholder:

```
[Image: Cloudflare Block Page]
```

#### 💡 Tips:

- If you can still access the site, try:
  1. Clear the browser's DNS cache (close and reopen the browser)
  2. Wait 1-2 minutes for the policy to propagate
  3. Verify that DoH is configured correctly

#### How to Clear DNS Cache:

**Windows:**
```cmd
ipconfig /flushdns
```

**macOS:**
```bash
sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
```

**Browser:** Close all browser windows and reopen

---

### B6) View DNS Logs

**Goal:** Verify policy operation through logs

#### Steps:

1. Return to the Zero Trust Dashboard: https://one.dash.cloudflare.com/

2. Click **"Insights"** in the left sidebar

3. Click **"Logs"**

4. Select the **"Gateway"** > **"DNS"** tab

5. Find the log entry for `malware.testcategory.com`

   > 💡 You may need to wait 1-2 minutes for the log to appear

6. Click on the log entry to view details

#### ✅ Verification:

- [ ] You can see the log entry for the tested domain
- [ ] You can see Action: "Block" or "Blocked"
- [ ] You can see the Policy name that matched

#### 📸 Screenshot placeholder:

```
[Image: DNS Logs page showing blocked request]
[Image: Log entry details]
```

---

## Part C — ZTNA with Remote Browser Isolation

### C1) Create a Self-Hosted Application

**Goal:** Publish an internal application through Cloudflare Access for secure, agentless access

#### Steps:

1. In the Zero Trust Dashboard, click **"Access"** in the left sidebar

2. Click **"Applications"**

3. Click **"Add an application"**

4. Select **"Self-hosted"**

5. Configure the application:

   | Setting | Value |
   |---------|-------|
   | **Application name** | `Internal App Demo` |
   | **Session Duration** | `24 hours` (or your preference) |
   | **Application domain** | `internal-app.yourdomain.com` (use your domain) |

6. Click **"Next"**

#### ✅ Verification:

- [ ] Application name and domain are configured
- [ ] You can proceed to the next step

#### 📸 Screenshot placeholder:

```
[Image: Add Application - Self-hosted configuration]
```

---

### C2) Configure Access Policy

**Goal:** Define who can access the application

#### Steps:

1. In the **"Add policies"** section, create a new policy:

   | Setting | Value |
   |---------|-------|
   | **Policy name** | `Allow Workshop Users` |
   | **Action** | `Allow` |

2. Configure the policy rule:

   | Selector | Operator | Value |
   |----------|----------|-------|
   | **Emails** | `Emails ending in` | `@yourdomain.com` |

   > 💡 You can also use specific email addresses, groups, or other identity providers

3. Click **"Next"**

#### ✅ Verification:

- [ ] Policy is configured with appropriate access rules
- [ ] Action is set to "Allow"

#### 📸 Screenshot placeholder:

```
[Image: Access Policy configuration]
```

#### 🔄 Rollback:

1. Go to Access > Applications
2. Click on the application
3. Edit or delete the policy as needed

---

### C3) Enable Remote Browser Isolation

**Goal:** Add an extra layer of security by isolating the browser session

#### Steps:

1. In the application settings, scroll to **"Additional settings"** or go to **"Settings"** tab

2. Find the **"Browser Isolation"** section

3. Enable **"Isolate application"**

4. Configure isolation settings:

   | Setting | Recommended Value |
   |---------|-------------------|
   | **Disable copy** | ✅ Enabled (prevents copying content) |
   | **Disable paste** | ✅ Enabled (prevents pasting content) |
   | **Disable printing** | ✅ Enabled (prevents printing) |
   | **Disable download** | ✅ Enabled (prevents file downloads) |
   | **Disable upload** | ✅ Enabled (prevents file uploads) |
   | **Disable keyboard** | ❌ Disabled (allow typing) |

   > 💡 Adjust these settings based on your security requirements

5. Click **"Save"** or **"Add application"**

#### ✅ Verification:

- [ ] Browser Isolation is enabled
- [ ] Data protection settings are configured

#### 📸 Screenshot placeholder:

```
[Image: Browser Isolation settings]
```

---

### C4) Configure Tunnel (for Internal Applications)

**Goal:** Connect your internal application to Cloudflare without opening firewall ports

#### Steps:

1. In the Zero Trust Dashboard, click **"Networks"** in the left sidebar

2. Click **"Tunnels"**

3. Click **"Create a tunnel"**

4. Select **"Cloudflared"** and click **"Next"**

5. Name your tunnel:
   - **Tunnel name:** `workshop-tunnel`

6. Click **"Save tunnel"**

7. **Install the connector:** Follow the instructions shown for your operating system

   **For Docker:**
   ```bash
   docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token <YOUR_TOKEN>
   ```

   **For Windows/macOS/Linux:** Download and run the installer with the provided token

8. After the connector is running, configure the **Public hostname**:

   | Setting | Value |
   |---------|-------|
   | **Public hostname** | `internal-app.yourdomain.com` |
   | **Service Type** | `HTTP` or `HTTPS` |
   | **URL** | `localhost:8080` (your internal app address) |

9. Click **"Save tunnel"**

#### ✅ Verification:

- [ ] Tunnel status shows "Healthy" or "Connected"
- [ ] Public hostname is configured

#### 📸 Screenshot placeholder:

```
[Image: Tunnel creation page]
[Image: Tunnel connector installation]
[Image: Public hostname configuration]
```

#### 🔄 Rollback:

1. Go to Networks > Tunnels
2. Click on the tunnel
3. Click Delete tunnel

---

### C5) Test Agentless Access with RBI

**Goal:** Verify that users can access the application securely without installing any agent

#### Steps:

1. Open a **new browser** (or incognito/private window)

2. Navigate to your application URL:
   ```
   https://internal-app.yourdomain.com
   ```

3. You will be redirected to the **Cloudflare Access login page**

4. Authenticate using one of the configured methods:
   - One-time PIN (sent to your email)
   - Identity provider (Google, Azure AD, etc.)

5. After successful authentication:
   - You will see the application rendered in an **isolated browser**
   - Notice the **Cloudflare toolbar** at the top (if enabled)
   - Try copying text — it should be blocked (if configured)

#### ✅ Verification:

- [ ] Access login page appears
- [ ] Authentication works correctly
- [ ] Application loads in isolated browser
- [ ] Data protection controls work (copy/paste blocked)

#### 📸 Screenshot placeholder:

```
[Image: Cloudflare Access login page]
[Image: Application in isolated browser with toolbar]
```

#### 💡 Tips:

- The isolated browser runs on Cloudflare's edge, not on the user's device
- All rendering happens remotely — only pixels are sent to the user
- No data can be exfiltrated if copy/paste/download are disabled
- Works on any device with a modern browser — no agent required

---

### C6) View Access Logs

**Goal:** Monitor who accessed the application and when

#### Steps:

1. In the Zero Trust Dashboard, click **"Insights"** in the left sidebar

2. Click **"Logs"**

3. Select the **"Access"** tab

4. Find the log entries for your application access

5. Click on a log entry to view details:
   - User email
   - Authentication method
   - Access time
   - Application accessed
   - Action (allowed/blocked)

#### ✅ Verification:

- [ ] You can see Access log entries
- [ ] Your test access is recorded
- [ ] User and application details are visible

#### 📸 Screenshot placeholder:

```
[Image: Access Logs page]
[Image: Log entry details]
```

---

## Troubleshooting

### Problem: Pages deployment failed

**Possible causes:**
- Build output directory is incorrect
- No files in the specified directory

**Solutions:**
1. Verify that Build output directory is set to `pages-site`
2. Verify that the repository has a `pages-site/` folder with `index.html` inside

---

### Problem: DNS blocking not working

**Possible causes:**
- DoH URL is incorrect
- DNS cache still has old values
- Policy is not yet active

**Solutions:**
1. Verify that the DoH URL was copied correctly
2. Clear DNS cache (see instructions in step B5)
3. Wait 2-3 minutes for the policy to propagate
4. Verify that the policy status is "Active"

---

### Problem: Cannot see DNS Logs

**Possible causes:**
- Logs have not synced yet
- Filter is incorrect

**Solutions:**
1. Wait 2-5 minutes and refresh
2. Check the time range of the filter
3. Try clearing the filter and view all logs

---

### Problem: Access application not loading

**Possible causes:**
- Tunnel is not connected
- Public hostname misconfigured
- Origin server not running

**Solutions:**
1. Check tunnel status in Networks > Tunnels
2. Verify the service URL points to the correct internal address
3. Ensure the origin application is running and accessible locally

---

### Problem: Browser Isolation not working

**Possible causes:**
- Isolation not enabled for the application
- Browser not supported

**Solutions:**
1. Verify "Isolate application" is enabled in application settings
2. Use a supported browser (Chrome, Edge, Firefox, Safari)
3. Clear browser cache and try again

---

## 🎉 Summary

After completing this guide, you will have learned:

| Topic | Skills Acquired |
|-------|-----------------|
| **Pages** | Deploy static websites via GitHub or direct upload |
| **DNS Security** | Create policies, configure DoH in browser, test blocking, view logs |
| **ZTNA + RBI** | Publish internal apps, configure access policies, enable browser isolation, agentless access |

---

## 📚 Additional Resources

- [Cloudflare Pages Documentation](https://developers.cloudflare.com/pages/)
- [Cloudflare Zero Trust Documentation](https://developers.cloudflare.com/cloudflare-one/)
- [DNS Filtering Test Domains](https://developers.cloudflare.com/cloudflare-one/policies/gateway/dns-policies/test-dns-filtering/)
- [Cloudflare Access Documentation](https://developers.cloudflare.com/cloudflare-one/policies/access/)
- [Browser Isolation Documentation](https://developers.cloudflare.com/cloudflare-one/policies/browser-isolation/)
- [Cloudflare Tunnels Documentation](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/)

---

## License

MIT
