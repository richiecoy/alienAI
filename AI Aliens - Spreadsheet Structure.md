# AI Aliens - Spreadsheet Structure

## Tab 1: Locations

| Column | Type | Purpose |
|--------|------|---------|
| Location_ID | String | Primary key (e.g., LOC001) |
| Name | String | Display name |
| Description | String | Scene description |
| Biome | String | Category for reference |
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
| Intro_Gorb_Action | String | Gorb's physical action description |
| Intro_Pleck_Action | String | Pleck's physical action description |
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

### Pool Values

**position:**
- Gorb in foreground, Pleck a step behind
- Side by side, shoulders nearly touching
- Pleck slightly ahead, Gorb catching up
- Back to back, looking opposite directions
- Gorb crouching, Pleck standing tall
- Both leaning against something
- (add more as needed)

**pose:**
- arms outstretched, taking in the view
- hands on hips
- arms crossed loosely
- one hand shielding eyes, looking into distance
- gesturing mid-conversation
- arms at sides, relaxed
- (add more as needed)

**action:**
- leans in conspiratorially
- nudges Pleck with elbow
- gestures enthusiastically
- scratches head
- looks around nervously
- shrugs dramatically
- (add more as needed)

**laugh_style:**
- burst into laughter
- chuckle and shake their heads
- groan and roll their eyes
- laugh until they wheeze
- snort-laugh uncontrollably
- (add more as needed)

**time_of_day:**
- dawn
- morning
- midday
- afternoon
- dusk
- night

---

## Tab 8: Photo_Activities

| Column | Type | Purpose |
|--------|------|---------|
| Activity | String | What they're doing |
| Framing | String | Camera framing note |
| Activity_Type | String | general / food |

---

## Workflow Summary

### Workflow 1: Style Generation
- **Trigger:** Manual
- **Reads:** Locations (Status = New or Style_Redo)
- **Creates:** 1 Content row (style, Image_Status = Approved)
- **Writes:** Location.Status = Style_Pending
- **Output:** Email notification

### Workflow 2: Image Pipeline
- **Trigger:** Manual
- **Initial Run (no Content rows except style):**
  - Random assignments (spots, foods, jokes)
  - Creates 14 Content rows
  - Builds image prompts
  - Claude generates titles/captions
  - Generates 14 images
  - Marks spots/foods/jokes as Used
  - Sets Image_Status = Review
  - Location.Status = Images_Review
- **Redo Run (Content rows exist with Image_Status = Redo):**
  - Keeps Spot_ID, Food_ID, Joke_ID, Title, Caption
  - Rebuilds prompts with fresh randoms
  - Generates only redo images
  - Sets those Image_Status = Review

### Workflow 3: Video Pipeline
- **Trigger:** Manual
- **Initial Run (no slideshow files):**
  - Generates music
  - Creates 2 slideshows
  - Creates postcard
  - Creates 3 Content rows (postcard, slideshow_01, slideshow_02)
  - Generates 4 videos
  - Sets Video_Status = Review
  - Location.Status = Videos_Review
- **Redo Run (Video_Status = Redo):**
  - Uses same approved images
  - Regenerates only redo videos
  - Sets those Video_Status = Review

---

## Files Per Location (22 total)

| Filename Pattern | Generated By |
|------------------|--------------|
| {Location_ID}_style.png | Style Generation |
| {Location_ID}_postcard.png | Video Pipeline |
| {Location_ID}_intro.png | Image Pipeline |
| {Location_ID}_intro.mp4 | Video Pipeline |
| {Location_ID}_review.png | Image Pipeline |
| {Location_ID}_review.mp4 | Video Pipeline |
| {Location_ID}_joke_01.png | Image Pipeline |
| {Location_ID}_joke_01.mp4 | Video Pipeline |
| {Location_ID}_joke_02.png | Image Pipeline |
| {Location_ID}_joke_02.mp4 | Video Pipeline |
| {Location_ID}_snapshot_01.png | Image Pipeline |
| {Location_ID}_snapshot_02.png | Image Pipeline |
| {Location_ID}_snapshot_03.png | Image Pipeline |
| {Location_ID}_snapshot_04.png | Image Pipeline |
| {Location_ID}_snapshot_05.png | Image Pipeline |
| {Location_ID}_snapshot_06.png | Image Pipeline |
| {Location_ID}_snapshot_07.png | Image Pipeline |
| {Location_ID}_snapshot_08.png | Image Pipeline |
| {Location_ID}_snapshot_09.png | Image Pipeline |
| {Location_ID}_snapshot_10.png | Image Pipeline |
| {Location_ID}_slideshow_01.mp4 | Video Pipeline |
| {Location_ID}_slideshow_02.mp4 | Video Pipeline |
