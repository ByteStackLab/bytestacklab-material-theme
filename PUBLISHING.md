# ByteStackLab Material Theme — Publish Guide (Step by Step)

> **Extension:** ByteStackLab Material Theme (Dark + Ocean + Light)
> **Publisher ID:** `bytestacklab`
> **Microsoft Account:** mokammeltanvir@outlook.com
> **Last updated:** July 2026

এই doc টা follow করলে GitHub repo push থেকে শুরু করে Marketplace এ live হওয়া পর্যন্ত সব হয়ে যাবে।

---

## বর্তমান অবস্থা (কী কী Ready)

- ✅ ৩টা variant: Dark, Ocean, Light (Apache-2.0 licensed Material Theme v33.11.0 scheme)
- ✅ Icon (128x128 PNG, ByteStackLab branding)
- ✅ README, CHANGELOG, LICENSE, THIRD-PARTY-NOTICES
- ✅ `.vsix` build + local install test করা
- ⬜ Screenshots (optional — পরে patch release এও দেওয়া যায়)
- ⬜ GitHub repo push
- ⬜ Azure DevOps PAT + Marketplace publisher
- ⬜ Publish!

---

## Step 1: Screenshots (Optional কিন্তু Recommended)

Marketplace page এ theme এর preview দেখালে install অনেক বেশি হয়।

1. VS Code এ theme select করুন: `Cmd+K Cmd+T` → **ByteStackLab Material Dark**
2. নিজের একটা সুন্দর code file খুলুন (Laravel controller / Vue component ভালো দেখায়)
3. Screenshot নিন: `Cmd+Shift+4` → area select করুন
4. তিনটা variant এর জন্য repeat করে এই নামে `images/` folder এ রাখুন:
   - `images/screenshot-dark.png`
   - `images/screenshot-ocean.png`
   - `images/screenshot-light.png`
5. `README.md` খুলে screenshot এর `<!-- -->` comment গুলো খুলে দিন

Screenshot ছাড়া publish করলেও সমস্যা নেই — পরে add করে `vsce publish patch` দিলেই হবে।

---

## Step 2: GitHub Repo Push

`package.json` এ repository URL দেওয়া আছে: `github.com/bytestacklab/bytestacklab-material-theme`

### Option A: GitHub Organization (Recommended — URL match করবে)

1. https://github.com এ নিজের account এ login করুন
2. উপরে ডানে **+** → **New organization** → **Free** plan
3. Organization name: `bytestacklab`
4. Org বানানোর পর: **New repository** → name: `bytestacklab-material-theme`
   - **Public** রাখুন (marketplace এ README এর image দেখাতে হলে public লাগবে)
   - README/License কিছু add করবেন না (আমাদের already আছে)

### Option B: Personal Account এ Repo

Personal account (`MokammelTanvir`) এ repo বানালে `package.json` এর `repository.url` আর `bugs.url` টা বদলাতে হবে। Claude কে বললেই ঠিক করে দেবে।

### Push Commands

Repo বানানোর পর terminal এ:

```bash
cd ~/ByteStackMaterialTheme
git remote add origin https://github.com/bytestacklab/bytestacklab-material-theme.git
git push -u origin main
```

> GitHub credential চাইলে: username + Personal Access Token (GitHub এরটা, Azure এরটা না)। `gh auth login` করা থাকলে এমনিই হয়ে যাবে।

---

## Step 3: Azure DevOps Organization + PAT Token

### 3.1 Login ও Organization

1. https://dev.azure.com এ যান
2. **Sign in** → `mokammeltanvir@outlook.com` দিয়ে login
   - ⚠️ Browser এ অন্য Microsoft account logged in থাকলে খেয়াল রাখুন — পুরো setup **এই এক account** দিয়েই করতে হবে
3. প্রথমবার হলে organization create করতে বলবে — যেকোনো নাম দিন (যেমন `bytestacklab`), region default রাখুন
4. Project create করতে বললে যেকোনো নাম দিয়ে বানিয়ে ফেলুন (এটা matter করে না)

### 3.2 Personal Access Token (PAT)

