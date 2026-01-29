# AI Aliens — Location Creation Workflow

This document guides the staged creation of a complete location for AI Aliens content generation.

---

## Stage 1: Location Concept

### Questions to ask:

1. **What's the core concept?** (One sentence — what is this place?)
2. **What's the vibe/aesthetic?** (e.g., serene monastery, bustling market, futuristic tech hub, ancient ruins)
3. **Do you have a reference image?** (Optional but helpful)

### If no reference image:

4. **What's the eye-level experience?** (What do you see walking around — materials, architecture, density)
5. **Any flora or fauna?** (Living elements, creatures, plants)
6. **What makes it alien vs. familiar?** (How sci-fi should it feel?)

---

## Stage 2: Location Description

### Purpose

The description is the **visual DNA** of the location — it defines architectural style, materials, and ambient details that maintain consistency across all generated images. It is NOT a compositional shot.

The same description gets combined with different spot descriptions, so it must:
- Establish the look and feel without dictating what's in frame
- Provide enough detail for visual consistency
- Avoid locking camera angles or specific elements that would repeat in every image

### Style Image

The style image is a **wide establishing shot** of the location — not street level. Think postcard or aerial view that shows the character of the place. No characters in the style image.

### Field Rules:

| Field | Purpose | Rule |
|-------|---------|------|
| **Description** | Visual DNA — architecture, materials, ambient details | No sky, no lighting, no weather. No compositional directives. |

### Questions to ask:

