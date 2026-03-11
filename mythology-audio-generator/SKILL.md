---
name: mythology-audio-generator
description: Generates high-quality Hindi audio from text files using Azure Text-to-Speech (Arjun Multilingual voice). Automatically handles large files by splitting them into sentence-aligned chunks and merging the resulting audio.
---

# Mythology Audio Generator

## Overview
This skill provides a deterministic way to synthesize large Hindi text files into a single unified `.mp3` file. It uses the `en-IN-ArjunIndicNeural` voice, optimized for high-fidelity Indic storytelling.

## Mandatory Workflow

### Step 1: Synthesize Audio
Run the bundled Python script to process the Hindi text file.

```bash
python "C:\Users\BCP\.gemini\skills\mythology-audio-generator\scripts\azure_tts_processor.py" "[input_hindi_txt]" "[output_audio_mp3]"
```

- **[input_hindi_txt]**: Absolute path to the source Hindi text file.
- **[output_audio_mp3]**: Absolute path where the final audio file should be saved.

## Rules
- **Sentence Alignment**: The script ensures that splits occur at complete Hindi sentences (using `।` or `|`) to maintain natural narration flow.
- **Auto-Merge**: Audio chunks are collected and merged into a single file automatically.
- **API Key**: The script uses a pre-configured Azure Speech key and region.
