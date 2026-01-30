# AI Aliens - Workflow Specifications

## Overview

Four workflows handle all content generation for the AI Aliens project. Each workflow is triggered manually and handles both initial runs and redo runs based on spreadsheet state.

---

## Workflow 1: Style Generation

### Purpose
Generates a style reference image for a location — an establishing shot with no characters that defines the visual look for all subsequent content.

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
- **Locations tab**: Status → Style_Pending
- **Google Drive**: {Location_Name}_style.png

### After Running
- You review the style image
- If approved: Set Location.Status = Style_Approved
- If redo needed: Set Location.Status = Style_Redo, run workflow again

---

## Workflow 2: Image Pipeline v3

### Purpose
Handles random assignments, prompt generation, title/caption creation, and image generation for all 14 content pieces (intro, gorb_story, pleck_story, 10 snapshots, review). Also handles image redos.

### Trigger
Manual

### Reads
- **Locations tab**: Row where `Status = Style_Approved` or `Status = Images_Review`
- **Content tab**: Existing rows for this location (if any)
- **Spots tab**: Available spots for this location
- **Foods tab**: Available foods for this location
- **Dialogue tab**: Row for this location
- **Pools tab**: time_of_day values
- **Photo_Activities tab**: All activities

### Logic

#### Initial Run (no Content rows except style)
```
1. Random Assignments:
   - intro → landing_zone spot (fixed), Gorb solo
   - gorb_story → random generic spot, Gorb solo
   - pleck_story → random generic spot, Pleck solo
   - snapshot_01-04 → random generic spots + general activities, both characters
   - snapshot_05 → random food_vendor spot + food + food activity, both characters
   - snapshot_06-09 → random generic spots + general activities, both characters
   - snapshot_10 → random food_vendor spot + food + food activity, both characters
   - review → random generic spot, solo (based on Review_Speaker)

2. Create 14 Content rows with assignments (Spot_ID, Food_ID)

3. Build image prompts for each piece:
   - Solo character framing for video content (intro, gorb_story, pleck_story, review)
   - Both characters for snapshots
   - Random time of day (daytime only for intro)

4. Build video prompts for video content:
   - Single clip format
   - Uses videoId + voice from character definitions
   - Reads dialogue from Dialogue tab

5. Call Claude to generate titles and captions for each piece

6. Generate 14 images via Nano Banana API
   - Solo content: single character reference image
   - Snapshots: both character reference images

7. Upload images to Google Drive: {Location_ID}_{content_type}.png

8. Mark spots/foods as Used in spreadsheet

9. Set all Content rows: Image_Status = Pending → Review

10. Update Location.Status = Images_Review

11. Send email notification "Images ready for review"
```

#### Redo Run (Content rows exist with Image_Status = Redo)
```
1. Get Content rows where Image_Status = Redo

2. For each redo row:
   - Keep existing: Spot_ID, Food_ID, Title, Caption
   - Rebuild image prompt with fresh randoms (activity, time of day)
   - Determine solo/both character based on content type

3. Generate only those images via Nano Banana API

4. Upload images, overwriting existing files

5. Set those Content rows: Image_Status = Review, update Image_Prompt

6. Send email notification "Redo images ready for review"
```

### Writes
- **Content tab**: 14 new rows (initial) or updated Image_Status + Image_Prompt (redo)
- **Spots tab**: Used = Yes for assigned spots
- **Foods tab**: Used = Yes for assigned foods
- **Locations tab**: Status → Images_Review
- **Google Drive**: 14 image files

### After Running
- You review each image
- If approved: Set Content.Image_Status = Approved
- If redo needed: Set Content.Image_Status = Redo, run workflow again
- When all 14 are Approved: Ready for Video Pipeline

---

## Workflow 3: Video Pipeline

### Purpose
Generates postcard, slideshows (auto-approved), and 4 videos (intro, gorb_story, pleck_story, review). Also handles video redos.

### Trigger
Manual (after all images approved)

