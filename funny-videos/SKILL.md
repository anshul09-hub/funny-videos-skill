---
name: funny-videos
description: Funny anime shorts that teach one AI idea. Two anime characters (an excited kid who explains, and a deadpan mom or another foil who roasts every point) act out a simple AI or Claude idea as a skit under 60 seconds, 9:16. An image model draws the characters, a local TTS server does the voices, and a HyperFrames edit builds it: a full-screen anime room, speech bubbles, a Claude Code terminal header, and word cards with one clear graphic each. Use for "funny video", "funny short", "make it funny", "anime skit", "/funny-videos", or a new episode of "I explained ___ to my mom".
---

# Funny Videos

**Plain English:** this makes a funny anime short (under 1 minute) that teaches one AI idea in everyday words. A cartoon kid explains it, and the other character twists every point into a joke. It has a full-screen anime room, speech bubbles, a Claude Code terminal header, and word cards that each carry one clear graphic. The voices run locally and for free. Nothing is uploaded without the creator's yes.

Author: Anshul M ([@anshul09.ai](https://instagram.com/anshul09.ai)). Built over several real episodes; every rule below came from a draft that got rejected.

## The format
- **Anime style**, international: no region-only words, clothes or accents.
- **Under 60 seconds.** Plain words anyone gets. The humour must be relatable (mom guilt, a forgotten birthday, "I paid for your English classes"), and the fact taught must be correct.
- **The explainer:** an energetic kid voice whose pitch goes up and down with emotion.
- **The look:**
  - A full-screen anime room with both characters standing. Not a paper panel with seam captions (tried, rejected).
  - White speech bubbles above whoever talks, with words lighting up as they're said.
  - A word card per idea ("1/4 · TOKENS · You pay a tiny bit for every word."). **Each card has one clear graphic on the right** that shows the idea correctly:
    - TOKENS: a sentence splits into token chips and a counter ticks up.
    - CONTEXT WINDOW: messages scroll through a window frame, and the ones that leave turn grey ("forgotten").
    - SKILLS: a recipe card that turns into 3 results made with "same steps".
    - MCP: the Claude logo wires up to apps as they are named.
  - Rejected graphics: a small data bar, a goldfish bowl, tiny boxes, interns, a plug, tick marks. Keep graphics big, simple and literally true.
- **The top:** no code on screen, ever. A hook-line card ("I explained AI to my mom. She destroyed me.") with a "MOM UNDERSTOOD" pill under it that climbs with each answer, then crashes to 0%.
- **The foil drives it:** she asks a funny, relatable question before every idea, and he answers in everyday words. Without her questions it feels like a lecture.
- **Emotion changes must land:**
  - Panic poses with sweat, shock lines, and the room turning grey on the freeze.
  - Camera punch-ins on every roast, screen shakes, and a dim blue for the crickets.
  - An EMOTIONAL DAMAGE stamp on the biggest roast.

## Steps
1. **Script** (`<episode>/lines.json`, one object per line: `id, who, text, instruct, gap, say`):
   - Open with a question, a wrong guess and a panic ("Is it a girl?").
   - Bridge into the lesson out loud ("Okay, okay! Let me explain it properly." then "Four things. First: tokens…"). Jumping straight from a joke into "Tokens" confuses people. Land each word card on the spoken word, not the line start.
   - Each beat follows the same pattern: he explains in everyday words with fillers ("First thing... basically..."), she restates it back as a family roast (Pitch Meeting style), and a short tag lands ("...Kind of, yeah."). Her restatement is the plain-English lesson.
   - **Never switch topics instantly:** after a roast, a 0.8 s pause, then his nervous "heh heh" (cringe pose + "*nervous laughing*" chip) before "Anyway! Next thing...". Repeat it once more, and let the other character cut it off.
   - Don't make the foil a quiz host ("Why is it charging my credit card?"). It isn't funny.
   - Use a fake win before the final fail ("Yes. One question… can Claude make pancakes?").
   - Aim for about 130 words. Leave pauses (`gap`) of 0.6 to 0.8 s after punchlines and about 1.3 s before the crickets.
   - Self-check out loud: is every joke clear to a stranger in 1 second? Does it teach the right thing?
2. **Characters:** use an image model (Gemini works well) to make one 3x2 pose sheet per character on pure white:
   - Explainer: idle, talk, panic, point, cringe, jump.
   - Foil: idle, talk, scold, prop, squint, laugh.
   - Cut each pose out of the sheet into its own transparent PNG. Reuse sheets and the room background across episodes.
3. **Voices** (a local TTS server such as [Voicebox](https://github.com/jamiepine/voicebox)):
   - The server must stay running for the whole voice step. It looks idle while it works; don't stop it.
   - Explainer: a Qwen custom-voice profile, with a per-line `instruct` that starts "Speak as an energetic, nervous 12-year-old American boy."
   - Foil: Kokoro `af_sarah`. Spell "Claude" as "Clawd" in her `say` field.
   - Record the kid's lines with a best-take loop: re-record each line until a speech-to-text pass (whisper) hears every word.
   - Stop the server when done.
4. **Timing:**
   - Clean each line, add the pauses, and write one `voice.wav` plus a `timeline.json` of line start times.
   - If the total goes over 59.5 s, first speed every line up 3.5% with pitch unchanged, then trim gaps. Keep the jokes.
5. **Edit** (a [HyperFrames](https://github.com/heygen-com/hyperframes) project):
   - Pose schedules, word cards and their graphics, terminal status lines and the SFX list are all driven from the timeline.
   - Logos: use the official press kit and keep the source.
   - Music bed: a light comedic track at about -28 LUFS, cut to silence on each big roast. Credit the song in the description. Rotate songs between episodes.
   - Meme sounds on every roast (vine boom, dun dun DUNNN, metal pipe, womp womp, a big one on the last line). Never let a sound run over the next spoken line.
   - Retention: hook card + score + a "wait for mom's last line" teaser from frame 1, no fade-in. No end card, so the Short loops back to line 1.
6. **Checks:**
   - `npx hyperframes lint` must show 0 errors.
   - Snapshot about 14 key frames and look for overlaps (stamps covering the thing they mock, cards wrapping).
   - Render with `-q high`, then loudnorm to -14 LUFS.
   - Transcribe the final audio and confirm every line.
   - Make a frame strip. Save a phone copy under 30 MB (`-crf 25`).
7. **Deliver:** hand over the final file and the phone copy. Say honestly what was not checked (listening by ear, real-time viewing). Upload only after the creator's yes.

## Traps
- **Gemini image page:** the send works, but the page sometimes doesn't show the reply. Never retry-spam (it makes duplicate chats). Open the newest chat and save the image from there. A new chat is sometimes needed for the image tool.
- **Gemini watermark:** there is a sparkle about 120 px from the bottom-right corner. Paint over a 190 px corner.
- **Kokoro and Chatterbox** say "Claude" as "clod". The Qwen kid voice says it correctly.
- **Lint rules:**
  - Every fade-out needs a `tl.set(opacity 0)` right after it.
  - Term cards need an inner non-clip wrapper.
  - No CSS `transform: scale(0)` on anything GSAP scales.
- **Card graphics** need `overflow:hidden` on their box. Long titles (over 8 letters) need a smaller font, or the text spills out of the card.
