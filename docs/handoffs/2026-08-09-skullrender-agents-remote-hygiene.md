# SkullRender-Agents — remote hygiene (apply locally)

Cloud agent **cannot push** to `crozzbite/SkullRender-Agents` (token is `cursor[bot]`, no write on that repo). Hygiene commit is prepared; apply + push from your PC.

## Already clean on GitHub

- Single branch: `master`
- Tip before hygiene: `048548a`
- Remote URL: https://github.com/crozzbite/SkullRender-Agents
- No extra remote branches

## Pending hygiene commit (local in agent: `0df7da1`)

- Harden `.gitignore` (`.atl/`, `.claude/`, IDE/caches)
- README: Legion tools table, GitHub + office-accelerator links
- Fix `agents-manager.test.ts` (expects all **13** manifests; was stale “5”)

## Apply on Windows

```powershell
cd C:\Users\zzorc\OneDrive\Desktop\WorkDesktop\SkullRender-Agents
git fetch origin
git checkout master
git pull origin master

# Option A — patch from WorkDesktop (this file's sibling):
git apply ..\docs\handoffs\2026-08-09-skullrender-agents-remote-hygiene.patch
# or if you prefer am:
# git am ..\docs\handoffs\2026-08-09-skullrender-agents-remote-hygiene.patch

bun test src/
git add .gitignore README.md src/agents-manager.test.ts
git commit -m "chore(agents): remote hygiene — gitignore, README Legion, fix manifest count test"
git push -u origin master
git status -sb
```

## Option B — open Agents in Cursor Local

Open folder `SkullRender-Agents`, paste the same commit message / files, push with your `crozzbite` credentials.
