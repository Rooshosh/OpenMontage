# Personal Media

## When To Use

Use this skill before source-led planning or visual asset planning. Henrik's real footage is the default visual source for OpenMontage work.

## Operating Contract

Prefer Henrik's real footage over stock footage or AI-generated visuals.

Use stock or generated visual media only when Henrik explicitly asks for it, or when personal footage cannot satisfy the brief and you surface the tradeoff first.

Do not plan around metadata alone. Inspect candidate files directly before committing them to a concept, scene plan, or asset manifest.

## Access

Source media is mounted read-only at `/media`.

The Postgres DSN is available as `CONTENT_DB_DSN`.

Postgres is the content-system database. It contains enriched metadata derived from Henrik's personal media, in addition to operational tables for ingest/preprocessing jobs. For footage discovery, start with the media/enrichment tables rather than job-control state.

If selected source bytes need hydration, run:

```bash
r2-fetch /media/...
```

## Process

1. Translate the brief into concrete footage needs: people, places, activities, objects, moods, dates, trips, events, visual motifs.
2. Explore Postgres and `/media` to find candidate personal footage.
3. Inspect promising files directly before using them in a plan.
4. Hydrate missing or archived files with `r2-fetch` when needed.
5. Build a source inventory before planning visuals.

## Output

When proposing a personal-footage edit, report:

- what you searched for,
- what promising media you found,
- what you inspected directly,
- what source gaps remain,
- whether any stock/generated fallback is recommended and why.

## Common Pitfalls

- Using stock or generated visuals before checking Henrik's personal footage.
- Planning from Postgres metadata without inspecting the actual file.
- Treating source media as disposable project assets.
- Copying source media into project folders when a referenced source path or derived render asset would be enough.
- Hiding a weak personal-footage match instead of surfacing the gap and recommending a fallback.
