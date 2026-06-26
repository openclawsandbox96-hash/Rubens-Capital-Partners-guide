# Working on this project

This repo is the **"Working with Claude — Rubens Capital Partners"** guide: a single,
self-contained `index.html` (inline CSS/JS, no build step, no dependencies). It is
published live via **GitHub Pages**.

- **Live site:** https://plainset.github.io/Rubens-Capital-Partners-guide/
- **Source of truth:** `index.html` on the `main` branch.

## The publish loop

`main` is the live branch — **every push to `main` publishes to the live site.**

1. **Edit** `index.html` locally.
2. **Review** the change — `git diff`.
3. **Commit** with a clear message.
4. **Push** — GitHub Pages auto-deploys.
5. **Wait ~1–2 min**, then **verify** at the live URL (hard-refresh: `⌘⇧R`).

```bash
cd "$HOME/Desktop/Code Projects/Claude guide website "   # note: the folder name ends with a space
git diff                          # review what changed
git add -A
git commit -m "Describe the change"
git push                          # → auto-deploys to GitHub Pages
```

**Roles:** Alex says what to change → Claude edits, reviews the diff, commits, and pushes →
we verify on the live site.

## Security & secrets — read before every commit

**This repository is PUBLIC.** Anything you commit is:

- world-readable and indexed by search engines, and
- kept in git history **forever** — deleting a file in a later commit does **not** remove it
  from history, and by then it may already be cached, cloned, or forked.

Treat every commit as a public release.

### Never commit

- **API keys / tokens** — Anthropic/Claude keys (`sk-ant-…`), AWS keys, GitHub tokens
  (`gho_…`, `ghp_…`), OAuth client secrets.
- **Credentials** — passwords, `.env` files, private keys (`*.pem`, `*.key`),
  service-account JSON.
- **Confidential RCP data** — real LP names, tenant data, portfolio/deal figures, or anything
  pulled from internal systems. Every example in the deck stays **illustrative**.

### Why a key must never go in the page

`index.html` is **client-side** — every byte is visible to anyone who opens the page or clicks
"View Source." A key placed in the HTML/JS is a *published* key. If a feature ever needs to call
an API with a secret, that call must run **server-side / in a backend**, never from this static
page.

### Guardrails in this repo

- `.gitignore` excludes `.env`, `*.key`, `*.pem`, secret files, and OS junk (`.DS_Store`) so
  they can't be staged by accident.
- Quick pre-push self-check: run `git diff --cached` and scan for anything that looks like a key,
  token, or real name/number before you push.

### If a secret ever gets committed

1. **Rotate / revoke it immediately** — assume it is compromised the moment it is pushed.
2. **Then** scrub it from history (`git filter-repo` or BFG) and force-push.

Order matters: **rotation first, history cleanup second.** A deleted-but-leaked key is still a
leaked key.

### Auth note

The GitHub token used by `gh` lives in the macOS Keychain — **not** in this repo. Never echo it,
paste it into a file, or commit it.
