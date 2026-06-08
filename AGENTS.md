# OpenMontage

**MANDATORY: Read `AGENT_GUIDE.md` before responding to ANY user message.**

Do not act on the user's request until you have read AGENT_GUIDE.md.
It contains routing rules that determine your first action based on what the user asked.
Skipping it WILL cause you to take the wrong action.

## Jarvis Runtime Environment

This repo runs as part of Henrik's Jarvis / AgentOS content system.

Runtime facts relevant to production decisions:

- You are running inside a Docker container on Henrik's Hetzner EX44 server.
- Working repo: `/home/agent/openmontage`.
- Persistent home: `/home/agent`.
- The container has access to the server's Intel iGPU through `/dev/dri`; the machine has no other GPU.
- For GPU-heavy AI work, first check available local/container capabilities. If the task clearly needs cloud GPU, surface that as a production decision before proceeding.
- Prefer normal project/dependency changes over manual container installs. Manual installs are acceptable for quick experiments, but durable requirements should be moved into the Dockerfile or project dependency files.
- Gemini native video understanding is available via `GEMINI_API_KEY` / `GOOGLE_API_KEY`. Use `gemini-3-flash-preview` for viewing videos when useful: source footage, rough cuts, and final edits. It must be used during review, and is also suggested during editing when visual/audio judgment would help. Current official/live limits: 1,048,576 input tokens and 65,536 output tokens. Gemini File API video is processed at 1 FPS by default, so do not assume it sees 30/60 FPS motion detail; pair it with ffmpeg/local checks for sub-second edit glitches. Gemini 3 video uses ~70 tokens/frame at default/low/medium resolution and ~280 tokens/frame at high. Use `MEDIA_RESOLUTION_HIGH` only when small on-screen text/OCR materially matters.
- Cost guideline: `gemini-3-flash-preview` is $0.50 per 1M input tokens and $3.00 per 1M output tokens. At Gemini 3 default video resolution, review calls are usually cheap, roughly a few cents per 10 minutes of video plus output tokens; `MEDIA_RESOLUTION_HIGH` is about 4x the video input cost and should be reserved for OCR/small-detail cases. When Gemini is used materially or repeatedly, include it in the project cost estimate / cost log rather than treating it as free.
- Our preprocessing pipeline already uses this model for personal-footage enrichment. Practical local rule: chunk long clips rather than forcing huge single calls. The live pipeline chunks clips over 70 min into <=45 min pieces because `gemini-3-flash-preview` has empirically failed around ~83 min even below the 1M-token context limit.

For review handoff to Henrik's Mac:

- Use `to-mac <file> [subpath/]` to send files to Henrik's Mac.
- When asking Henrik to review a rendered video, send the review video with `to-mac`; keep support files local unless Henrik asks for them.
- Do not send scratch caches or source media unless Henrik asks.

## Privacy And Redaction

Privacy redaction should be narrow and surgical across the whole editing system. Always redact API keys, auth tokens, payment details, and nudity. Do not over-redact public social media, normal app UI, names, or general email addresses unless they appear in a sensitive auth/payment context.

When useful footage contains a privacy leak, prefer minimally censoring the sensitive element inside the clip over dropping the clip. Cover only the sensitive content itself, such as the API key text rather than the whole surrounding paragraph, and track the blur/box with the footage when the element moves.

## Source Media Handling

Do not copy source footage into the project workspace unless a local working file is actually needed. When creating project-local working media from a large or high-resolution source video, prefer trimming the needed segment first, then downscaling that segment. Henrik's normal outputs are 1080p, so avoid processing entire long 4K originals when only a short 1080p segment is needed. Keep higher resolution only when it is useful for reframing, zooming, OCR, stabilization, or another specific edit need.

## Personal Media Policy

Henrik's real footage is the default visual source.

For production requests, first look for relevant personal footage before planning stock footage, AI-generated visuals, or generic image/video generation. Use stock or generated visual media only when Henrik explicitly asks for it, or when personal footage is insufficient and you surface the tradeoff first.

Before source-led planning, read `skills/environment/personal-media.md`.
