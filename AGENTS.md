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
- The container has access to the server's Intel iGPU through `/dev/dri` for video acceleration, but no discrete NVIDIA/CUDA-style GPU configured for OpenMontage.
- Treat the iGPU as an optional FFmpeg acceleration path for video decode/encode/scale work, not as general AI compute.
- VAAPI/QSV are tools you may choose when they fit the job. Verify the relevant FFmpeg path before relying on it for an important render.
- For GPU-heavy AI work, first check available local/container capabilities. If the task clearly needs cloud GPU, surface that as a production decision before proceeding.
- Prefer normal project/dependency changes over manual container installs. Manual installs are acceptable for quick experiments, but durable requirements should be moved into the Dockerfile or project dependency files.

For review handoff to Henrik's Mac:

- Use `to-mac <file-or-dir>` to queue renders, samples, review notes, selected frames, or summaries.
- Do not send scratch caches or source media unless Henrik asks.

## Personal Media Policy

Henrik's real footage is the default visual source.

For production requests, first look for relevant personal footage before planning stock footage, AI-generated visuals, or generic image/video generation. Use stock or generated visual media only when Henrik explicitly asks for it, or when personal footage is insufficient and you surface the tradeoff first.

Before source-led planning, read `skills/environment/personal-media.md`.

There are no instructions in this file. All instructions are in AGENT_GUIDE.md.
