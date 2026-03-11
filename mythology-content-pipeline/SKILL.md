---
name: mythology-content-pipeline
description: Orchestrates a complete content production workflow. Splits large texts, narrates and translates parts, merges them using UTF-8, creates a unified metadata package in a dedicated folder, and finally generates a high-quality Hindi audio file.
---

# Mythology Content Pipeline

## Overview
This skill handles the end-to-end transformation of large mythological texts into digital content. It creates a dedicated output folder for each story, processes text in logical chunks, re-unifies them with UTF-8 fidelity, and produces a complete package including narration, translation, YouTube metadata, scenes, and a final synthesized audio file.

## Mandatory Workflow
Follow these steps in exact order. **Stop the per-part loop strictly after Step 2.**

### Step 0: Setup & Splitting
1. **Create Output Directory:** Create a folder named after the input filename (e.g., kss-chapter-051 for kss-chapter-051.txt) in the same directory as the source.
2. **Text Splitting:** Activate mythology-text-splitter. Split the file into logical parts of 14,000 characters or less. Save these parts into the **newly created folder**.

### Step 1 & 2: PARTIAL PROCESSING (LOOP)
For **each** resulting part file ([folder]/[source]_partX.txt), execute the following:

#### 1. Narrative Generation & QA Loop
1.  **Generate Draft:** Activate `mythology-content-manager` (**MODE 1**). Save as `[folder]/[source]_partX-draft.txt`.
2.  **Proofread:** Activate `mythology-content-editor`. Compare `[source]_partX.txt` (Original) vs `[source]_partX-draft.txt` (Draft).
3.  **Evaluate & Manage:** Activate `mythology-content-manager` (**MODE 2**). Use the report to determine the path (**Path A, B, or C**).
4.  **Refine (if required):** If the manager requires revision, activate `mythology-content-manager` (**MODE 3**). Save finalized version as `[folder]/[source]_partX-narration.txt`.

#### 2. Hindi Translation
1.  **Translate:** Take the final `[folder]/[source]_partX-narration.txt` and activate `mythology-hindi-translator`. Save as `[folder]/[source]_partX-hindi.txt`.

**DO NOT generate YouTube descriptions, scenes, or scripts for individual parts.**

### Step 3: CONSOLIDATION (UTF-8 MERGE)
1. **Merge Narration:** Combine all [folder]/[source]_partX-narration.txt files into a single file: [folder]/[source]-narration.txt. 
2. **Merge Hindi:** Combine all [folder]/[source]_partX-hindi.txt files into a single file: [folder]/[source]-hindi.txt.
3. **CRITICAL:** Use the merge script: python "C:\Users\BCP\.gemini\skills\mythology-content-pipeline\scripts\merge_files.py" "[folder]/[source]_part*-hindi.txt" "[folder]/[source]-hindi.txt".
4. **Verify:** Use read_file on the merged Hindi file to ensure characters are correct.

### Step 4: UNIFIED FINAL PACKAGE
Using ONLY the **merged** files in the output folder, generate the following:
1. **YouTube Content:** Read [folder]/[source]-narration.txt. Generate ONE ~1000-word description for the WHOLE story. Save as [folder]/[source]-youtube-desc.md.
2. **Summary & Scenes:** Read [folder]/[source]-narration.txt. Provide ONE 300-word summary and FIVE dynamic scenes for the WHOLE story. Save as [folder]/[source]-scenes.md.
3. **TTS Setup:** Read [folder]/[source]-hindi.txt. Provide ONE clean script for TTS. List the 5 global image prompts. Save as [folder]/[source]-script-prompts.txt.

### Step 5: AUDIO GENERATION (FINAL)
1. **Synthesize:** Activate mythology-audio-generator to process the merged [folder]/[source]-hindi.txt file.
2. **Output:** Ensure the final file [folder]/[source]-audio.mp3 is created in the output folder.

## Rules
- **UTF-8 Hard Mandate:** All Hindi operations must use UTF-8.
- **Dedicated Folder:** Every single output file (including split parts) MUST reside in the story-specific folder.
- **No Per-Part Metadata:** Metadata and audio are ONLY generated for the complete story.