1. **Era/genre?** (Victorian steampunk, tropical resort, ancient ruins, etc.)
2. **Architecture?** (Building style, materials, density)
3. **Ambient details?** (What exists in this world — street materials, lighting fixtures, vehicles, flora)
4. **Landmark?** (One defining feature that can appear but isn't forced into every shot)

### Writing rules:

**DO include:**
- Architectural style (half-timbered, brutalist, organic curves)
- Materials (copper, brass, stone, glass, wood)
- World-building facts ("streets made of cobblestone" not "crowded cobblestone streets")
- Ambient elements that define the aesthetic (gas lamps, airships, floating lanterns)
- Landmark existence ("a giant clock tower rising above the city")

**DO NOT include:**
- Sky references (belongs in Sky_Day/Sky_Night)
- Weather references (belongs in Weather)
- Lighting quality (belongs in Lighting fields)
- Compositional directives ("dominates the skyline", "in the foreground")
- Specific named elements that would repeat (shop names, specific storefronts)
- Crowd/activity descriptions (let spot descriptions handle this)
- Vague genre shorthand ("spinning brass gears", "mechanical wares") — be specific or omit

### Example:

**Too specific (causes repetition):**
> "A bustling Victorian market with clock towers, vendor carts, a lens grinder's shop, a locksmith, spinning brass gears everywhere"

**Too vague (loses consistency):**
> "A steampunk city"

**Right balance (visual DNA):**
> "A Victorian steampunk city with cramped half-timbered buildings, exposed copper pipes and fittings accenting the architecture, streets made of cobblestone, ornate wrought-iron gas lamp posts, airships drifting overhead, and one giant clock tower rising above the city."

### After description is complete:

**What should this location be called?** (Name should reflect the description and vibe)

---

## Stage 3: Location Fields

### Field Rules:

| Field | Purpose | Rule |
|-------|---------|------|
| **Weather** | Atmospheric conditions | Short and simple — "Clear and calm", "Misty with light rain", etc. |
| **Color_Palette** | Reference only | Used when writing descriptions. Not included in prompts. |
| **Camera_Angle** | Default camera perspective | e.g., "Eye level", "Elevated street view", "Low angle looking up" |
| **Sky_Day** | What you see looking up | Sky color, sun(s)/moon(s), clouds, flying objects. No shadow or lighting effects. |
| **Sky_Night** | Nighttime sky | Same as above for night. |
| **Lighting_Day** | How light affects the scene | Shadow quality, color temperature, highlights on surfaces. Reference light source but don't describe it. |
| **Lighting_Night** | Nighttime lighting | Same as above for night. |
| **Ambient_Day** | Daytime soundscape | List of sounds you'd hear. |
| **Ambient_Night** | Nighttime soundscape | Same as above for night. |
| **Music** | Slideshow music prompt | Genre, instruments, mood. |

### Questions to ask:

1. **Weather** — Clear, hazy, misty? Should visuals stay crisp or atmospheric?
2. **Sky** — How alien? Single or multiple suns/moons? Anything flying (ships, creatures, etc.)?
3. **Music** — What genre/mood fits the location?

---

## Stage 4: Spots

Each location needs **14 spots**.

### Fixed spots (8):

| Spot_Type | Count |
|-----------|-------|
| landing_zone | 1 |
| food_vendor | 2 |
| landmark | 2 |
| market | 1 |
| bar | 1 |
| street | 1 |

### Random pool spots (6):

Pick 6 from this pool, **max 1 per category**:

- transit
- viewpoint
- local_life
- hidden_gem
- workshop
- entertainment
- museum
- culture
- neighborhood
- shopping

### Writing Spot Descriptions:

**Bad (just physical elements):**
> "A wooden boardwalk extending over pale pink sand where a sleek shuttle has touched down"

**Good (perspective + context + moment):**
> "A wooden shuttle dock where passengers disembark onto pale pink sand, the resort's beachfront stretching out ahead with cabanas and loungers visible in the distance"

**Questions to answer for each spot:**
1. Where exactly are they standing?
2. What do they see in front of them?
3. What's the context/moment? (arriving, exploring, eating, etc.)

### Spatial Context Rules:

The location description provides the world's DNA, but the spot description must specify **where in that world** the characters are. Without this, the model will default to street-level exteriors.

**Interior spots — explicitly say "inside":**

Bad:
> "Municipal steam baths fed by the city's boiler system, tiled pools with condensation dripping from copper pipes overhead"

Good:
> "Inside the municipal steam baths — a tiled hall with sunken pools fed by the city's boiler system, condensation dripping from copper pipes overhead, arched windows fogged with steam"

**Elevated/spanning spots — specify what's below and what's visible:**

Bad:
> "Iron suspension bridge with riveted plates and cable stays, swaying slightly underfoot, open sky visible through the grated walkway"

Good:
> "An iron suspension bridge spanning high above the city rooftops, riveted plates and cable stays, grated walkway with open sky visible below, the clock tower and distant airships in view"

**Key principle:** If the spot isn't at street level or isn't exterior, you must explicitly state where it is — otherwise the model will combine it with the location's street-level DNA and produce confused results.

---

## Stage 5: Foods

Each location needs **2 foods**:
- 1 savory
- 1 sweet

### Questions to ask:

1. **How alien vs. familiar?** (Recognizable Earth food with a twist, or fully alien cuisine?)
2. **Does the food match the location aesthetic?** (Victorian = meat pies, tropical = fresh fruit, etc.)

### Writing food descriptions:

Include:
- What it looks like
- How it's served/presented
- Sensory details (texture, color, steam, etc.)

---

## Stage 6: Dialogue

Each location needs dialogue for intro, stories, and review.

### Fields:

| Field | Purpose |
|-------|---------|
| **Intro_Line** | Gorb's excited arrival line (~6 seconds spoken) |
| **Gorb_Story** | Gorb's enthusiastic anecdote about the location (~6 seconds spoken) |
| **Pleck_Story** | Pleck's grumpy complaint about Gorb (~6 seconds spoken) |
| **Review_Speaker** | Who delivers the review — "Gorb" or "Pleck" |
| **Review_Line** | Final review of the location (~6 seconds spoken) |

### Character notes:

**Gorb:** Enthusiastic, optimistic, easily awed, loves everything. His stories are about absurd things that happened to him that he thought were wonderful — getting hurt, getting lost, doing something stupid, and framing it as the best thing ever.

**Pleck:** Dry, skeptical, exhausted by Gorb. His stories are complaints about something Gorb did — not the location itself, but Gorb's ridiculous behavior while there.

### Example stories:

**Gorb_Story:** "I got lost in the passages for three hours. No map, no clue, just me and the ice. Best self-guided tour I've ever had."

**Pleck_Story:** "Gorb licked nine walls. Nine. He said he was tasting the history. I'm traveling with a child who eats glaciers."

### Questions to ask:

1. **What's Gorb excited about on arrival?** (What's the first thing he'd react to?)
2. **What absurd thing did Gorb do?** (For Gorb_Story — something dumb that he loved)
3. **What did Gorb do that annoyed Pleck?** (For Pleck_Story — Gorb's behavior, not the location)
4. **Who gives the review?** (Gorb = glowing, Pleck = critical)
5. **What's the final verdict?** (One-liner summary of the location)

### Writing guidelines:

- All dialogue should be speakable in ~5-6 seconds (roughly 15-25 words)
- Intro_Line should reference something visible at the landing zone
- Gorb_Story: something objectively bad happens, Gorb thinks it's amazing
- Pleck_Story: Pleck is exhausted by Gorb's specific behavior
- Review_Line should feel like a conclusion to the week's content
 
---

## Prompt Structure (Reference)

When building image prompts, layer the information in this order:

```
1. [Location Description] — sets the world (visual DNA)
2. [Spot Description] — places them with perspective and spatial context
3. [Characters + Activity] — minimal ID + what they're doing + camera
4. [Sky + Lighting] — atmosphere
5. [Composition] — aspect ratio
```

### Example Prompt:

```
Photorealistic 3D CGI render. Veluna Shores — a laid-back tropical beach resort with pale pink sand, spiral-trunked palms with iridescent fronds, pink and gold coral formations, translucent jellyfish-like creatures drifting lazily in the air.

A wooden shuttle dock where passengers disembark onto pale pink sand, the resort's beachfront stretching out ahead with cabanas and loungers visible in the distance.

GORB (grey alien, blue eyes, black "I love humans" t-shirt) and PLECK (grey-green alien, amber eyes, Hawaiian shirt): Gorb looking around in awe, Pleck scratching head uncertainly. Medium shot.

Pale lavender sky with twin suns. Warm golden light casting soft double shadows, lavender ambient fill.

Vertical 9:16 composition.
```

---

## What NOT to Include in Prompts

- Color palette (redundant if descriptions are written well)
- Camera angle field (composition handles this)
- Weather (usually implied by lighting/sky)
- Ambient sound fields (for video/audio only)
- Music field (for slideshows only)