1. উপরে ডানে **User settings icon** (মানুষ + gear চিহ্ন) → **Personal Access Tokens**
2. **+ New Token** চাপুন
3. Form:

| Field | Value |
|---|---|
| Name | `vsce-publish` |
| Organization | ⚠️ **All accessible organizations** (dropdown থেকে — এটা ভুল হলে publish fail!) |
| Expiration | Custom defined → ১ বছর পরের date |
| Scopes | **Custom defined** → নিচে **"Show all scopes"** চাপুন → **Marketplace** খুঁজে → ✅ **Manage** tick দিন |

4. **Create** চাপুন
5. ⚠️ **Token এখনই copy করুন** — এই একবারই দেখাবে! Password manager এ রাখুন।
6. Token টা **কাউকে দেবেন না, chat/message এ paste করবেন না** — এটা password এর মতো।

---

## Step 4: Marketplace Publisher Create

1. https://marketplace.visualstudio.com/manage এ যান
2. **Same account** (`mokammeltanvir@outlook.com`) দিয়ে login
3. **Create publisher** চাপুন
4. Form:

| Field | Value |
|---|---|
| ID | `bytestacklab` — ⚠️ **exactly এই spelling, সব lowercase। পরে বদলানো যায় না!** `package.json` এর `"publisher"` এর সাথে match করতেই হবে |
| Name | `ByteStackLab` (display name, পরে বদলানো যায়) |
| Logo | ByteStackLab logo (optional) |
| Website | `https://bytestacklab.com` (optional) |

5. **Create** → Publisher ready!

> **ভবিষ্যতে:** `bytestacklab@gmail.com` দিয়ে Microsoft account বানিয়ে এই publisher এ Member হিসেবে add করা যাবে (Manage page → Members) — brand ownership আলাদা করতে চাইলে।

---

## Step 5: Login ও Publish

নিজের terminal এ:

```bash
cd ~/ByteStackMaterialTheme

# Login — PAT চাইবe, copy করা token paste করে Enter
vsce login bytestacklab

# Publish!
vsce publish
```

Success হলে ৫-১০ মিনিটের মধ্যে live:

```
https://marketplace.visualstudio.com/items?itemName=bytestacklab.bytestacklab-material-theme
```

Status দেখতে: https://marketplace.visualstudio.com/manage (verification/malware scan এ কয়েক মিনিট লাগে)

### Publish হওয়ার পর Check করুন

1. Marketplace link খুলে দেখুন — icon, README, ৩টা variant ঠিক দেখাচ্ছে কিনা
2. VS Code এ Extensions panel → "ByteStackLab Material Theme" search করে দেখুন
3. Local `.vsix` version uninstall করে marketplace থেকে install করুন:
   ```bash
   code --uninstall-extension bytestacklab.bytestacklab-material-theme
   code --install-extension bytestacklab.bytestacklab-material-theme
   ```

---

## Step 6: Future Updates

Theme এ change করার পর:

```bash
# ছোট fix / color tweak / screenshot add (1.0.0 → 1.0.1)
vsce publish patch

# নতুন variant / বড় feature (1.0.0 → 1.1.0)
vsce publish minor
```

- এগুলো automatically version bump + publish করে
- `CHANGELOG.md` update করতে ভুলবেন না
- GitHub এও push করুন: `git push`
- Users রা automatically update পাবে

---

## Troubleshooting

| সমস্যা | সমাধান |
|---|---|
| `401/403 Unauthorized` publish এ | PAT এর Organization "All accessible organizations" ছিল না — নতুন PAT বানান |
| `Publisher not found` | Publisher ID আর `package.json` এর `publisher` match করছে না |
| Token expired | নতুন PAT বানিয়ে আবার `vsce login bytestacklab` |
| README এর image দেখাচ্ছে না | GitHub repo public আছে কিনা check করুন, push হয়েছে কিনা দেখুন |
| `vsce: command not found` | `npm install -g @vscode/vsce` আবার চালান |
| Marketplace এ theme আসছে না | ৫-১০ মিনিট অপেক্ষা করুন, manage page এ status দেখুন |

---

*ByteStackLab · July 2026*
