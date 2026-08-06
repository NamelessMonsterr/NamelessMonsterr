# Setup

1. Create (or open) the special repo `NamelessMonsterr/NamelessMonsterr` — GitHub renders its README on your profile page automatically.
2. Copy everything in this folder into that repo's root:
   - `README.md`
   - `assets/` (all `.svg` files)
   - `.github/workflows/snake.yml`
3. Commit to `main` and push.
4. The `snake.yml` workflow needs an `output` branch to publish to. Either let the first workflow run create it, or run:
   ```bash
   git checkout --orphan output
   git rm -rf .
   git commit --allow-empty -m "init output branch"
   git push origin output
   ```
5. Go to **Settings → Actions → General** and confirm "Read and write permissions" is enabled for the `GITHUB_TOKEN`, or the snake job can't push.
6. Run the workflow once manually from the **Actions** tab (`Generate Contribution Snake` → `Run workflow`) so the snake SVGs exist immediately instead of waiting for the nightly cron.

# Why SVGs animate on GitHub but inline `<style>`/`<script>` don't

GitHub's markdown sanitizer strips `<style>` and `<script>` tags that are written directly in `README.md`. It does **not** sanitize the *contents* of a separate `.svg` file — when the README references it with `<img src="assets/hero.svg">`, the browser fetches and renders that file as a raw image resource, so its internal `<animate>`/SMIL tags run natively. That's why every animated element here lives in `/assets/*.svg` rather than inline in the README.

# Editing

- Colors are defined per-file as SVG gradients/fills (`#00F5FF`, `#8B5CF6`, `#00FF99`, `#0D1117`) — find/replace across `assets/*.svg` to re-theme.
- GitHub's Markdown sanitizer strips `style="..."` on arbitrary HTML tags, not just `<style>` blocks — don't add inline CSS to a `<div>`/`<span>` expecting it to render. Presentational attributes on `<table>`/`<td>` (`width`, `valign`, `align`) survive and are used throughout; everything else visual (cards, section labels, panels) is a real `.svg` file referenced via `<img>`, which is why the project cards live in `assets/projects/*.svg` rather than as styled `<div>`s — add a new card by copying one of those SVGs.
- Section headings are `assets/labels/label-*.svg`, each paired with an `<a id="slug"></a>` anchor and an entry in the quick-nav bar at the top of `README.md` — see "Navigation" in `docs/asset-guide.md` before adding or renaming a section.
- Swap `NamelessMonsterr` for a different GitHub username throughout `README.md` if this ever moves to another account.
