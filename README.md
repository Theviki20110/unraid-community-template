# Unraid Community Applications — Theviki20110

Community Apps repository for the Docker applications maintained by
[@Theviki20110](https://github.com/Theviki20110).

## Applications

| App | Template | Source |
| --- | --- | --- |
| **feedbard** | [`templates/feedbard.xml`](templates/feedbard.xml) | [Theviki20110/feedbard](https://github.com/Theviki20110/feedbard) |

### feedbard

Polls RSS/Atom feeds, turns each new post into a narrated audio episode —
translated into your language, with figures described out loud — and publishes
it into an Audiobookshelf **book** library, one folder per article with its own
cover.

Headless: no WebUI. The container runs the pipeline every
`CRON_INTERVAL_SECONDS` and writes to two paths, `/app/data` (its own state)
and `/app/library` (the tree Audiobookshelf reads).

**Before first start**, create the feed list file mounted at
`/app/assets/feeds_list.txt` — one RSS/Atom URL per line. An empty or missing
file means nothing gets processed.

You supply the model backends: an LLM (Anthropic API, AWS Bedrock, or Ollama)
and a TTS service (HTTP, SageMaker, or Amazon Polly). See the template's
variable descriptions, and `.env.example` in the feedbard repo.

## Repository Layout

- `ca_profile.xml` — repository overview shown in Community Apps. Must stay in the root.
- `templates/` — one XML file per Docker app.
- `icon.svg` — repository icon referenced by `ca_profile.xml`.
- `LICENSE` — MIT.

## Maintenance Notes

- Every Docker app entry needs a `<Repository>` tag.
- Keep each template's `<TemplateURL>` pointed at the raw GitHub URL for that exact XML file.
- Run **Validate** and **Scan** in the Community Apps submit flow: `/submit`.
