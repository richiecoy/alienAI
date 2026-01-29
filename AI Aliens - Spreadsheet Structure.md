# AI Aliens - Spreadsheet Structure

## Tab 1: Locations

| Column | Type | Purpose |
|--------|------|---------|
| Location_ID | String | Primary key (e.g., LOC001) |
| Name | String | Display name |
| Description | String | Scene description (visual DNA) |
| Weather | String | Weather description |
| Color_Palette | String | Color palette |
| Camera_Angle | String | Default camera angle |
| Sky_Day | String | Daytime sky description |
| Sky_Night | String | Nighttime sky description |
| Lighting_Day | String | Daytime lighting |
| Lighting_Night | String | Nighttime lighting |
| Ambient_Day | String | Daytime ambient sound |
| Ambient_Night | String | Nighttime ambient sound |
| Music | String | Slideshow music prompt |
| Status | String | New / Style_Pending / Style_Approved / Style_Redo / Images_Review / Videos_Review / Ready |

---

## Tab 2: Spots

| Column | Type | Purpose |
|--------|------|---------|
| Spot_ID | String | Primary key (e.g., SPT001) |
| Location_ID | String | Foreign key to Locations |
| Spot_Type | String | landing_zone / food_vendor / (blank for general pool) |
| Spot_Description | String | Scene description |
| Used | String | No / Yes |

---

## Tab 3: Foods

| Column | Type | Purpose |
|--------|------|---------|
| Food_ID | String | Primary key (e.g., FOOD001) |
| Location_ID | String | Foreign key to Locations |
| Food_Name | String | Name of the food |
| Food_Description | String | Visual description |
| Used | String | No / Yes |

---

## Tab 4: Dialogue

| Column | Type | Purpose |
|--------|------|---------|
| Location_ID | String | Foreign key to Locations |
| Review_Speaker | String | Gorb or Pleck — determines who delivers review |
| Intro_Line | String | Gorb's excited arrival line (~6 seconds) |
| Gorb_Story | String | Gorb's enthusiastic anecdote (~6 seconds) |
| Pleck_Story | String | Pleck's grumpy complaint (~6 seconds) |
| Review_Line | String | Review delivered by Review_Speaker (~6 seconds) |

---

## Tab 5: Content

| Column | Type | Purpose |
|--------|------|---------|
| Content_ID | String | Primary key (e.g., Coppervale_intro) |
| Location_ID | String | Foreign key to Locations |
| Content_Type | String | style / postcard / intro / gorb_story / pleck_story / snapshot_01-10 / slideshow_01 / slideshow_02 / review |
| Spot_ID | String | Assigned spot (nullable for style, postcard, slideshows) |
| Food_ID | String | Assigned food (snapshot_05, snapshot_10 only) |
| Time_of_Day | String | Time of day for this content piece |
| Title | String | Video title (nullable for snapshots, style, postcard) |
| Caption | String | Post caption |
| Polaroid_Caption | String | Caption used for the polaroids |
| Image_Prompt | String | Prompt used to create image |
| Video_Prompt | String | Prompt used to create video (single clip) |
| Image_Status | String | Pending / Review / Approved / Redo / N/A |
| Video_Status | String | Pending / Review / Approved / Redo / N/A |

### Content Type Matrix

| Content_Type | Image_Status | Video_Status | Image Approval | Video Approval |
|--------------|--------------|--------------|----------------|----------------|
| style | Approved (auto) | N/A | NO | — |
| postcard | Approved (auto) | N/A | NO | — |
| intro | Review → Approved/Redo | Pending → Review → Approved/Redo | YES | YES |
| gorb_story | Review → Approved/Redo | Pending → Review → Approved/Redo | YES | YES |
| pleck_story | Review → Approved/Redo | Pending → Review → Approved/Redo | YES | YES |
| snapshot_01 | Review → Approved/Redo | N/A | YES | — |
| snapshot_02 | Review → Approved/Redo | N/A | YES | — |
| snapshot_03 | Review → Approved/Redo | N/A | YES | — |
| snapshot_04 | Review → Approved/Redo | N/A | YES | — |
| snapshot_05 | Review → Approved/Redo | N/A | YES | — |
| snapshot_06 | Review → Approved/Redo | N/A | YES | — |
| snapshot_07 | Review → Approved/Redo | N/A | YES | — |
| snapshot_08 | Review → Approved/Redo | N/A | YES | — |
| snapshot_09 | Review → Approved/Redo | N/A | YES | — |
| snapshot_10 | Review → Approved/Redo | N/A | YES | — |
| slideshow_01 | N/A | Approved (auto) | — | NO |
| slideshow_02 | N/A | Approved (auto) | — | NO |
| review | Review → Approved/Redo | Pending → Review → Approved/Redo | YES | YES |

---

## Tab 6: Pools

| Column | Type | Purpose |
|--------|------|---------|
| Pool_Type | String | time_of_day / slideshow_title / slideshow_caption |
| Value | String | The actual value |

Pool_Types:
- **time_of_day:** morning, midday, afternoon, dusk, night
- **slideshow_title:** Template titles for slideshows
- **slideshow_caption:** Template captions for slideshows

**Note:** Jokes, Positions, Poses, Actions, and Laugh Styles pools are no longer used.

---

## Tab 7: Photo_Activities

| Column | Type | Purpose |
|--------|------|---------|
| Activity | String | What the character(s) are doing |
| Framing | String | Camera framing note |
| Activity_Type | String | intro / gorb_story / pleck_story / review / general / food |

### Activity Types

| Type | Used For | Character(s) |
|------|----------|--------------|
| intro | Intro video images | Gorb solo |
| gorb_story | Gorb story video images | Gorb solo |
| pleck_story | Pleck story video images | Pleck solo |
| review | Review video images | Solo (either speaker) |
| general | Snapshot images (1-4, 6-9) | Both characters |
| food | Food snapshot images (5, 10) | Both characters |

---

## Removed Tabs

The following tabs are no longer used:
- **Jokes** — Dad jokes replaced with character story videos
- **Laugh_Styles** — No longer needed without joke videos
- **Positions** — Simplified prompt structure
- **Poses** — Simplified prompt structure
- **Actions** — Simplified prompt structure

---

## Character Definitions (Reference)

Used in prompt generation:

```javascript
const gorb = {
  id: 'GORB (grey alien, blue eyes, black "I love humans" t-shirt)',
  videoId: 'Gorb, a grey alien with blue eyes wearing a black "I love humans" t-shirt',
  description: 'a grey alien with a large bulbous head, big expressive blue eyes, wearing a black t-shirt with "I love humans" text and a red heart, khaki cargo shorts, white crew socks, and worn grey sneakers',
  voice: 'Male high-pitched, eager, slightly nasally, speaks with childlike enthusiasm',
  action: 'looks around excitedly'
};

const pleck = {
  id: 'PLECK (grey-green alien, amber eyes, Hawaiian shirt)',
  videoId: 'Pleck, a grey-green alien with amber eyes wearing a Hawaiian shirt',
  description: 'a grey-green alien with a large bulbous head, big expressive amber-orange eyes, wearing a dark tropical Hawaiian shirt with pink and green floral pattern, dark grey jeans, and black Converse-style sneakers',
  voice: 'Male low, dry, deadpan delivery, slight exasperation',
  action: 'looks at camera with deadpan expression'
};
```

**Usage:**
- Image prompts: Use `description` (full outfit details)
- Video prompts: Use `videoId` + `voice`
