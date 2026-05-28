# Code companion — frontend/backend deploy coordination post


Each folder maps to one stage of the post's narrative:

```
00-independent/         starting point — two workflows, no gate, the race exists
01-coupled-detection/   git diff vs the parent commit to know if backend changed
02-poll-by-sha/         GitHub Actions API filtered by head_sha
03-grace-window/        handle "backend run not registered yet" (race fix)
04-final/               full step with meaningful error surfacing
```

## Suggested reading order

1. **`00-independent/`** — the baseline; what most projects start with.
2. Each stage folder in order — the post's evolution.
3. **`04-final/`** — drop this into your `deploy-frontend.yml` as
   the first step inside `build-and-deploy`.

## Notes

* `00-independent/` is two separate files — they exist as `.yml` so
  you can diff them against your own workflows.
* `04-final/` contains the complete step block plus the workflow
  permissions and checkout config it depends on. Copy all three
  pieces or the gate will silently fail open.
* The `actions: read` permission is the most-missed prerequisite —
  without it the GitHub API returns 403 and `jq` extracts an empty
  string, which the loop treats as "no run yet."

## Pin versions used in the post

| Tool | Version |
| --- | --- |
| `actions/checkout` | `v5` |
| `curl` | system |
| `jq` | system |
| GitHub Actions API | `2022-11-28` |
