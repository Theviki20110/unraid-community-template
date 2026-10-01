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
- `icon.png` — repository/app icon referenced by `ca_profile.xml` and the templates (512×512, transparent).
- `icon.svg` — vector source of the icon. Regenerate `icon.png` from it if you change it.
- `.github/workflows/validate.yml` — CI: XML well-formedness, required tags, `TemplateURL` consistency, and a `docker manifest inspect` of every `<Repository>` (fails if the image is missing or private).
- `LICENSE` — MIT.

## Publishing to Unraid

Unraid pulls everything from public URLs, so three things must be public:

1. **This repository** — GitHub → Settings → General → Danger Zone → *Change visibility* → Public.
   All `raw.githubusercontent.com` links in `ca_profile.xml` and the templates 404 while it is private.
2. **The container image** `ghcr.io/theviki20110/feedbard` — the first push from GitHub Actions creates the
   package as *private*. Go to https://github.com/Theviki20110?tab=packages → `feedbard` → *Package settings* →
   *Change visibility* → Public. Unraid does `docker pull` anonymously and fails otherwise.
3. **The image itself** — built and pushed by `docker-publish.yml` in the feedbard repo on every push to `main`
   (tags `latest`, `sha-*`, `v*`), for `linux/amd64` and `linux/arm64`.

Then either:

- **Install directly (no CA listing needed):** current Unraid releases no longer have a *Template Repositories*
  field, so drop the XML into the user-templates folder on the flash drive. From the Unraid terminal:

  ```bash
  wget -O /boot/config/plugins/dockerMan/templates-user/my-feedbard.xml https://raw.githubusercontent.com/Theviki20110/unraid-community-template/main/templates/feedbard.xml
  ```

  Then *Docker* tab → *Add Container* → *Template* dropdown → `feedbard` under *User templates*.
- **Get listed in Community Applications:** open a thread in the Unraid forum under *Docker Containers*
  (one thread per app, this is the support link), then submit the repository URL in the CA application
  form: https://forums.unraid.net/topic/87144-ca-application-policies-notes/ and wait for the moderator scan.

Quick check that everything is reachable:

```bash
curl -fsI https://raw.githubusercontent.com/Theviki20110/unraid-community-template/main/templates/feedbard.xml >/dev/null && docker manifest inspect ghcr.io/theviki20110/feedbard:latest >/dev/null && echo OK
```

## Maintenance Notes

- Every Docker app entry needs a `<Repository>` tag.
- Keep each template's `<TemplateURL>` pointed at the raw GitHub URL for that exact XML file.
- CI (`validate.yml`) runs on every push; it fails until the repo and the ghcr package are public.
- Bump `<Date>` and add a `<Changes>` entry whenever a template changes; CA shows the changelog to users.
