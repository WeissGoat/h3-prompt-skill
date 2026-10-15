---
name: h3-prompt
description: "Write, fix or explain MiniMax H3 (海螺 3) video prompts for ComfyUI, SFW or explicit adult NSFW, especially making anime/AI images move (图生视频): I2VA first-frame line, integrated_multimodal_description / overall_soundscape / non_diegetic_music, Ref2VA six sections, FL2VA/L2VA/T2VA, camera, sound, dialogue, timed beats, and the trigger words of the user's H3 LoRAs. Use when the user wants an H3 prompt, asks why an H3 clip came out wrong, or wants a prompt for the cloud workflows in research_reports/minimax-h3/workflows/cloud."
---

# MiniMax H3 prompts

H3 reads a structured, English, script-like prompt. This skill turns what the
user wants (and the image, when there is one) into that prompt, checks it, and
says where to paste it.

Local-only: it quotes private community findings and covers adult content.
Never publish it.


## Read before writing

- [references/practice.md](references/practice.md): required for every
  prompt. Content rules (first-frame anchor, motion beats, camera, sound),
  NSFW vocabulary, the LoRAs in the user's workflows, where each workflow
  takes its prompt, and a troubleshooting table.
- [references/base-en.txt](references/base-en.txt): official guide for T2VA,
  I2VA, FL2VA, L2VA. Read it for anything beyond a single-shot I2VA: cuts and
  their timestamps, first-and-last frame, last frame, dialogue rules
  (`<d>`, voiceover, `<scenetrans>`, `<cutoff>`), singing, on-screen text.
- [references/ref-en.txt](references/ref-en.txt): official guide for Ref2VA
  (six sections, `<Subject N>`/`<Picture N>`/`<Video N>`/`<Audio N>`, task
  types, retention markers). Read it whenever reference images, a reference
  video or reference audio are involved.

Both official files are verbatim from MiniMax-AI/MiniMax-H3
`skills/h3-prompt-writing` at commit a107547 (2026-08-11). On format the
official guide wins; on content and wording, practice.md wins.

## Workflow

1. **Mode.** One image that should be the opening frame → I2VA (the default
   for this user). Two images as start and end → FL2VA. Image as the ending →
   L2VA. No image → T2VA. Images/video/audio used as identity, motion or
   sound references → Ref2VA. If the target workflow is known, its mode
   decides (practice.md §1).
2. **Look.** If the image is available, look at it and note style, characters,
   outfit details, pose, setting, lighting. Otherwise work from the user's
   description and do not invent details.
3. **Plan.** Duration (default 5 s), beats (practice.md §3), camera (§4),
   sounds (§5), dialogue if asked. SFW or NSFW follows the user's request.
4. **Write** in the official structure:
   - I2VA: the fixed first line, a blank line, then
     `integrated_multimodal_description:`, `overall_soundscape:`,
     `non_diegetic_music:`, each separated by a blank line.
   - Ref2VA: the six sections of ref-en.txt in order.
   - English throughout; dialogue keeps its language inside `<d>[Language] …</d>`.
5. **Check** (fix before answering):
   - Field names exact and in order; I2VA first line verbatim.
   - Beats fit the duration; the key action is not last; timed beats cover
     the whole clip without gaps.
   - Camera stated; static shots list the excluded moves.
   - Every sound has a source and material; no onomatopoeia; music `N/A`
     unless requested.
   - Nothing added that the user did not ask for (characters, props, outfit
     or scene changes).
   - Anime: `2d anime style, 2D-animated` opens `[Shot 1]`.
6. **Answer** in the user's language: the prompt in one `text` code block,
   then two or three lines on where to paste it, which LoRA groups to switch
   on or off, and suggested settings (0.7 MP, fixed seed while iterating).
   When fixing a prompt, say what was wrong first.

## Out of scope

Writing the image itself, editing workflows, or running Modal. Point to
nsfw-prompt-guide.md` for the human-facing
version of these rules and its DeepSeek system prompt.
