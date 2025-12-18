# Module 4: ZTNA with Remote Browser Isolation

Publish internal applications with Zero Trust Network Access (ZTNA) and Remote Browser Isolation (RBI) for secure, agentless access.

**Time needed: 45 minutes**

---

## What You Will Do

1. Create a Self-Hosted Application
2. Configure Access Policy
3. Enable Remote Browser Isolation
4. Configure Tunnel
5. Test Agentless Access
6. View Access Logs

---

## Step 1: Create a Self-Hosted Application

### 1.1 Navigate to Access Applications

1. In the Zero Trust Dashboard, click **"Access controls"** in the left sidebar

2. Click **"Applications"**

3. Click **"Add an application"**

4. Select **"Self-hosted"**

### 1.2 Configure the Application

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

## Step 2: Configure Access Policy

### 2.1 Create a Policy

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

1. Go to Access controls > Applications
2. Click on the application
3. Edit or delete the policy as needed

---

## Step 3: Enable Remote Browser Isolation

### 3.1 Configure Browser Isolation

1. In the application settings, scroll to **"Additional settings"** or go to **"Settings"** tab

2. Find the **"Browser Isolation"** section

3. Enable **"Isolate application"**

### 3.2 Configure Data Protection Settings

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

## Step 4: Configure Tunnel

### 4.1 Create a Tunnel

1. In the Zero Trust Dashboard, click **"Networks"** in the left sidebar

2. Click **"Tunnels"**

3. Click **"Create a tunnel"**

4. Select **"Cloudflared"** and click **"Next"**

5. Name your tunnel:
   - **Tunnel name:** `workshop-tunnel`

6. Click **"Save tunnel"**

### 4.2 Install the Connector

7. **Install the connector:** Follow the instructions shown for your operating system

   **For Docker:**
   ```bash
   docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token <YOUR_TOKEN>
   ```

   **For Windows/macOS/Linux:** Download and run the installer with the provided token

### 4.3 Configure Public Hostname

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

## Step 5: Test Agentless Access with RBI

### 5.1 Access the Application

1. Open a **new browser** (or incognito/private window)

2. Navigate to your application URL:
   ```
   https://internal-app.yourdomain.com
   ```

3. You will be redirected to the **Cloudflare Access login page**

### 5.2 Authenticate

4. Authenticate using one of the configured methods:
   - One-time PIN (sent to your email)
   - Identity provider (Google, Azure AD, etc.)

### 5.3 Verify Isolation

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

## Step 6: View Access Logs

### 6.1 Navigate to Logs

1. In the Zero Trust Dashboard, click **"Insights"** in the left sidebar

2. Click **"Logs"**

3. Select **"Access"** from the log type options

### 6.2 Find Your Log Entry

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

### Problem: Access application not loading

**Possible causes:**
- Tunnel is not connected
- Public hostname misconfigured
- Origin server not running

**Solutions:**
1. Check tunnel status in Networks > Tunnels
2. Verify the service URL points to the correct internal address
3. Ensure the origin application is running and accessible locally

### Problem: Browser Isolation not working

**Possible causes:**
- Isolation not enabled for the application
- Browser not supported

**Solutions:**
1. Verify "Isolate application" is enabled in application settings
2. Use a supported browser (Chrome, Edge, Firefox, Safari)
3. Clear browser cache and try again

---

## Summary

After completing this module, you have learned:

| Feature | What You Did |
|---------|--------------|
| **Self-Hosted App** | Published an internal application through Cloudflare Access |
| **Access Policy** | Defined who can access the application |
| **Browser Isolation** | Added data protection with RBI |
| **Tunnel** | Connected internal app without opening firewall ports |
| **Agentless Access** | Verified secure access without installing any agent |

---

## Additional Resources

- [Cloudflare Access Documentation](https://developers.cloudflare.com/cloudflare-one/policies/access/)
- [Browser Isolation Documentation](https://developers.cloudflare.com/cloudflare-one/policies/browser-isolation/)
- [Cloudflare Tunnels Documentation](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/)
