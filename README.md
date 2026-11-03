# Email Signature Generator

A single-file, no-build web page that makes an email signature styled like a terminal "fetch" tool (think `neofetch`): ASCII logo on the left, `Label: value` rows on the right, and a color ramp along the bottom.

You edit a `signature.conf`-style form and the preview updates as you type. When you're done, you export it.

## Usage

1. Open `signature-generator.html` in any modern browser (double-click it, or host it on GitHub Pages).
2. Fill in your name, title, org and email. Use the checkboxes to turn optional rows (phone, website, location, certs, os, shell) on or off.
3. Pick an accent color and an ASCII logo.
4. Export:
   - **Copy signature HTML**: an inline-styled table you can paste into the Gmail or Outlook (web) signature editor.
   - **Copy plain text**: for plain-text clients.
   - **Download PNG**: a 2× image. Use this when your client strips fonts and colors (Outlook desktop is the usual culprit).

Nothing leaves your browser. There's no server, tracking or storage.

## Adapting it

Everything you'd want to change is near the top of the `<script>` block in `signature-generator.html`.

### Use your organization's brand colors
Edit the `brand` entries in `ACCENTS`:

```js
brand_primary: { name:'Brand Primary', hex:'#3B82F6', group:'brand',
                 ramp:['#0b1e3d','#163a75','#2257ad','#2f6fe0','#3b82f6','#a9c8fb'] },
```

`hex` is the accent color. `ramp` is the six-swatch strip under the preview (dark → light). Rename, add or remove entries freely. To start with your brand color selected, set `accent:` in the `state` line to its key.

### Pre-fill fields for a team
Set `default:` values in `FIELD_DEFS` (e.g. `org`, `website`) so coworkers only need to type their name, title and email. Then host the file somewhere shared.

### Add or remove fields
Add an object to `FIELD_DEFS`:

```js
{ key:'pronouns', label:'pronouns', placeholder:'they/them', default:'',
  optional:true, defaultShow:false, group:'identity (optional)' },
```

Optional fields show up in the preview automatically. Required (non-optional) rows are listed explicitly in `buildModel()`, so add them there too.

### Add your own ASCII logo
Add an array of strings to `LOGOS` and a matching label to `LOGO_LABELS`. Escape backslashes (`\\`). Keep it under ~10 lines so the signature stays compact.

### Change the look
- Window chrome text (`zsh — ~/dotfiles/signature.conf`) appears in the HTML (`.tpath`) and in `downloadPng()`.
- Page colors are CSS variables in `:root`.
- Fonts: JetBrains Mono for the preview and PNG. The email HTML falls back to `Consolas, Monaco, 'Courier New'` because most clients won't load web fonts.

### Other ideas
- Drop the terminal theme: rewrite `generateEmailHtml()` for a conventional layout and keep the editor/export plumbing.
- Add an `<img>` logo cell (hosted URL) in place of ASCII art for a more corporate signature.
- Make it a team tool: publish via GitHub Pages and add a URL-parameter prefill (`?org=...&website=...`).

## Email client notes
- **Gmail / Outlook web**: paste the HTML copy straight into the signature settings.
- **Outlook desktop / Apple Mail**: results vary. The PNG is the reliable fallback. Link it to your website if you want it clickable.
- Dark backgrounds can look heavy in light-mode inboxes. Test by sending yourself an email.

## License
MIT. Use it, fork it, rebrand it.
