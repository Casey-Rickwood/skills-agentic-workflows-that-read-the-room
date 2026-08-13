---
name: update-github-info
description: Refresh Mona's GitHub Info content from the latest official GitHub Blog and Changelog updates.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
engine: copilot
tools:
  edit:
  web-fetch:
  github:
    toolsets: [repos]
network:
  allowed:
    - github.blog
    - github.com
safe-outputs:
  create-pull-request:
    max: 1
    title-prefix: "[mona] "
---

# Update GitHub Info

Keep Mona's GitHub Info website current with concise, practical updates from official GitHub sources.

## Instructions

1. Read `notes/mona-notes.md` using the GitHub repository API tools. Treat those notes as the editorial source of truth.
2. Read the current `site/content/github-info.md` using the GitHub repository API tools before deciding what to change.
3. Use `web-fetch` to read https://github.blog/latest/.
4. Use `web-fetch` to read https://github.blog/changelog/.
5. Select the most useful recent Blog and Changelog items for developers. Prefer genuinely new information and avoid duplicating existing entries.
6. Update `site/content/github-info.md` with short, practical summaries. Include the official source URL and identify whether each item came from the GitHub Blog or GitHub Changelog.
7. Preserve the existing Markdown structure and editorial themes unless a small structural improvement is needed for the new information.
8. Review the resulting diff for accuracy, clear source attribution, concise writing, and valid Markdown.
9. Open one pull request with the changes for Mona to review. Use the `create_pull_request` safe output rather than writing directly to the default branch. Explain which sources were used and summarize the content changes in the pull request body.
10. If there are no worthwhile updates or no changes are needed, do not open an empty pull request; report that no update was necessary.

## Constraints

- Do not run `gh aw compile` or any other compilation command.
- Do not create, update, or commit any `.lock.yml` file. Only edit `site/content/github-info.md` and propose that change through the pull request safe output.
