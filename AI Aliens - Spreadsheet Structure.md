# AI Aliens - Spreadsheet Structure

## Tab 1: Locations

| Column | Type | Purpose |
|--------|------|---------|
| Location_ID | String | Primary key (e.g., LOC001) |
| Name | String | Display name |
| Description | String | Scene description |
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
| Arrival_Gorb | String | Gorb's first line on arrival |
| Arrival_Pleck | String | Pleck's response |
| Arrival_Gorb_Response | String | Gorb's comeback |
| Review_Gorb | String | Gorb's review line |
| Review_Pleck | String | Pleck's review line |

---

## Tab 5: Content

| Column | Type | Purpose |
|--------|------|---------|
| Content_ID | Integer | Primary key (auto-increment) |
| Location_ID | String | Foreign key to Locations |
| Content_Type | String | style / postcard / intro / joke_01 / joke_02 / snapshot_01-10 / slideshow_01 / slideshow_02 / review |
| Spot_ID | String | Assigned spot (nullable for style, postcard, slideshows) |
| Food_ID | String | Assigned food (snapshot_05, snapshot_10 only) |
| Joke_ID | String | Assigned joke (joke_01, joke_02 only) |
| Title | String | Video title (nullable for snapshots, style, postcard) |
| Caption | String | Post caption |
| Polaroid_Caption | String | Caption used for the polaroids |
| Video_Prompt_1 | String | Prompt used to create Video 1 |
| Video_Prompt_2 | String | Prompt used to create Video 2 |
| Image_Status | String | Pending / Review / Approved / Redo / N/A |
| Video_Status | String | Pending / Review / Approved / Redo / N/A |

### Content Type Matrix

| Content_Type | Image_Status | Video_Status | Image Approval | Video Approval |
|--------------|--------------|--------------|----------------|----------------|
| style | Approved (auto) | N/A | NO | â€” |
| postcard | Approved (auto) | N/A | NO | â€” |
| intro | Review â†’ Approved/Redo | Pending â†’ Review â†’ Approved/Redo | YES | YES |
| joke_01 | Review â†’ Approved/Redo | Pending â†’ Review â†’ Approved/Redo | YES | YES |
| joke_02 | Review â†’ Approved/Redo | Pending â†’ Review â†’ Approved/Redo | YES | YES |
| snapshot_01 | Review â†’ Approved/Redo | N/A | YES | â€” |
| snapshot_02 | Review â†’ Approved/Redo | N/A | YES | â€” |
| snapshot_03 | Review â†’ Approved/Redo | N/A | YES | â€” |
| snapshot_04 | Review â†’ Approved/Redo | N/A | YES | â€” |
| snapshot_05 | Review â†’ Approved/Redo | N/A | YES | â€” |
| snapshot_06 | Review â†’ Approved/Redo | N/A | YES | â€” |
| snapshot_07 | Review â†’ Approved/Redo | N/A | YES | â€” |
| snapshot_08 | Review â†’ Approved/Redo | N/A | YES | â€” |
| snapshot_09 | Review â†’ Approved/Redo | N/A | YES | â€” |
| snapshot_10 | Review â†’ Approved/Redo | N/A | YES | â€” |
| slideshow_01 | N/A | Approved (auto) | â€” | NO |
| slideshow_02 | N/A | Approved (auto) | â€” | NO |
| review | Review â†’ Approved/Redo | Pending â†’ Review â†’ Approved/Redo | YES | YES |

---

## Tab 6: Jokes

| Column | Type | Purpose |
|--------|------|---------|
| Joke_ID | Integer | Primary key (auto-increment) |
| Setup | String | Setup line |
| Punchline | String | Punchline |
| Posted | String | No / Yes |

---

## Tab 7: Pools

| Column | Type | Purpose |
|--------|------|---------|
| Pool_Type | String | position / pose / action / laugh_style / time_of_day |
| Value | String | The actual value |

Pool_Types
**laugh_style:**
**time_of_day:**
**slideshow_title:**
**slideshow_caption:**

---

## Tab 8: Photo_Activities

| Column | Type | Purpose |
|--------|------|---------|
| Activity | String | What they're doing |
| Framing | String | Camera framing note |
| Activity_Type | String | general / food |