### Reads
- **Locations tab**: Row where all Content.Image_Status = Approved
- **Content tab**: All rows for this location
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

3. Generate 4 videos (all single clip, no stitching):
   - intro: 1 clip via Veo3, Gorb solo
   - gorb_story: 1 clip via Veo3, Gorb solo
   - pleck_story: 1 clip via Veo3, Pleck solo
   - review: 1 clip via Veo3, solo (based on Review_Speaker)
   - Use approved images as first frame
   - Upload: {Location_ID}_intro.mp4, {Location_ID}_gorb_story.mp4, etc.

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
   - Regenerate video via Veo3 using existing Video_Prompt

4. Upload videos, overwriting existing files

5. Set those Content rows: Video_Status = Review

6. Send email notification "Redo videos ready for review"
```

### Writes
- **Content tab**: 3 new rows (postcard, slideshow_01, slideshow_02) + updated Video_Status
- **Locations tab**: Status → Videos_Review
- **Google Drive**: postcard, 2 slideshows, 4 videos

### After Running
- You review each video
- If approved: Set Content.Video_Status = Approved
- If redo needed: Set Content.Video_Status = Redo, run workflow again
- When all 4 videos Approved: Ready for Audio Post-Process

---

## Workflow 4: Audio Post-Process

### Purpose
Replaces Veo3 audio with consistent character voices via ElevenLabs Speech-to-Speech and adds generated ambient audio. Processes approved videos after Video Pipeline.

### Trigger
Manual (after all videos approved)

### Reads
- **Locations tab**: Row where `Status = Videos_Review`
- **Content tab**: Video rows where `Video_Status = Approved` AND `Audio_Status = (blank)`
- **Dialogue tab**: Row for this location (for Review_Speaker)
- **Google Drive**: Approved video files

### Voice IDs
| Character | ElevenLabs Voice ID |
|-----------|---------------------|
| Gorb | `nNXPmxHfg9PtGzFxr9Zd` |
| Pleck | `zYcjlYFOd3taleS0gkk3` |

### Voice Routing
| Content_Type | Voice |
|--------------|-------|
| intro | Gorb |
| gorb_story | Gorb |
| pleck_story | Pleck |
| review | Based on Dialogue.Review_Speaker |

### Ambient Routing
- Uses `Time_of_Day` from Content row
- Night/dusk/dawn → Location.Ambient_Night
- Otherwise → Location.Ambient_Day

### Logic
```
For each video (intro, gorb_story, pleck_story, review):

1. Download video from Drive

2. Write video to disk (/mnt/data/tmp/)

3. Extract audio from video (ffmpeg → mp3)

4. Send audio to ElevenLabs Speech-to-Speech:
   - voice_id based on content type
   - remove_background_noise: true
   - Batching: 1 item at a time, 10s interval (rate limit protection)

5. Write converted voice audio to disk (_voice.mp3)

6. Generate ambient audio via ElevenLabs Sound Effects:
   - text: Location ambient description
   - duration_seconds: 8
   - Batching: 1 item at a time, 10s interval

7. Write ambient audio to disk (_ambient.mp3)

8. Mix voice + ambient (ffmpeg):
   - Voice at 100% volume
   - Ambient at 30% volume
   - Output duration matches voice track

9. Merge mixed audio back to original video (ffmpeg):
   - Copy video stream (no re-encoding)
   - Replace audio with mixed track

10. Upload final video to Drive (overwrites original)

11. Clean up temp files

12. Set Content.Audio_Status = Processed

