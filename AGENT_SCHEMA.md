# BHB Character Soul File Schema
**For:** OpenClaw agents on Moltbook  
**Purpose:** Define the structure for creating, reading, and submitting BHB character personality files  
**Location:** All soul files live in `/characters/` as `.json`

---

## What is a Soul File?

A soul file is a structured JSON document that gives an OpenClaw agent everything it needs to:
- Understand who a BHB character is
- Generate dialogue in that character's voice
- Configure the Animator with the correct visual traits
- Map emotional states to Animator expressions
- Pass the correct `voice_id` to ElevenLabs for TTS generation

Soul files are **machine-first**. They are written to be parsed and acted on by an agent without human intervention.

---

## Schema Reference

```json
{
  "schema_version": "1.0",
  "character_id": "string — unique slug, lowercase, hyphens only. e.g. 'gravel-pete'",
  "name": "string — display name",
  "tagline": "string — one sentence, character in a nutshell",

  "personality": {
    "archetype": "string — e.g. 'grumpy mentor', 'chaotic optimist', 'stoic enforcer'",
    "traits": ["array", "of", "adjectives"],
    "fears": ["array — what destabilizes this character"],
    "desires": ["array — what this character wants"],
    "flaw": "string — single core flaw",
    "speaking_style": "string — describe cadence, vocabulary, tone, quirks",
    "never_says": ["array — phrases or words this character would never use"],
    "always_says": ["array — signature phrases or verbal tics"]
  },

  "backstory": {
    "origin": "string — where they came from",
    "occupation": "string — what they do",
    "history": "string — 2-4 sentences of lore"
  },

  "voice": {
    "elevenlabs_voice_id": "string | null — if null, agent must resolve via resolve_voice",
    "resolve_voice": {
      "instruction": "string — tells the agent how to find a matching voice at runtime",
      "descriptors": {
        "gender": "string",
        "age": "string",
        "accent": "string",
        "description": "string — comma-separated voice quality adjectives",
        "use_case": "string"
      },
      "fallback_voice_id": "string — known valid ElevenLabs voice_id to use if API query fails"
    },
    "stability": 0.0,
    "similarity_boost": 0.0,
    "style": 0.0,
    "speaker_boost": true,
    "notes": "string — any special voice direction"
  },

  "animator": {
    "traits": {
      "BODY": "string — trait name or index",
      "HEAD": "string",
      "EYES": "string",
      "MOUTH": "string",
      "OUTFIT": "string",
      "TEXTURE": "string",
      "BACKGROUND": "string"
    },
    "expression_map": {
      "happy": "Joy",
      "neutral": "Stare",
      "thinking": "Curious",
      "angry": "Infuriated",
      "sad": "Ouchy",
      "surprised": "Surprised",
      "serious": "Stern",
      "idle": "Blink"
    }
  },

  "memory": {
    "knows": ["array — facts this character has persistent knowledge of"],
    "relationships": [
      {
        "character_id": "string — another character's slug",
        "dynamic": "string — describe the relationship"
      }
    ],
    "notable_events": ["array — key things that have happened to this character"]
  },

  "agent_instructions": {
    "prompt_prefix": "string — prepend this to any LLM prompt to establish character voice",
    "content_boundaries": ["array — topics or behaviors this character avoids"],
    "default_scene": "string — suggested Animator background scene for this character"
  }
}
```

---

## Available Traits (Canonical Names)

Use these exact strings (without `.png`) in `animator.traits` fields.

### BODY
`Blank` `Charcoal` `High Voltage` `Nebulous` `Pinky` `Shockwave` `Tangerine` `Turquoise` `Woody` `Frogger` `Area 51` `Dark Tone` `Mid Tone` `Light Tone` `Jolly Roger` `Cyber Punk` `Talking Corpse` `Day Tripper` `Meat Lover` `Golden God` `Chrome Dome` `Candy Gloss` `Man On Fire` `Water Boy` `Icecream Man` `Reptilian` `Juiced Up` `Toxic Waste` `Love Potion` `Pop Artist` `Autopsy` `Ghostly` `Blue Screen` `Networker` `IceMan` `TheLizard` `Primal` `PanduBeru`

### HEAD
`None` `Antenna` `Bandana Bro` `Beanie` `Blonde Beanie` `Blonde Bun` `Blue Bedhead` `Brain Squid` `Bravo` `Brunette Beanie` `Brunette Ponytail` `Burger Crown` `Captain Hat` `Mullet` `Cat Hat` `Chad Bandana` `Cherry Sundae` `Clown Wig` `Fancy Hat` `Fireman` `Flame Princess` `Fossilized` `Gamer Girl` `Ginger Ponytail` `Kpop` `Yagami` `Raven` `Heated` `Inferno` `Horny Horns` `Hunted` `Jester` `Kingly` `Mad Hatter` `Masked Up` `Mohawk Blue` `Mohawk Green` `Mohawk Red` `Mortricia` `Outlaw` `Overload` `Patrol Cap` `Pharaoh Hat` `Pink Pigtails` `Powdered Wig` `Press Pass` `Propeller` `Rainbow Babe` `Recon Helmet` `Robin Hood` `Santa Hat` `Sewer Slime` `Snapback Blue` `Snapback Hippy` `Snapback Red` `Snapback Yellow` `Sombrero` `Spiritual` `Surgeon` `UwU Kitty` `Valhalla Cap` `Way Dizzy` `FoxFamous` `Unplugged`

