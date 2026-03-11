---
name: mythology-video-generator
description: Generates storytelling videos by combining an image with a narration track and background music using FFmpeg. Supports background music looping, volume adjustment, and audio fading.
---

# Mythology Video Generator

## Overview
This skill automates the production of video content from static images and audio tracks. It is specifically tuned for mythology storytelling, ensuring the narration is prominent while the background music adds atmosphere.

## Mandatory Workflow

### Step 1: Generate Video
Run the Python script with your assets.

```bash
python "C:\Users\BCP\.gemini\skills\mythology-video-generator\scripts\create_video.py" \
  --image "[path_to_image]" \
  --narration "[path_to_narration_mp3]" \
  --music "[path_to_bg_music_mp3]" \
  --output "[path_to_output_mp4]" \
  --bg_vol 0.12
```

### Parameters
- **--image**: Path to the cover image.
- **--narration**: Path to the main voiceover audio.
- **--music**: Path to the background music track (will auto-loop).
- **--output**: Destination path for the `.mp4` file.
- **--bg_vol**: (Optional) Volume of background music (0.0 to 1.0). Default is `0.12`.

## Rules
- **Audio Priority**: The narration track determines the length of the video.
- **Looping**: Background music will loop infinitely until the narration ends.
- **Fading**: The mixed audio will automatically fade out over the last 2 seconds.
