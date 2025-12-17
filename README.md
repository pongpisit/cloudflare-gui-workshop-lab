# Cloudflare GUI Lab: Workers/Pages + DNS Security (Browser DoH)

This lab is designed for a **4-hour** hands-on session and follows a **GUI-only** approach:

- Workers: create, edit, deploy, variables, logs — **Cloudflare Dashboard only**
- Pages: deploy a static site — **Cloudflare Dashboard only**
- Zero Trust: **DNS Security only**, tested via **Browser Secure DNS (DoH)** (no WARP, no SWG, no AV)

> Video recording is allowed. Do not record or screenshot secret values.

---

## Prerequisites

- A Cloudflare account (free is OK)
- A web browser (Chrome / Edge / Firefox recommended)
- Stable internet access to:
  - https://dash.cloudflare.com/
  - https://one.dash.cloudflare.com/

Optional (recommended for the Pages lab via GitHub integration):

- A GitHub account

---

## Time plan (4 hours)

- 0:00–0:15 Overview + access check
- 0:15–1:30 Workers (Hello World, edit, deploy, variables, logs)
- 1:30–2:00 Pages (deploy static HTML)
- 2:00–2:10 Break
- 2:10–3:50 Zero Trust DNS Security via Browser DoH (policies + test domains + logs)
- 3:50–4:00 Q&A + cleanup

---

## Part A — Workers (GUI-only)

### A1) Navigate to Workers & Pages

1. Open Cloudflare Dashboard: https://dash.cloudflare.com/
2. In the left navigation, open **Workers & Pages**.

Verify:

- You can see entry points for **Workers** and **Pages**.

---

### A2) Create and deploy a new Worker (Hello World)

1. Go to **Workers & Pages**
2. Create a **new Worker** (use the default Hello World template)
3. Deploy

Verify:

- The Worker has a public `*.workers.dev` URL.
- Visiting the URL returns a response.

Rollback:

- Delete the Worker if it was created only for the lab.

---

### A3) Edit the Worker to return JSON (copy/paste)

In the Worker editor, replace the code with the snippet below and deploy.

```js
export default {
  async fetch(request, env) {
    const url = new URL(request.url);

    if (url.pathname === '/api/hello') {
      return Response.json({
        ok: true,
        message: 'Hello from Cloudflare Workers (GUI deploy)!',
        environment: env.ENVIRONMENT ?? 'not-set',
      });
    }

    return new Response('Worker is running. Try /api/hello', {
      headers: { 'content-type': 'text/plain; charset=utf-8' },
    });
  },
};
```

Verify:

- Visiting `/api/hello` returns JSON.

Rollback:

- Revert code in the editor to the previous saved version.

---

### A4) Add a non-secret variable (GUI)

1. Open your Worker settings in the dashboard.
2. Add a **variable**:
   - Key: `ENVIRONMENT`
   - Value: `demo`
3. Save and redeploy if required.

Verify:

- `/api/hello` now returns `environment: "demo"`.

Rollback:

- Remove the variable and redeploy if required.

---

### A5) View Logs / Observability (GUI)

1. Open the Worker.
2. Go to **Logs / Observability**.
3. Trigger a few requests:
   - `/`
   - `/api/hello`

Verify:

- Logs show recent requests.

---

## Part B — Pages (GUI-only)

This lab includes a tiny static site at:

- `pages-site/index.html`

You can deploy it in one of two ways.

### Option 1 (recommended): Deploy Pages from GitHub

1. Fork this repository to your GitHub account.
2. In Cloudflare Dashboard, go to **Workers & Pages** > **Pages**.
3. Create a new Pages project and connect GitHub.
4. Select your forked repo.
5. Configure build settings:
   - Framework preset: **None** (or “Static HTML” if shown)
   - Build command: *(leave empty)*
   - Build output directory: `pages-site`
6. Deploy.

Verify:

- You get a `*.pages.dev` URL.
- The page loads and shows the demo content.

Rollback:

- Delete the Pages project if it was demo-only.

### Option 2: Deploy Pages by direct upload

If your environment does not allow GitHub integration:

1. Download the `index.html` file locally.
2. Create a Pages project and use direct upload.
3. Upload `index.html`.

Verify:

- The site loads successfully.

---

## Part C — Zero Trust (DNS Security only) via Browser Secure DNS (DoH)

### C1) Create/enter a Zero Trust organization

1. Open: https://one.dash.cloudflare.com/
2. If prompted, create a Zero Trust organization (Free plan is fine).

Verify:

- You can access the Zero Trust dashboard.

---

### C2) Create a DNS Location (to get the DoH endpoint)

1. Go to **Networks > Resolvers & Proxies > DNS Locations**
2. Add a location (example name: `Workshop-Browser-DoH`)
3. Note the **DoH endpoint** (format):

```
https://xxxxx.cloudflare-gateway.com/dns-query
```

Verify:

- You can see the DNS location and its DoH URL.

Rollback:

- Delete the DNS location if it was created only for the lab.

---

### C3) Create a DNS policy to block a security category

1. Go to **Traffic policies > Firewall policies**
2. Select the **DNS** tab
3. Add a policy:
   - Name: `Block Malware (Demo)`
   - Selector: **Security Categories**
   - Operator: `in`
   - Value: `Malware` (or `All security risks` if you want broader coverage)
   - Action: `Block`
4. Save.

Verify:

- Policy appears in the DNS policy list.

Rollback:

- Disable/delete the policy.

---

### C4) Configure Browser Secure DNS (DoH)

Configure your browser to use the DoH endpoint from **C2**.

#### Chrome / Edge

1. Settings
2. Privacy and security
3. Security
4. Use Secure DNS
5. Choose a service provider: **Custom**
6. Paste:

- `https://xxxxx.cloudflare-gateway.com/dns-query`

#### Firefox

1. Settings
2. Privacy & Security
3. DNS over HTTPS
4. Increased Protection / Max Protection (depending on UI)
5. Add custom provider URL:

- `https://xxxxx.cloudflare-gateway.com/dns-query`

Verify:

- Browser Secure DNS is enabled and points to your Gateway DoH endpoint.

Rollback:

- Turn off Secure DNS / remove custom provider.

---

### C5) Test blocking with safe test domains (recommended)

Cloudflare provides safe category test domains. After enabling your DNS policy:

- Open in your browser:

```
malware.testcategory.com
```

Expected:

- You should see a block page (or the request fails) if the Malware category is blocked.

More common test domains:

- `phishing.testcategory.com`
- `spam.testcategory.com`
- `spyware.testcategory.com`
- `cryptomining.testcategory.com`
- `newdomains.testcategory.com`

Reference (Cloudflare docs):

- https://developers.cloudflare.com/cloudflare-one/policies/gateway/dns-policies/test-dns-filtering/

Tip:

- If you recently visited the domain, you may need to clear browser/OS DNS cache before retesting.

---

### C6) Verify with DNS Logs (GUI)

1. Go to **Insights > Logs (DNS tab)**
2. Filter for your test domain (example: `malware.testcategory.com`)

Verify:

- You see the query and the action (blocked/allowed) and the matched policy.

---

## License

MIT
