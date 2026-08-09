# PM OS Marketing Site

Marketing and documentation site for the PM OS plugin, deployed to **[pm-os.quick.mozilla.cloud](https://pm-os.quick.mozilla.cloud/)**.

## Purpose

This site is the public-facing documentation and marketing page for the PM OS plugin. It includes:
- Overview of what PM OS is and how it works
- Full skills catalog (50+ skills organized by JTBD)
- Installation guide for Firefox PMs
- Release notes for each version
- FAQ

The site is **version controlled** to prevent accidental overwrites. All changes must go through git commits.

## Making Changes

**IMPORTANT: Never deploy directly to Quick without committing to git first.** This repo is the source of truth.

### Workflow

1. **Edit `index.html`** - All site content is in this single file
2. **Test locally** - `open index.html` to preview changes
3. **Commit and push:**
   ```bash
   git add index.html
   git commit -m "Description of changes"
   git push
   ```
4. **Deploy:**
   - **Manual (works now):** `quick deploy . pm-os -y`
   - **Auto (after allow-listing):** Push to main triggers GitHub Actions deploy

### Never Do This

❌ **DO NOT** deploy to Quick without committing to git first  
❌ **DO NOT** overwrite the site from a different directory  
❌ **DO NOT** deploy from scratchpad or temp directories  
❌ **DO NOT** make changes directly on Quick without git

The July 28 → Aug 5 incident happened because another agent deployed from a temp directory, overwriting the version-controlled site. Always work from this repo.

## Deployment Details

**Current state:** Manual deploy via `quick deploy . pm-os -y`

**Future state (pending):** Auto-deploy via GitHub Actions on push to main. Blocked on infrastructure allow-listing - repo needs to be added to `gha_repos` in Quick's `tf/prod/main.tf`.

**GitHub Actions workflow:** `.github/workflows/workflows-quick.deploy.yml` is configured but fails with permission error until allow-listed.

## Site Structure

```
pm-os-marketing-site/
├── index.html              # The entire site (single-page app)
├── CLAUDE.md               # Router to this file
├── AGENTS.md               # This file - context for working with the site
├── README.md               # Human-facing documentation
└── .github/workflows/
    └── workflows-quick.deploy.yml  # Auto-deploy (pending allow-listing)
```

**Note:** No favicon.png yet - needs to be added from the original colorful PM OS logo.

## Permissions

- **Team `pm-os-maintainers`** has maintain access (can push to main, manage settings)
- **No branch protection** - team members can push directly to main for quick updates
- **Public repo** - anyone can read, but only team can write

## Adding Release Notes

When a new PM OS version ships:

1. Read the current Release Notes tab in `index.html` (search for `id="whats-new-tab"`)
2. Add a new `<div class="step">` section at the top with the new version
3. Follow the existing format (see v0.3.0 and v0.2.6 as examples)
4. Commit, push, deploy

## Quick Module Reference

This site is deployed via Mozilla's Quick static hosting. For Quick CLI details, see:
- Plugin context: `$CLAUDE_PLUGIN_ROOT/modules/quick/context.md` (if pm-os-base plugin is installed)
- Quick commands: `quick --help`
- Site management: `quick list`, `quick url pm-os`, `quick open pm-os`

## Version History

- **2026-08-08:** Site recovered from July 28 artifact after being overwritten; moved to version control in this repo
- **2026-07-28:** Original site built with Mozilla purple theme, dark mode, full navigation
- **Earlier:** Site was deployed from temp directories without version control (caused the overwrite incident)

## Recovery Process (In Case of Emergency)

If the site gets overwritten again:

1. **Check this repo first** - the source HTML is version controlled here
2. **Deploy from main:** `cd ~/projects/work/mozilla/pm-os-marketing-site && quick deploy . pm-os -y`
3. **If this repo is corrupted:** Check the git history or GitHub for previous versions
4. **Last resort:** PM OS site artifacts may exist in Claude Code session transcripts - search for artifact URLs

The July 28 version was recovered from artifact `https://claude.ai/code/artifact/d89f45e4-f15c-4e63-8ae4-43135f3e2f0d`.