13. Update Location.Status = Audio_Review
```

### Writes
- **Content tab**: Audio_Status → Processed (for each video)
- **Locations tab**: Status → Audio_Review
- **Google Drive**: Overwrites 4 video files with processed versions

### After Running
- You review each video's audio
- If good: Manually set Location.Status = Ready
- If redo needed: Clear Audio_Status for that video, run workflow again

---

## Content Type Reference

| Content_Type | Has Image | Has Video | Image Approval | Video Approval | Audio Approval | Character |
|--------------|-----------|-----------|----------------|----------------|----------------|-----------|
| style | Yes | No | NO (auto) | — | — | None |
| postcard | Yes | No | NO (auto) | — | — | None |
| intro | Yes | Yes | YES | YES | YES | Gorb solo |
| gorb_story | Yes | Yes | YES | YES | YES | Gorb solo |
| pleck_story | Yes | Yes | YES | YES | YES | Pleck solo |
| snapshot_01-10 | Yes | No | YES | — | — | Both |
| slideshow_01 | No | Yes | — | NO (auto) | — | Both |
| slideshow_02 | No | Yes | — | NO (auto) | — | Both |
| review | Yes | Yes | YES | YES | YES | Solo (Review_Speaker) |

---

## File Naming Convention

All files stored in Google Drive under: `Content/LOC_XXX/`

| File | Generated By |
|------|--------------|
| {Location_Name}_style.png | Style Generation |
| {Location_ID}_postcard.png | Video Pipeline |
| {Location_ID}_intro.png | Image Pipeline |
| {Location_ID}_intro.mp4 | Video Pipeline → Audio Post-Process |
| {Location_ID}_gorb_story.png | Image Pipeline |
| {Location_ID}_gorb_story.mp4 | Video Pipeline → Audio Post-Process |
| {Location_ID}_pleck_story.png | Image Pipeline |
| {Location_ID}_pleck_story.mp4 | Video Pipeline → Audio Post-Process |
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
| {Location_ID}_review.mp4 | Video Pipeline → Audio Post-Process |

---

## APIs Used

| Service | Purpose |
|---------|---------|
| Nano Banana | Image generation with character references |
| Veo3 (via kie.ai) | Video generation |
| kie.ai | Music generation for slideshows |
| ElevenLabs | Speech-to-Speech voice conversion, Sound Effects generation |
| Claude | Title and caption generation |
| Google Drive | File storage |
| Google Sheets | Data storage |
| Blotato | Social media posting (future) |

---

## Character References (Nano Banana)

Gorb: `https://drive.google.com/uc?export=view&id=1pH63y7S_m9n7VW0xgV-_9KQ7Bbgt0acy`

Pleck: `https://drive.google.com/uc?export=view&id=1fTadAxxCe8Oz2z4OHRrMxOr6-hUHUD-z`

---

## Character Voices (ElevenLabs)

| Character | Voice ID | Description |
|-----------|----------|-------------|
| Gorb | `nNXPmxHfg9PtGzFxr9Zd` | Young adult male, American neutral, higher pitch with pre-pubescent quality, light nasal tone, childlike enthusiasm and wonder |
| Pleck | `zYcjlYFOd3taleS0gkk3` | Middle-aged male, American neutral, deep voice (not baritone), dry deadpan delivery, slightly clipped pace, weary dad energy |

---

## Video Prompt Template

All videos use single-clip, single-character format:

```
Static camera, medium close-up. ${character.videoId}, ${character.action}. Voice: ${character.voice}.

${Name}: "${dialogue}"

Ambient sounds: ${ambient}.
```

---

## Image Prompt Structure

### Solo Character (video content)
```
Photorealistic 3D CGI render. ${loc.Name} — ${loc.Description}

${spot.Spot_Description}

${CHARACTER}, ${character.description}, in foreground, medium close-up, ${activity}. ${framing}.

${time.sky}. ${time.lighting}.

Vertical 9:16 composition.
```

### Both Characters (snapshots)
```
Photorealistic 3D CGI render. ${loc.Name} — ${loc.Description}

${spot.Spot_Description}

GORB, ${gorb.description}, and PLECK, ${pleck.description}: ${activity}. ${framing}.

${time.sky}. ${time.lighting}.

Vertical 4:5 composition.
```

---

## Workflow Status

| Workflow | Status |
|----------|--------|
| Style Generation | ✅ Built |
| Image Pipeline v3 | ✅ Built |
| Video Pipeline | ✅ Built |
| Audio Post-Process | ✅ Built |
