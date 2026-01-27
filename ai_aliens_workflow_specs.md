# AI Aliens - Workflow Specifications

## Overview

Three workflows handle all content generation for the AI Aliens project. Each workflow is triggered manually and handles both initial runs and redo runs based on spreadsheet state.

---

## Workflow 1: Style Generation

### Purpose
Generates a style reference image for a location Ã¢â‚¬â€ an establishing shot with no characters that defines the visual look for all subsequent content.

### Trigger
Manual

### Reads
- **Locations tab**: First row where `Status = New` or `Status = Style_Redo`

### Logic
```
1. Find eligible location (Status = New or Style_Redo)
2. Build style image prompt (cinematic establishing shot, 16:9, no characters)
3. Generate image via Nano Banana API
4. Create folder in Google Drive: Content/LOC_XXX/
5. Upload image: {Location_Name}_style.png
6. Create Content row:
   - Content_Type = style
   - Image_Status = (blank)
   - Video_Status = N/A
7. Update Location.Status = Style_Pending
8. Send email notification with link to review
```

### Writes
- **Content tab**: 1 new row (style)
- **Locations tab**: Status Ã¢â€ â€™ Style_Pending
- **Google Drive**: {Location_Name}_style.png

### After Running
- You review the style image
- If approved: Set Location.Status = Style_Approved
- If redo needed: Set Location.Status = Style_Redo, run workflow again

---

## Workflow 2: Image Pipeline

### Purpose
Handles random assignments, prompt generation, title/caption creation, and image generation for all 14 content pieces (intro, 2 jokes, 10 snapshots, review). Also handles image redos.

### Trigger
Manual

### Reads
- **Locations tab**: Row where `Status = Style_Approved` or `Status = Images_Review`
- **Content tab**: Existing rows for this location (if any)
- **Spots tab**: Available spots for this location
- **Foods tab**: Available foods for this location
- **Jokes tab**: Unposted jokes (Posted = No)
- **Dialogue tab**: Row for this location
- **Pools tab**: All pool values (position, pose, action, laugh_style, time_of_day)
- **Photo_Activities tab**: All activities

### Logic

#### Initial Run (no Content rows except style)
```
1. Random Assignments:
   - intro Ã¢â€ â€™ landing_zone spot (fixed)
   - joke_01 Ã¢â€ â€™ random spot from pool + random unposted joke
   - joke_02 Ã¢â€ â€™ random spot from pool + random unposted joke
   - snapshot_01-04 Ã¢â€ â€™ random spots from pool + random general activities
   - snapshot_05 Ã¢â€ â€™ random food_vendor spot + random food + random food activity
   - snapshot_06-09 Ã¢â€ â€™ random spots from pool + random general activities
   - snapshot_10 Ã¢â€ â€™ random food_vendor spot + random food + random food activity
   - review Ã¢â€ â€™ random spot from pool

2. Create 14 Content rows with assignments (Spot_ID, Food_ID, Joke_ID)

3. Build image prompts for each piece:
   - Random position, poses, time of day
   - For jokes: random actions, laugh style
   - Use Veo3 best practices (no positional markers, natural prose)

4. Call Claude to generate titles and captions for each piece

5. Generate 14 images via Nano Banana API

6. Upload images to Google Drive: {Location_ID}_{content_type}.png

7. Mark spots/foods/jokes as Used in spreadsheet

8. Set all Content rows: Image_Status = Review

9. Update Location.Status = Images_Review

10. Send email notification "Images ready for review"
```

#### Redo Run (Content rows exist with Image_Status = Redo)
```
1. Get Content rows where Image_Status = Redo

2. For each redo row:
   - Keep existing: Spot_ID, Food_ID, Joke_ID, Title, Caption
   - Rebuild image prompt with fresh randoms (position, poses, time of day, etc.)

3. Generate only those images via Nano Banana API

4. Upload images, overwriting existing files

5. Set those Content rows: Image_Status = Review

6. Send email notification "Redo images ready for review"
```

### Writes
- **Content tab**: 14 new rows (initial) or updated Image_Status (redo)
- **Spots tab**: Used = Yes for assigned spots
- **Foods tab**: Used = Yes for assigned foods
- **Jokes tab**: Posted = Yes for assigned jokes
- **Locations tab**: Status Ã¢â€ â€™ Images_Review
- **Google Drive**: 14 image files

### After Running
- You review each image
- If approved: Set Content.Image_Status = Approved
- If redo needed: Set Content.Image_Status = Redo, run workflow again
- When all 14 are Approved: Ready for Video Pipeline

---

## Workflow 3: Video Pipeline

### Purpose
Generates postcard, slideshows (auto-approved), and 4 videos (intro, 2 jokes, review). Also handles video redos.

### Trigger
Manual (after all images approved)

### Reads
- **Locations tab**: Row where all Content.Image_Status = Approved
- **Content tab**: All rows for this location
- **Dialogue tab**: Row for this location (for video prompts)
- **Google Drive**: Approved images

