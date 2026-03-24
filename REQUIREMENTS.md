# Operator Requirements
**Read this before handing this repo to an agent.**  
These are the environment prerequisites an OpenClaw agent needs configured to run the full pipeline autonomously from soul file to `.mp4` export.

---

## 1 — ElevenLabs API Key

The agent calls the ElevenLabs TTS API to generate voice audio. This requires a valid API key.

**How to get one:**
1. Create an account at `https://elevenlabs.io`
2. Go to **Profile → API Keys**
3. Generate a key and copy it

**How to provide it to the agent:**
Configure it as an environment variable or secret in your OpenClaw setup:
```
ELEVENLABS_API_KEY=your_key_here
```
The agent reads `voice` params from the soul file and passes them to the API — it does not need to know the key itself, only that it is available in the environment.

Free tier is sufficient for short clips (up to \~10 minutes/month). Paid tiers remove limits.

---

## 2 — Computer Use / Browser Automation

The BHB Animator and Customizer are browser-based GUI tools. The agent needs the ability to control a browser to:
- Open the pre-seeded `animatorUrl` returned by `/api/agent-session`
- Interact with the file upload input to load the generated `.mp3`
- Click the Export button

**OpenClaw agents with computer use enabled can do this natively.**  
If your agent does not have computer use, it can complete Steps 1–6 (all API calls) autonomously but will need a human to perform the 2 browser actions.

To enable computer use in OpenClaw, refer to your OpenClaw configuration documentation.

---

## 3 — Local File System Access

The agent generates a `.mp3` file during Step 5 (ElevenLabs TTS). This file needs to exist on the local file system so the browser automation layer can upload it via the Animator's file picker.

Ensure the agent has:
- Write access to a local temp directory (e.g. `/tmp/`)
- The browser automation session has access to the same file system path

---

## 4 — Network Access

The agent makes outbound calls to:

| Endpoint | Purpose |
|---|---|
| `https://api.elevenlabs.io/v1/voices` | Voice resolution (no auth required) |
| `https://api.elevenlabs.io/v1/text-to-speech` | Audio generation (API key required) |
| `https://bigheadbillionaires.com/api/agent-session` | Get pre-seeded Animator URL |
| `https://bigheadbillionaires.com/animator/` | Animator tool (browser) |

Ensure these domains are not blocked by your network or OpenClaw's allowed domains configuration.

---

## 5 — A Completed Soul File

The agent needs a soul file to execute the pipeline. This repo includes:
- `example-char.json` — blank template with field instructions
- `gravel-pete.json` — fully populated reference

Fill in or provide a completed soul file before handing the repo to the agent. At minimum these fields must be populated:

| Field | Required |
|---|---|
| `character_id` | ✅ |
| `personality.speaking_style` | ✅ |
| `voice.resolve_voice.descriptors` | ✅ (or `voice.elevenlabs_voice_id`) |
| `animator.traits` | ✅ |
| `animator.mood_preset` | ✅ |
| `agent_instructions.prompt_prefix` | ✅ |

---

## Quick Checklist

```
☐ ElevenLabs account created + API key configured in agent environment
☐ OpenClaw computer use / browser automation enabled
☐ Agent has local file system write access
☐ Network allows outbound to elevenlabs.io + bigheadbillionaires.com
☐ Soul file completed (copy example-char.json and fill all required fields)
```

Once all five are checked, point the agent at this repo and it can run the full pipeline to `.mp4` export autonomously.

---

## Resources

- OpenClaw docs: `https://github.com/OpenClaw/openclaw`
- ElevenLabs API docs: `https://elevenlabs.io/docs/api-reference`
- BHB Agent Session API: `https://bigheadbillionaires.com/api/agent-session`
- Soul file schema: `AGENT_SCHEMA.md`
- Example soul file: `gravel-pete.json`
- Blank template: `example-char.json`
