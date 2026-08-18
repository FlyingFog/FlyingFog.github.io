# Chinese Synchronization

Use this reference only after the user explicitly agrees to synchronize Chinese pages.

1. Inspect the same English homepage additions and compare them with `content/_index.zh.md`, `content/publications.zh.md`, and `content/projects.zh.md`.
2. Add only entries that are missing from the matching Chinese full list. Preserve every existing Chinese full-list entry; never delete or replace one because it disappeared from the homepage.
3. For publication entries, preserve official titles, venue names, author order, and links. Translate only surrounding descriptive text when needed.
4. For project entries, draft a Chinese translation that preserves the project type, title, organization, location, dates, and links. Present each proposed project translation and explicitly ask the user to confirm its accuracy before modifying any Chinese file.
5. After confirmation, update `content/_index.zh.md` as requested and append the confirmed missing entries to `content/publications.zh.md` or `content/projects.zh.md`. Keep the relevant Markdown conventions and ordering.
6. Summarize the Chinese additions and run `hugo --panicOnWarning --destination /tmp/flyingfog-hugo-check`.
