# Cloudflare GUI Workshop Lab

**Hands-on Workshop: Cloudflare Workers/Pages and DNS Security via Dashboard**

| Detail | Value |
|--------|-------|
| Duration | 4 hours |
| Format | GUI-only (no CLI installation required) |
| Level | Beginner to Intermediate |
| Language | English |

---

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Time Schedule](#time-schedule)
- [Part A: Cloudflare Workers](#part-a--cloudflare-workers-gui-only)
- [Part B: Cloudflare Pages](#part-b--cloudflare-pages-gui-only)
- [Part C: Zero Trust DNS Security](#part-c--zero-trust-dns-security-via-browser-doh)
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

### Access Verification (Do This Before the Workshop)

1. Open https://dash.cloudflare.com/ → You should be able to log in
2. Open https://one.dash.cloudflare.com/ → You should see the Zero Trust Dashboard

> ⚠️ **Note:** If you cannot access these, please inform the instructor before the workshop begins.

---

## Time Schedule

| Time | Topic | Details |
|------|-------|---------|
| 0:00 – 0:15 | Introduction | Introductions, access verification |
| 0:15 – 1:30 | **Part A: Workers** | Create, edit, deploy, variables, logs |
| 1:30 – 2:00 | **Part B: Pages** | Deploy static website via Dashboard |
| 2:00 – 2:10 | Break | ☕ |
| 2:10 – 3:50 | **Part C: DNS Security** | Create policy, configure DoH, test, view logs |
| 3:50 – 4:00 | Q&A + Summary | Questions, cleanup |

---

## Part A — Cloudflare Workers (GUI-only)

### A1) Navigate to Workers & Pages

**Goal:** Access the Workers and Pages management page on Cloudflare Dashboard

#### Steps:

1. Open your browser and go to:
   ```
   https://dash.cloudflare.com/
   ```

2. Log in with your Cloudflare account email and password

3. After successful login, you will see the main Dashboard

4. In the **left sidebar menu**, click on **"Workers & Pages"**

   ```
   📍 Location: Left sidebar > Workers & Pages
   ```

#### ✅ Verification:

- [ ] You can see the "Workers & Pages" overview page
- [ ] You can see the "Create" or "Create application" button

#### 📸 Screenshot placeholder:

```
[Image: Workers & Pages overview page with Create button]
```

---

### A2) Create a New Worker (Hello World)

**Goal:** Create your first Worker and deploy it to production

#### Steps:

1. On the Workers & Pages page, click the **"Create"** button (or "Create application")

2. Select **"Create Worker"**

3. Set the Worker name:
   - **Name:** `my-first-worker` (or any name you prefer)
   
   > 💡 This name will be part of your URL: `my-first-worker.xxx.workers.dev`

4. Click **"Deploy"** (using the default Hello World template)

5. Wait until you see the "Success" or "Deployed" message

#### ✅ Verification:

- [ ] You can see the Worker URL, e.g., `https://my-first-worker.xxx.workers.dev`
- [ ] Clicking the URL shows "Hello World!" or similar message

#### 🧪 Test:

1. Click on the Worker URL (or copy and paste it into a new browser tab)
2. You should see a response from the Worker

#### 📸 Screenshot placeholder:

```
[Image: Worker overview page after successful deployment with URL]
```

#### 🔄 Rollback (if you want to delete):

1. Go to Workers & Pages
2. Click on the Worker name
3. Go to Settings > Delete
4. Type the Worker name to confirm, then click Delete

---

### A3) Edit Worker Code to Return JSON

**Goal:** Learn how to edit code and redeploy via the Dashboard

#### Steps:

1. On your Worker page, click the **"Code"** tab (or "Edit code")

2. You will see a code editor in the browser

3. **Delete all existing code** and **paste the following code:**

```javascript
export default {
  async fetch(request, env) {
    const url = new URL(request.url);

    // If accessing /api/hello path, return JSON
    if (url.pathname === '/api/hello') {
      return Response.json({
        ok: true,
        message: 'Hello from Cloudflare Workers (GUI deploy)!',
        environment: env.ENVIRONMENT ?? 'not-set',
        timestamp: new Date().toISOString(),
      });
    }

    // For other paths, return plain text
    return new Response('Worker is running. Try visiting /api/hello', {
      headers: { 'content-type': 'text/plain; charset=utf-8' },
    });
  },
};
```

4. Click the **"Save and deploy"** button (or "Deploy")

5. Wait until you see the success message

#### ✅ Verification:

- [ ] Deployment succeeded without errors
- [ ] Visiting `https://your-worker.xxx.workers.dev/` shows "Worker is running..."
- [ ] Visiting `https://your-worker.xxx.workers.dev/api/hello` shows JSON response

#### 🧪 Test:

1. Open a new tab and go to your Worker URL + `/api/hello`
   ```
   https://my-first-worker.xxx.workers.dev/api/hello
   ```

2. You should see JSON like this:
   ```json
   {
     "ok": true,
     "message": "Hello from Cloudflare Workers (GUI deploy)!",
     "environment": "not-set",
     "timestamp": "2025-02-05T10:30:00.000Z"
   }
   ```

#### 📸 Screenshot placeholder:

```
[Image: Code editor with new code and Save and deploy button]
[Image: Browser showing JSON response from /api/hello]
```

---

### A4) Add an Environment Variable

**Goal:** Learn how to configure settings without changing code

#### Steps:

1. On your Worker page, click the **"Settings"** tab

2. Scroll down to find the **"Variables and Secrets"** section (or "Environment Variables")

3. Click **"Add variable"** (or "Add")

4. Enter the following:
   - **Variable name:** `ENVIRONMENT`
   - **Value:** `workshop-demo`

5. Click **"Save"** (or "Save and deploy")

   > ⚠️ Some UI versions may require clicking Deploy again after saving

#### ✅ Verification:

- [ ] The variable appears in the list
- [ ] Visiting `/api/hello` shows `"environment": "workshop-demo"`

#### 🧪 Test:

1. Open `https://your-worker.xxx.workers.dev/api/hello`
2. Check the `environment` value in the JSON response
3. You should see:
   ```json
   {
     "ok": true,
     "message": "Hello from Cloudflare Workers (GUI deploy)!",
     "environment": "workshop-demo",
     "timestamp": "..."
   }
   ```

#### 📸 Screenshot placeholder:

```
[Image: Settings > Variables page with ENVIRONMENT variable]
[Image: JSON response showing environment: "workshop-demo"]
```

#### 🔄 Rollback:

1. Go to Settings > Variables
2. Click Delete on the variable you want to remove
3. Save and redeploy

---

### A5) View Logs and Observability

**Goal:** Learn how to monitor and debug your Worker

#### Steps:

1. On your Worker page, click the **"Logs"** tab (or "Observability")

2. You will see the Real-time Logs page

3. Open a new browser tab and visit your Worker URL multiple times:
   - `https://your-worker.xxx.workers.dev/`
   - `https://your-worker.xxx.workers.dev/api/hello`
   - `https://your-worker.xxx.workers.dev/test`

4. Return to the Logs page and you will see the incoming requests

#### ✅ Verification:

- [ ] You can see log entries for the requests you just made
- [ ] You can see information: timestamp, path, status code, duration

#### 📸 Screenshot placeholder:

```
[Image: Logs page showing request entries]
```

#### 💡 Tips:

- If you don't see logs, wait a moment and refresh
- You can filter logs by status code or path
- If there are errors, you will see the stack trace in the logs

---

## Part B — Cloudflare Pages (GUI-only)

### B1) Prepare Files for Deployment

**Goal:** Understand the file structure to be deployed

#### Sample files in this repository:

```
pages-site/
└── index.html    ← Single-page website file
```

#### Contents of `pages-site/index.html`:

A simple HTML page that displays a confirmation message when deployment is successful.

---

### B2) Deploy Pages via GitHub (Recommended)

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

### B3) Deploy Pages via Direct Upload (Alternative)

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

## Part C — Zero Trust DNS Security (via Browser DoH)

### C1) Access Zero Trust Dashboard

**Goal:** Access the Cloudflare Zero Trust Dashboard

#### Steps:

1. Open your browser and go to:
   ```
   https://one.dash.cloudflare.com/
   ```

2. Log in with the same Cloudflare account used in Part A and B

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

### C2) Create a DNS Location (to get DoH endpoint)

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

### C3) Create a DNS Policy (Block Malware)

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

### C4) Configure Browser to Use DoH Endpoint

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

### C5) Test Blocking with Test Domains

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

### C6) View DNS Logs

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

## Troubleshooting

### Problem: Worker deployment failed

**Possible causes:**
- Syntax error in the code
- Worker name already exists

**Solutions:**
1. Check the error message displayed
2. Verify that the code was copied completely
3. Try using a different Worker name

---

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
2. Clear DNS cache (see instructions in step C5)
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

## 🎉 Summary

After completing this workshop, you will have learned:

| Topic | Skills Acquired |
|-------|-----------------|
| **Workers** | Create, edit, deploy, configure variables, view logs |
| **Pages** | Deploy static websites via GitHub or direct upload |
| **DNS Security** | Create policies, configure DoH in browser, test blocking, view logs |

---

## 📚 Additional Resources

- [Cloudflare Workers Documentation](https://developers.cloudflare.com/workers/)
- [Cloudflare Pages Documentation](https://developers.cloudflare.com/pages/)
- [Cloudflare Zero Trust Documentation](https://developers.cloudflare.com/cloudflare-one/)
- [DNS Filtering Test Domains](https://developers.cloudflare.com/cloudflare-one/policies/gateway/dns-policies/test-dns-filtering/)

---

## License

MIT
