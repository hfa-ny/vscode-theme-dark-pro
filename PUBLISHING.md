# Publishing Eloquent Dark Pro to the VS Code Marketplace

A complete, do-this-then-this guide for getting **Eloquent Dark Pro** live on the [Visual Studio Marketplace](https://marketplace.visualstudio.com/vscode).

> All commands below assume your working directory is `eloquent-dark-pro/` unless noted.

---

## 0. Prerequisites

- **Node.js** 18+ installed (`node -v`)
- A **Microsoft account** (any personal or work account works)
- The VS Code Extension Manager CLI:
  ```powershell
  npm install -g @vscode/vsce
  ```

---

## 1. Add the required icon

The Marketplace requires a **128×128 PNG icon** at `eloquent-dark-pro/images/icon.png` (already referenced in `package.json`).

Create the folder and drop in your icon:

```powershell
mkdir eloquent-dark-pro\images
# copy your 128x128 icon.png into eloquent-dark-pro\images\icon.png
```

> If you don't have one yet, you can temporarily remove the `"icon"` line from `package.json` to publish without it — but a custom icon dramatically improves install rates.

---

## 2. Create an Azure DevOps organization (one-time)

The Marketplace authenticates via **Azure DevOps Personal Access Tokens (PAT)**.

1. Go to <https://dev.azure.com/> and sign in with your Microsoft account
2. If prompted, create a new organization (any name — it's only used for the PAT)

---

## 3. Create a Personal Access Token (PAT)

1. In Azure DevOps, click your avatar (top-right) → **Personal access tokens**
2. Click **+ New Token** and configure:
   - **Name:** `vsce-publish`
   - **Organization:** **All accessible organizations** ← important
   - **Expiration:** 90 days (or your preference)
   - **Scopes:** click **Show all scopes**, then under **Marketplace** check **Manage**
3. Click **Create** and **copy the token immediately** — you cannot view it again

---

## 4. Create your Publisher

A publisher is the identity that owns your extension. Your `package.json` is set to `"publisher": "eloquentsoftware"` — change this if you want a different one.

1. Go to <https://marketplace.visualstudio.com/manage>
2. Click **Create publisher**
3. Fill in:
   - **ID:** `eloquentsoftware` (must exactly match `package.json` → `publisher`)
   - **Name:** `Eloquent Software` (display name)
   - **Email / website:** as desired
4. Click **Create**

---

## 5. Log vsce in to your publisher

```powershell
cd eloquent-dark-pro
vsce login eloquentsoftware
# paste the PAT from step 3 when prompted
```

You should see: `The Personal Access Token verification succeeded for the publisher 'eloquentsoftware'.`

---

## 6. Package and inspect locally (recommended)

Build a `.vsix` you can install in VS Code as a smoke test:

```powershell
vsce package
```

This produces `eloquent-dark-pro-1.0.0.vsix`. Test it locally:

```powershell
code --install-extension .\eloquent-dark-pro-1.0.0.vsix
```

Open VS Code → **Preferences: Color Theme** → confirm **Eloquent Dark Pro** loads correctly.

---

## 7. Publish

```powershell
vsce publish
```

That's it. Within a few minutes the extension appears at:

```
https://marketplace.visualstudio.com/items?itemName=eloquentsoftware.eloquent-dark-pro
```

### Bumping versions

Don't edit the version manually — let `vsce` do it (it commits and tags too if you're in a git repo):

```powershell
vsce publish patch    # 1.0.0 -> 1.0.1
vsce publish minor    # 1.0.0 -> 1.1.0
vsce publish major    # 1.0.0 -> 2.0.0
```

---

## 8. (Optional) Publish to Open VSX

[Open VSX](https://open-vsx.org/) is the registry used by VSCodium, Cursor, Gitpod, and others. Highly recommended.

1. Sign in at <https://open-vsx.org/> with GitHub
2. Generate an access token at <https://open-vsx.org/user-settings/tokens>
3. Install the publishing CLI and publish:
   ```powershell
   npm install -g ovsx
   ovsx publish eloquent-dark-pro-1.0.0.vsix -p <YOUR_OPENVSX_TOKEN>
   ```

---

## 9. After it's live

- **Add the Marketplace badge** to README:
  ```markdown
  [![Version](https://img.shields.io/visual-studio-marketplace/v/eloquentsoftware.eloquent-dark-pro)](https://marketplace.visualstudio.com/items?itemName=eloquentsoftware.eloquent-dark-pro)
  [![Installs](https://img.shields.io/visual-studio-marketplace/i/eloquentsoftware.eloquent-dark-pro)](https://marketplace.visualstudio.com/items?itemName=eloquentsoftware.eloquent-dark-pro)
  ```
- **Update the promo site** — the `marketplace.visualstudio.com/...` link in `website/index.html` will start working automatically.
- **Tag a GitHub release** matching the published version.

---

## Troubleshooting

| Problem | Fix |
| --- | --- |
| `ERROR Missing publisher name` | Add `"publisher"` to `package.json` and re-run. |
| `ERROR The Personal Access Token verification has failed` | PAT must have **Marketplace → Manage** scope and **All accessible organizations**. |
| `ERROR Make sure to edit the README.md file before you publish your extension.` | Replace the boilerplate "Working with Markdown" text — already done in this repo. |
| Images don't render on the Marketplace page | Use **absolute** URLs (e.g. `https://raw.githubusercontent.com/...`) — relative paths often break. |
| `ERROR icon ... does not exist` | Either add `images/icon.png` or remove the `"icon"` field from `package.json`. |
| Want to unpublish a bad version? | `vsce unpublish eloquentsoftware.eloquent-dark-pro@1.0.0` |

---

## Quick reference

```powershell
# one-time
npm install -g @vscode/vsce
vsce login eloquentsoftware

# every release
vsce package          # build .vsix locally
vsce publish patch    # bump + publish
```