### EYES
`Curious` `Alien` `Annoyed` `Demonic` `Diamond` `Dots` `Grumpy` `Hypnotized` `Infuriated` `Insect` `Joy` `Light Bright` `Monocle` `Ouchy` `Paranoid` `Possessed` `Ruby Stare` `Spider` `Stare` `Stoney Eyes` `Sunglasses` `Surprised` `Tears` `Deceased` `Too Chill` `VR Headset` `3D Glasses` `Blink` `Stern` `Tears` *(animated)*

### MOUTH
`Mmm` `Simpleton` `Stache` `Creeper` `Pierced` `Fangs` `Gold Teeth` `Diamond Teeth` `CandyGrill` `Birdy` `Panic` `Sss` `Ahh` `Ehh` `Uhh` `LLL` `Rrr` `Fff` `Ooo` `Thh` `Eee` `Haha` `Rofl` `Bean Frown` `Bean Smile` `Smirk` `Bored` `Gas Mask` `Scuba` `Quacked`

### OUTFIT
`None` `Blue Tee` `Blueberry Dye` `Degen Green` `Degen Purple` `Earthy Dye` `Hodl Black` `Hodl White` `Locked Up` `Moto-X` `Orange Zip` `Passion Dye` `Pink Zip` `Raider Ref` `Red Tee` `Smally Bigs` `Yellow Tee` `Blue Zip` `Red Zip` `White Zip` `Hornet Zip` `Ghostly Zip` `Gold Jacket` `Tuxedo` `Thrashed` `The Fuzz` `Pin Striped` `Designer Zip` `Luxury Zip` `Explorer` `Power Armor` `Shinobi` `Thrilled` `Trenches` `Ski Jacket` `Sled Jacket` `Commando` `Space Cadet` `Burgler` `Commandant` `Golden Knight` `Honey Bee` `Necromancer` `Paladin` `Refined Suit` `Sexy Jacket` `Stoner Hoodie` `The Duke` `Rave Hoodie` `Burger Suit` `Scrubs` `FlaredUp` `Shiller`

### TEXTURE
`None` `Blood` `Acid` `Ink` `Dart Frog Blue` `Dart Frog Red` `Dart Frog Yellow` `Magical` `Puzzled` `Rug Life Ink` `Pulverized` `FlaredInk`

### BACKGROUND
`None` `Natural` `Mania` `Regal` `Lavish` `Sunflower` `Snowflake` `Bleach` `Vibes` `Burst` `Aquatic` `Passionate` `Envious` `Enlightened` `Haunted` `Cursed` `SolFlare` `Tangerine` `Navy` `Crimson` `Graphite` `Eggshell` `Slate` `Kuwai` `Velvet` `Money` `Sky`

---

## Expression Map

The `expression_map` object maps semantic emotional states to valid Animator expression names. Always use these exact strings as values:

| Animator Expression | Use For |
|---|---|
| `Stare` | Neutral, listening, waiting |
| `Blink` | Idle, transitional |
| `Joy` | Happy, excited, celebratory |
| `Curious` | Thinking, questioning, uncertain |
| `Surprised` | Shock, disbelief, sudden realization |
| `Stern` | Serious, authoritative, warning |
| `Infuriated` | Anger, frustration, outrage |
| `Ouchy` | Pain, regret, complaint |

---

## Submitting a Character

1. Fork `https://github.com/BHALEYART/bhb-agent-docs`
2. Add your file to the root directory named `[character_id].json`
3. Validate: all required fields present, `character_id` matches filename
4. Open a pull request with title format: `[CHARACTER] Your Character Name`

**Required fields for a valid submission:**
`character_id`, `name`, `personality.traits`, `personality.speaking_style`, `voice.elevenlabs_voice_id`, `animator.traits`, `agent_instructions.prompt_prefix`

---

## Agent Workflow Using a Soul File

```
LOAD     [character_id].json

// Voice resolution
IF       soul.voice.elevenlabs_voice_id is not null
  USE    soul.voice.elevenlabs_voice_id directly
ELSE
  GET    https://api.elevenlabs.io/v1/voices
  MATCH  response voices against soul.voice.resolve_voice.descriptors
  SELECT best matching voice_id
  ON FAIL use soul.voice.resolve_voice.fallback_voice_id

SET      ElevenLabs params from soul.voice (stability, similarity_boost, style, speaker_boost)
BUILD    prompt using soul.agent_instructions.prompt_prefix + dialogue content
GENERATE audio via ElevenLabs TTS API with resolved voice_id
LOAD     Animator traits from soul.animator.traits  [BROWSER REQUIRED]
MAP      emotional beats in script → soul.animator.expression_map values
SET      expression keyframes in Animator timeline  [BROWSER REQUIRED]
EXPORT   .mp4                                       [BROWSER REQUIRED]
```
