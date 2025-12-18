# Module 3: Zero Trust DNS Security

Configure DNS Security using Browser DNS over HTTPS (DoH).

**Time needed: 30 minutes**

---

## What You Will Do

1. Access Zero Trust Dashboard
2. Create a DNS Location
3. Create a DNS Policy
4. Configure Browser DoH
5. Test blocking
6. View DNS Logs

---

## Step 1: Access Zero Trust Dashboard

### 1.1 Open Zero Trust Dashboard

1. Open your browser and go to:
   ```
   https://one.dash.cloudflare.com/
   ```

2. Log in with your Cloudflare account

3. **If this is your first time:** The system will prompt you to create a Zero Trust organization
   - Set team name: e.g., `my-workshop-org`
   - Select plan: **Free**
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

## Step 2: Create a DNS Location

### 2.1 Navigate to DNS Locations

1. In the Zero Trust Dashboard, click **"Networks"** in the left sidebar

2. Click **"Resolvers & Proxies"**

3. Click **"DNS Locations"**

### 2.2 Add a New Location

4. Click **"Add a location"**

5. Enter the following:
   - **Name:** `Workshop-Browser-DoH`
   - Other options: use default values

6. Click **"Add location"** or **"Save"**

### 2.3 Note the DoH Endpoint

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

## Step 3: Create a DNS Policy

### 3.1 Navigate to DNS Policies

1. In the Zero Trust Dashboard, click **"Traffic policies"** in the left sidebar

2. Click **"Firewall policies"**

3. Click the **"DNS"** tab

### 3.2 Add a New Policy

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

## Step 4: Configure Browser to Use DoH

### 4.1 For Google Chrome

1. Open Chrome and go to:
   ```
   chrome://settings/security
   ```
   
   Or: Settings > Privacy and security > Security

2. Scroll down to find the **"Use secure DNS"** section

3. Enable it (toggle on)

4. Select **"With Custom"** or **"Customized"**

5. Paste the DoH URL you noted from Step 2:
   ```
   https://xxxxxx.cloudflare-gateway.com/dns-query
   ```

6. Press Enter or click outside the input field

### 4.2 For Microsoft Edge

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

### 4.3 For Mozilla Firefox

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

## Step 5: Test Blocking with Test Domains

### 5.1 Cloudflare Test Domains

Cloudflare provides special domains for testing category blocking without visiting actual malicious websites:

| Category | Test Domain |
|----------|-------------|
| Malware | `malware.testcategory.com` |
| Phishing | `phishing.testcategory.com` |
| Spam | `spam.testcategory.com` |
| Spyware | `spyware.testcategory.com` |
| Cryptomining | `cryptomining.testcategory.com` |
| New Domains | `newdomains.testcategory.com` |

### 5.2 Test Steps

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

### 5.3 How to Clear DNS Cache

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

## Step 6: View DNS Logs

### 6.1 Navigate to Logs

1. Return to the Zero Trust Dashboard: https://one.dash.cloudflare.com/

2. Click **"Insights"** in the left sidebar

3. Click **"Logs"**

4. Select the **"Gateway"** > **"DNS"** tab

### 6.2 Find Your Log Entry

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

### Problem: DNS blocking not working

**Possible causes:**
- DoH URL is incorrect
- DNS cache still has old values
- Policy is not yet active

**Solutions:**
1. Verify that the DoH URL was copied correctly
2. Clear DNS cache (see Step 5.3)
3. Wait 2-3 minutes for the policy to propagate
4. Verify that the policy status is "Active"

### Problem: Cannot see DNS Logs

**Possible causes:**
- Logs have not synced yet
- Filter is incorrect

**Solutions:**
1. Wait 2-5 minutes and refresh
2. Check the time range of the filter
3. Try clearing the filter and view all logs

---

## Next Step

**Ready?** Go to [Module 4: ZTNA with Browser Isolation](./04-ztna-rbi.md)
