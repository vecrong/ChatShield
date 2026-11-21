# ChatShield for FM-DX Webserver (v1.4.2+)

**ChatShield** is a modular chat security, anti-spam, and network protection plugin designed for **FM-DX Webserver v1.4.2+**. It provides real-time WebSocket message filtering, network-level ASN/VPN verification, and an integrated Material 3 administrative interface.

## Core Capabilities

### Text Normalization & Content Filtering
* **Unicode & Homoglyph Defense:** Neutralizes pseudo-Latin glyphs, mixed Latin/Cyrillic homoglyphs, zero-width/invisible characters, and Zalgo diacritics prior to rule evaluation.
* **Leetspeak & Stretch Folding:** Normalizes character substitutions (`@` $\rightarrow$ `a`, `3` $\rightarrow$ `e`, `0` $\rightarrow$ `o`, `$` $\rightarrow$ `s`, etc.) and repeated characters to prevent filter evasion.
* **Formatting Controls:** Enforces configurable thresholds for uppercase text (CAPS), character repetition, emoji density, line breaks, and ASCII art.
* **Link & PII Protection:** Blocks external URLs (with domain whitelisting), social media invites/handles, IPv4 addresses, phone/card numbers, and emails.

### Anti-Flood & Raid Mitigation
* **Rate Limiting:** Dual-window flood and burst protection paired with an optional per-user slow mode.
* **Duplicate & Coordinated Spam Detection:** Blocks exact repeats, fuzzy variations via Levenshtein distance, and multi-IP spam bursts.
* **Raid & Quarantine Controls:** Supports immediate anti-raid lockdown and configurable newcomer chat quarantine periods.

### Network & Access Security
* **ASN & VPN Verification:** Evaluates client IPs against multiple lookup providers (`ipwho.is`, `ip-api.com`, `freeipapi.com`) with 24-hour caching and a client-side fallback probe to block datacenter, hosting, and VPN traffic.
* **Header Integrity & Bot Filtering:** Validates `X-Real-IP` / `X-Forwarded-For` headers strictly from trusted local reverse proxies, rejects spoofed IP headers, and drops headless/automated user-agents.
* **CIDR Access Control:** Supports IPv4/IPv6 addresses and CIDR subnets for allowlisting and instant blocking.

### Moderation & Administration
* **Hybrid Ruleset:** Combines an automatically synced remote dictionary with local custom rules (`*` prefix wildcards, `$` end-of-phrase anchors, and `!` exceptions) and a built-in rule tester.
* **Enforcement & Penalties:** Configurable **Block** (with client input draft restoration) or **Mask (`***`)** modes, nickname protection, progressive auto-muting, and shadow muting.
* **Audit Logging:** Records blocked events with one-click moderation actions (Mute, Shadow Mute, IP Ban).

## Installation

1. **Deploy Files:** Place the plugin files into the FM-DX Webserver `plugins/` directory:
   ```text
   plugins/
   ├── chatshield.js
   └── ChatShield/
       ├── chatshield.js
       ├── chatshield_server.js
       ├── engine.js
       └── hook.js
   ```
2. **Enable Plugin:** Activate **ChatShield** in the FM-DX Webserver Admin Panel under **Plugins** and restart the server.
3. **Apply Core Hook:** Open the **ChatShield** tab in the Admin Panel and click **Install Hook** to patch `server/chat.js` (an automatic backup is saved as `server/chat.js.before-chatshield`), then restart the server.

## Configuration Storage

Runtime configurations, custom dictionaries, and audit logs are persisted automatically in `plugins_configs/`:
* `chatshield.json` — Plugin settings, IP lists, and active mutes.
* `chatshield-custom-words.json` — Local custom rules and exceptions.
* `chatshield-remote-cache.json` — Cached remote ruleset.
* `chatshield-log.json` — Security event audit log.