### Logic

#### Initial Run (no slideshow/postcard files exist)
```
1. Generate postcard:
   - Apply postcard treatment to style image
   - Upload: {Location_ID}_postcard.png
   - Create Content row: Content_Type = postcard, Image_Status = Approved

2. Generate slideshows:
   - Generate music via kie.ai API using Location.Music prompt
   - Download approved snapshot images (1-10)
   - Apply polaroid frame treatment to each
   - Create slideshow_01 from snapshots 1-5
   - Create slideshow_02 from snapshots 6-10
   - Upload: {Location_ID}_slideshow_01.mp4, {Location_ID}_slideshow_02.mp4
   - Create Content rows: Content_Type = slideshow_01/02, Video_Status = Approved

3. Generate 4 videos:
   - intro: 2 clips via Veo3, stitch together
   - joke_01: 1 clip via Veo3
   - joke_02: 1 clip via Veo3
   - review: 2 clips via Veo3, stitch together
   - Use approved images as first frame
   - Upload: {Location_ID}_intro.mp4, {Location_ID}_joke_01.mp4, etc.

4. Set video Content rows: Video_Status = Review

5. Update Location.Status = Videos_Review

6. Send email notification "Videos ready for review"
```

#### Redo Run (Video_Status = Redo)
```
1. Check: Do slideshow files exist?
   - If yes: Skip postcard and slideshow generation
   - If no: Run initial slideshow/postcard steps first

2. Get Content rows where Video_Status = Redo

3. For each redo row:
   - Use same approved image as first frame
   - Regenerate video via Veo3

4. Upload videos, overwriting existing files

5. Set those Content rows: Video_Status = Review

6. Send email notification "Redo videos ready for review"
```

### Writes
- **Content tab**: 3 new rows (postcard, slideshow_01, slideshow_02) + updated Video_Status
- **Locations tab**: Status Ã¢â€ â€™ Videos_Review (or Ready when all approved)
- **Google Drive**: postcard, 2 slideshows, 4 videos

### After Running
- You review each video
- If approved: Set Content.Video_Status = Approved
- If redo needed: Set Content.Video_Status = Redo, run workflow again
- When all 4 videos Approved: Set Location.Status = Ready

---

## Content Type Reference

| Content_Type | Has Image | Has Video | Image Approval | Video Approval |
|--------------|-----------|-----------|----------------|----------------|
| style | Yes | No | NO (auto) | Ã¢â‚¬â€ |
| postcard | Yes | No | NO (auto) | Ã¢â‚¬â€ |
| intro | Yes | Yes | YES | YES |
| joke_01 | Yes | Yes | YES | YES |
| joke_02 | Yes | Yes | YES | YES |
| snapshot_01-10 | Yes | No | YES | Ã¢â‚¬â€ |
| slideshow_01 | No | Yes | Ã¢â‚¬â€ | NO (auto) |
| slideshow_02 | No | Yes | Ã¢â‚¬â€ | NO (auto) |
| review | Yes | Yes | YES | YES |

---

## File Naming Convention

All files stored in Google Drive under: `Content/LOC_XXX/`

| File | Generated By |
|------|--------------|
| {Location_Name}_style.png | Style Generation |
| {Location_ID}_postcard.png | Video Pipeline |
| {Location_ID}_intro.png | Image Pipeline |
| {Location_ID}_intro.mp4 | Video Pipeline |
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
| {Location_ID}_review.png | Image Pipeline |
| {Location_ID}_review.mp4 | Video Pipeline |

---

## APIs Used

| Service | Purpose |
|---------|---------|
| Nano Banana | Image generation with character references |
| Veo3 (via kie.ai) | Video generation |
| kie.ai | Music generation for slideshows |
| Claude | Title and caption generation |
| Google Drive | File storage |
| Google Sheets | Data storage |
| Blotato | Social media posting (future) |

---

## Character References (Nano Banana)

Gorb: `https://drive.google.com/uc?export=view&id=1pH63y7S_m9n7VW0xgV-_9KQ7Bbgt0acy`

Pleck: `https://drive.google.com/uc?export=view&id=1fTadAxxCe8Oz2z4OHRrMxOr6-hUHUD-z`

---

## Veo3 Prompting Best Practices

Based on research from Powtoon and GodofPrompt articles:

1. **Natural prose flow** Ã¢â‚¬â€ No heavy section headers or timestamps
2. **Visual character ID** Ã¢â‚¬â€ Describe characters, don't use "(left)"/"(right)"
3. **Colon dialogue format** Ã¢â‚¬â€ `Character says: "dialogue"`
4. **Always add "No subtitles"** Ã¢â‚¬â€ At the end of every video prompt
5. **Keep dialogue short** Ã¢â‚¬â€ Must fit in ~8 seconds
6. **Specific speaker** Ã¢â‚¬â€ Always clarify who is speaking each line
7. **Not over-engineered** Ã¢â‚¬â€ Keep prompts compact and clear
