# o2cloud_login_aurora — Aurora 2026 ✨

A **TailwindCSS-powered** login skin for RoundCube Webmail, built around the 2025-2026 "grainy gradient" trend: animated violet/fuchsia/orange halos, a subtle grain texture, a light glass card and bold Space Grotesk typography — only the login page changes, the rest of your webmail keeps the standard **Elastic** look and feel.

![Démo](https://img.shields.io/badge/Démo-en%20ligne-d946ef?style=for-the-badge) ← [Voir la démo](https://o2cloud.fr/demos/demo-aurora-2026.html)

## ✨ Features

- **TailwindCSS** loaded via CDN — all visual styling is driven by utility classes, no build pipeline required
- **3 animated gradient halos** (fuchsia / violet / orange) floating slowly behind a light glass card
- **Grain texture overlay** : subtle SVG-based noise, on-trend "grainy gradient" look
- **Space Grotesk** display typography for the heading, Inter for body text
- **Gradient button** with hover scale + shadow glow
- **Show/hide password** toggle
- **Chrome autofill fix**
- **Fully responsive**
- **Zero impact on the rest of the webmail** : only the login page is touched

## 🚀 Installation

### Prerequisites

- RoundCube Webmail ≥ 1.6 (skin inheritance via `meta.json` `extends` requires this)
- The **Elastic** skin must remain installed (this skin extends it, it does not replace it)
- Outbound HTTPS access from the server (Google Fonts, FontAwesome, and the **Tailwind CDN script** itself)

### Quick install

```bash
# 1. Extract the skin into RoundCube's skins/ directory
unzip o2cloud_login_aurora.zip -d /var/www/roundcube/skins/

# 2. Fix ownership/permissions
chown -R www-data:www-data /var/www/roundcube/skins/o2cloud_login_aurora

# 3. Activate it in config.inc.php
echo "\$config['skin'] = 'o2cloud_login_aurora';" >> /var/www/roundcube/config/config.inc.php

# 4. Clear RoundCube's compiled-template cache
rm -rf /var/www/roundcube/temp/*
```

Hard-refresh your browser (Ctrl+Shift+R) and you're done.

---

## ⚙️ Customization

Colors and animation are TailwindCSS utility classes, set directly in `scripts/login.js` — any Tailwind color works:

```js
// Background halos
'bg-fuchsia-500/40'   // halo 1
'bg-violet-600/40'    // halo 2
'bg-orange-400/30'    // halo 3

// Button
'from-fuchsia-600 via-violet-600 to-orange-500'
```

Halo float speed/amplitude is defined as custom keyframes in `templates/login.html`, inside the `tailwind.config` block:

```js
keyframes: {
    'float-a': { '0%,100%': { transform: 'translate(0,0) scale(1)' }, '50%': { transform: 'translate(40px,60px) scale(1.08)' } },
    // ...
},
animation: {
    'float-a': 'float-a 22s ease-in-out infinite',
    // ...
}
```

Welcome text is set in `scripts/login.js`, function `init()`.

---

## 📁 File structure

| Path | Description |
|---|---|
| `meta.json` | Skin declaration — `"extends": "elastic"` |
| `templates/login.html` | Login page template — loads Tailwind CDN + config, our CSS/JS |
| `styles/login.css` | Only what Tailwind can't do natively: structural fixes, grain texture, halo keyframes, autofill fix |
| `scripts/login.js` | Builds the visual layer (halos, card, fields, button) using Tailwind utility classes — never touches auth logic |

No other file is provided or modified: every other page of RoundCube keeps rendering through the standard Elastic skin.

---

## 🔍 Troubleshooting

**Page doesn't change after install**
```bash
grep "skin" config/config.inc.php   # must show $config['skin'] = 'o2cloud_login_aurora';
rm -rf temp/*
```
Then hard-refresh (Ctrl+Shift+R).

**No styling at all, card looks like plain unstyled HTML**
The Tailwind CDN script (`cdn.tailwindcss.com`) couldn't load — check outbound internet access from the server, or browser console for a blocked-request error. For air-gapped/high-security environments, replace the CDN `<script>` with a locally built Tailwind CSS bundle.

**Halos don't animate**
The custom keyframes are registered in the inline `tailwind.config` script in `templates/login.html`. If you remove or reorder that block, the `animate-float-a/b/c` classes used in `scripts/login.js` will have no matching animation.

**CSS/JS return 404**
Don't prefix asset paths with the skin folder name — RoundCube already resolves root-relative paths against the active skin folder.

---

## 🛠️ Technical details

- **Skin inheritance** : declared via `"extends": "elastic"` in `meta.json`. Only `templates/login.html` is overridden — everything else is inherited from Elastic.
- **Hybrid CSS strategy** : Tailwind utility classes (injected at runtime by `scripts/login.js`) drive the actual visual design of the card/fields/button/halos. Plain CSS in `styles/login.css` only handles what Tailwind can't express cleanly: killing leftover Bootstrap/Elastic rules (`!important` overrides), the grain texture (SVG data-URI), and the halo keyframes.
- **Field selection** : by `name` attribute rather than `id`, for robustness across RoundCube versions/configs.
- **No authentication logic is touched** : the real `<input>` elements stay in the form and submit exactly as before.

---

## ⚠️ Security notes

- This skin contains no credentials and no server-side code — pure front-end (CSS/JS) layered on top of RoundCube's own login form.
- It depends on **three** third-party CDNs: Google Fonts, FontAwesome (cdnjs), and the Tailwind Play CDN script. The Tailwind CDN script in particular is meant for prototyping — for a hardened/offline production setup, compile a proper Tailwind build instead of using the CDN script.
- Always review third-party skins before deploying to production.

---

## 📄 Changelog

| Version | Date | Changes |
|---|---|---|
| v1.0.0 | June 2026 | Initial release — TailwindCSS via CDN, animated gradient halos, grain texture, Space Grotesk typography, Chrome autofill fix |

---

## 🤝 Contributing

1. Fork the project
2. Create a feature branch (`git checkout -b feature/my-feature`)
3. Commit your changes (`git commit -m 'Add my feature'`)
4. Push to the branch (`git push origin feature/my-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## 👤 Author

**o2Cloud**
- GitHub: [@o2Cloud-fr](https://github.com/o2Cloud-fr)
- Email: github@o2cloud.fr

---

[![RoundCube](https://img.shields.io/badge/RoundCube-%E2%89%A5%201.6-1a82e2?style=for-the-badge)](https://roundcube.net)
[![Elastic skin required](https://img.shields.io/badge/Requires-Elastic%20skin-d946ef?style=for-the-badge)](https://github.com/roundcube/roundcubemail)
[![TailwindCSS](https://img.shields.io/badge/TailwindCSS-CDN-38BDF8?style=for-the-badge&logo=tailwindcss&logoColor=white)](https://tailwindcss.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

---

⭐ **If this skin helps you, feel free to give it a star!**

> **Note**: This skin only replaces the RoundCube login page. It does not modify authentication logic, account data, or any other part of the webmail.
