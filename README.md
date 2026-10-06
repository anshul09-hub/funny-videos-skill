# Funny Videos: a Claude Code skill

A free [Claude Code](https://claude.com/claude-code) skill that makes funny anime shorts (under 60 seconds, 9:16) that teach one AI idea in everyday words.

A cartoon kid explains the idea. A deadpan mom roasts every point back in plain English, and that roast *is* the lesson. Think "I explained AI to my mom. She destroyed me."

## What's inside
- `funny-videos/SKILL.md`: the full method. Script pattern, pacing, character poses, voices, word-card graphics, sound design, checks, and the traps hit on real episodes.

## Install
Copy the `funny-videos` folder into `~/.claude/skills/` (or your project's `.claude/skills/`). Then ask Claude Code for a funny video, or run `/funny-videos`.

## What it uses
- An image model for the character pose sheets (Gemini works well).
- A local TTS server for the voices, e.g. [Voicebox](https://github.com/jamiepine/voicebox) (Qwen custom voice + Kokoro).
- [HyperFrames](https://github.com/heygen-com/hyperframes) for the edit, plus ffmpeg and whisper for the checks.

## Author
Anshul M. Instagram [@anshul09.ai](https://instagram.com/anshul09.ai), YouTube [Anshul AI](https://www.youtube.com/@anshul09ai).

MIT licensed.
