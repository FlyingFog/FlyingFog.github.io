---
name: sync-homepage-info
description: Sync additions from a Hugo site's English homepage content/_index.md into content/publications.md and content/projects.md. Use when homepage publication or project entries have been updated and the corresponding full lists need append-only synchronization; optionally synchronize the Chinese counterparts after explicit confirmation.
---

# Sync Homepage Information

Synchronize homepage entries with their full lists while preserving all historical list content.

## Workflow

1. Inspect the current Git diff for `content/_index.md`, then read the current homepage and the relevant full-list files. If no usable homepage diff exists, ask for the intended entries instead of guessing.
2. Identify additions only in the `### Research Publication` and `### Project & Program` sections. Ignore homepage-only prose, headings, and “Full” links.
3. For each candidate, compare normalized entry text with its matching full list to avoid duplicate additions. Treat format-only changes as existing entries unless the user says they are distinct.
4. Append each missing publication to `content/publications.md` and each missing project to `content/projects.md`, preserving the local Markdown style and placing new entries in the appropriate existing order. Never remove, replace, or rewrite existing full-list entries—even if the homepage no longer contains them.
5. Show a concise summary of proposed additions and apply the changes. Run `hugo --panicOnWarning --destination /tmp/flyingfog-hugo-check` after editing.

## Chinese synchronization

Ask whether to synchronize `*_zh.md` before editing any Chinese file. Only if the user agrees, read [references/chinese-sync.md](references/chinese-sync.md) and follow it. Do not load that reference for English-only work.
