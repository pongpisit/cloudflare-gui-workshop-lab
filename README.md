# Cloudflare Workshop Lab

Learn to deploy **Cloudflare Pages**, configure **DNS Security**, and set up **ZTNA with Browser Isolation**.

---

## What You Will Learn

| Module | Topic |
|--------|-------|
| 1 | Set up your Cloudflare account |
| 2 | Deploy a static website with Pages |
| 3 | Configure DNS Security with Browser DoH |
| 4 | Set up ZTNA with Remote Browser Isolation |

---

## Workshop Modules

| # | Module | Time | Description |
|---|--------|------|-------------|
| 1 | [Prerequisites](./docs/01-prerequisites.md) | 10 min | Create account, verify access |
| 2 | [Cloudflare Pages](./docs/02-cloudflare-pages.md) | 15 min | Deploy static website via GitHub or Direct Upload |
| 3 | [DNS Security](./docs/03-dns-security.md) | 30 min | Create DNS policies, configure Browser DoH, test blocking |
| 4 | [ZTNA + RBI](./docs/04-ztna-rbi.md) | 45 min | Publish internal apps with agentless access |

**Total Time: ~1.5 hours**

---

## What You Need

- Web browser (Chrome, Edge, or Firefox)
- Internet connection
- Free Cloudflare account
- GitHub account (recommended)

---

## Quick Start

1. Go to [Module 1: Prerequisites](./docs/01-prerequisites.md)
2. Follow each step exactly
3. No CLI required — everything is done via Dashboard!

---

## Repository Structure

```
cloudflare-gui-workshop-lab/
├── README.md              ← You are here
├── docs/
│   ├── 01-prerequisites.md
│   ├── 02-cloudflare-pages.md
│   ├── 03-dns-security.md
│   └── 04-ztna-rbi.md
└── pages-site/
    └── index.html         ← Sample site for Pages deployment
```

---

## Additional Resources

- [Cloudflare Pages Documentation](https://developers.cloudflare.com/pages/)
- [Cloudflare Zero Trust Documentation](https://developers.cloudflare.com/cloudflare-one/)
- [Cloudflare Access Documentation](https://developers.cloudflare.com/cloudflare-one/policies/access/)
- [Browser Isolation Documentation](https://developers.cloudflare.com/cloudflare-one/policies/browser-isolation/)

---

**Ready?** [Start Module 1](./docs/01-prerequisites.md)
