# QuickZTNA User Guide — user.quickztna.com

Manual testing guide for QuickZTNA. Built with Astro + MDX. Deployed to Cloudflare Pages.

## Local Development

```bash
npm install
npm run dev
# Open http://localhost:4321
```

## Build

```bash
npm run build
# Output: dist/
# Also runs Pagefind indexing automatically
```

## Adding New Chapters

1. Create a new MDX file in `src/content/chapters/`:

```bash
# Example:
src/content/chapters/acl-basics.mdx
```

2. Required frontmatter:
```mdx
---
title: "ACL Basics"
topic: "Access Control"
order: 6
machines: ["Win-A", "Win-B", "Linux-C"]
summary: "One-line description shown on the home page."
---
```

3. `order` controls sort order within the topic group.
4. `topic` creates the sidebar section heading. Use the same string for all chapters in the same topic.
5. Use `<MachineRef name="Win-A" />` anywhere in the MDX body to render a colored machine badge.

## Cloudflare Pages Deployment

### First-time setup

1. Push this folder to a GitHub repo (create one at github.com/vikasswaminh/quickztna-user-guide or add as subfolder).

2. In Cloudflare Dashboard → Pages → Create a project → Connect to Git:
   - Repository: your repo
   - Branch: `main`
   - Build command: `npm run build`
   - Build output directory: `dist`
   - Root directory: `/` (or `user-guide/` if it's a subfolder of a monorepo)

3. Add custom domain:
   - Pages project → Custom domains → Add `user.quickztna.com`
   - Cloudflare DNS: `user` CNAME → `<project-name>.pages.dev` (Cloudflare adds this automatically if DNS is on Cloudflare)

### Daily update workflow

1. Claude writes new MDX chapters (you ask, Claude writes)
2. Copy the MDX file(s) to `src/content/chapters/`
3. Commit and push:
   ```bash
   git add src/content/chapters/
   git commit -m "Day X: Add <topic> chapters"
   git push
   ```
4. Cloudflare Pages auto-deploys within 60 seconds.

## Project Structure

```
src/
  content/
    chapters/       ← MDX chapter files go here
  content.config.ts ← Schema definition (Astro 6.x)
  layouts/
    ChapterLayout.astro  ← Page layout with sidebar
  components/
    Sidebar.astro        ← Chapter navigation
    MachineRef.astro     ← Machine badge component
  pages/
    index.astro          ← Landing page
    chapters/[...slug].astro  ← Chapter pages
  styles/
    global.css           ← Tailwind + custom styles
```
