# Module 2: Cloudflare Pages

Deploy a static website using Cloudflare Pages via the Dashboard.

**Time needed: 15 minutes**

---

## What You Will Do

1. Understand the file structure
2. Deploy via GitHub (recommended)
3. Or deploy via Direct Upload (alternative)

---

## Step 1: Prepare Files for Deployment

### 1.1 Understand the File Structure

This repository contains a sample site:

```
pages-site/
└── index.html    ← Single-page website file
```

The `index.html` file is a simple HTML page that displays a confirmation message when deployment is successful.

---

## Step 2: Deploy Pages via GitHub (Recommended)

### 2.1 Fork This Repository

1. Open GitHub and go to this repository:
   ```
   https://github.com/pongpisit/cloudflare-gui-workshop-lab
   ```

2. Click the **"Fork"** button (top right corner)

3. Select your account as the destination

4. Wait for the fork to complete

### 2.2 Create a Pages Project on Cloudflare

5. Open Cloudflare Dashboard: https://dash.cloudflare.com/

6. Go to **Workers & Pages** (left sidebar)

7. Click **"Create"** > Select **"Pages"**

8. Select **"Connect to Git"**

9. Click **"Connect GitHub"** (if not already connected)

10. Authorize Cloudflare to access your GitHub account

### 2.3 Select Repository and Configure Settings

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
2. You should see a web page with the message "Cloudflare Pages — Lab Site"

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

## Step 3: Deploy Pages via Direct Upload (Alternative)

**Use this method if:** You cannot use GitHub or want to upload files directly

### 3.1 Download the File

1. Download the `pages-site/index.html` file from this repository to your computer

### 3.2 Upload to Cloudflare

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

## Troubleshooting

### Problem: Pages deployment failed

**Possible causes:**
- Build output directory is incorrect
- No files in the specified directory

**Solutions:**
1. Verify that Build output directory is set to `pages-site`
2. Verify that the repository has a `pages-site/` folder with `index.html` inside

---

## Next Step

**Ready?** Go to [Module 3: DNS Security](./03-dns-security.md)
