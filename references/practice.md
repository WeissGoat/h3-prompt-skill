# H3 practice layer: anime image-to-video, SFW and adult NSFW

What the official guides do not cover: how to write content that H3 and the
user's LoRAs actually render well. Sources are listed at the end. Where this
file and `base-en.txt` / `ref-en.txt` disagree on format, the official guide
wins; this file decides content and wording.

## 1. Where the prompt goes

The user's cloud workflows live in
`research_reports/minimax-h3/workflows/cloud/`.

| Workflow | Mode | Prompt node |
|---|---|---|
| `i2v_00172_nsfw_v5.json` | I2VA (PinkCherry v1.8 hybrid, heretic text encoder, rzgar 8-step motion enhancer + NSFW LoRAs). The first test on 2026-10-21 smeared into ghosting; use v4 until it is fixed (`nsfw.md` §21) | `MiniMaxH3ImageToVideo` #131 |
| `i2v_00172_nsfw_v4.json` | I2VA (official FL2VA, Turbo v4 + NSFW LoRAs + VBVR) | `MiniMaxH3ImageToVideo` #131 |
| `i2v_00172_nsfw.json` … `_v3.json` | Older NSFW versions, kept to reproduce old results | `MiniMaxH3ImageToVideo` #131 |
| `i2v_00172_turbo.json` | I2VA (official FL2VA, Turbo v4) | `MiniMaxH3ImageToVideo` #131 |
| `i2v_fl2va_ref2va_quality_chain.json` | I2VA / T2VA with Motion Context; Ref2VA part bypassed | `User Prompt` #121 |
| `r2v_hybrid_selflift_00268.json` | Ref2VA (Hybrid loader + SelfLift) | `Input Text (Prompt)` #138 |
| `i2v_long_chain_ref_v3.json` | Ref2VA chain, H3 Motion Context (long videos, §8). Switches: 参考图 2 (`<Picture 2>`), LoRA (MysticXXX), both off | H3 Studio chain card: the image's prompt, segments split by `---` lines |
| `i2v_long_chain_ref_v2.json` / `_fl_v2.json` | Same without the switches; the FL2VA chain flashes at every join, use Ref2VA | Same |
| `long_wanwu_v2.json` | Ref2VA, one segment per run (万物皆可接续) | `提示词` #5518. How to write each segment and run it: `long_wanwu_v2.md` next to the workflow (video-gen `widgets` prints it) |
| `long_continuum_v39.json` | H3 Continuum, all chunks in one prompt | `Text (Multiline)` #188. Prompt format and run steps: `long_continuum_v39.md` next to the workflow |

The image goes into the workflow's LoadImage; the prompt only refers to it as
`<Picture 1>`.

## 2. Anchor the first frame (I2VA)

- Open `[Shot 1]` with the style, then the first-frame anchor. For anime
  images: `2d anime style, 2D-animated.` (`2d anime style` is the trigger of the
  Anime NSFW LoRA and harmless without it).
- Restate in one or two sentences what is in the image: composition, each
  character's hair, eyes, outfit and accessories, pose, setting, lighting. Add
  `keeps the character, outfit, colors and background exactly as in the first
  frame`.
- End the description with a closing consistency line: `The same framing,
  identities, outfits and spatial relationships are kept unchanged until the
  end of the video.` (Telegram group workflow, 2026-10-16.)
- Describe only what the image shows. If the user did not say, do not invent
  hidden body parts, extra characters or a new location.

## 3. Motion

- Plan beats from the duration: 2–3 beats for 4–6 s, 3–4 for 8–10 s. Each
  beat changes one thing and ends in an observable state (where the hand is,
  how the body is positioned, which way she faces).
- Order: action onset → continuous development → result or reaction. Put the
  most important beat in the middle; the last beat is the one H3 compresses.
- One character drives a repeated motion at a time; the other reacts. Writing
  both partners' hips moving (thrust + bounce) gives stutter, shallow motion
  or both moving in sync.
- **Anchor the contact as an absolute state.** For insertion or any body
  contact, say which part is pressed fully and flush against which, and how
  deep (`her pelvis stays pressed flush against his`, `fully inside`). Never
  write hovering, thrusting in the air, or sliding over the belly or buttocks;
  H3 then aims at the belly, the buttocks or empty air. Keep the contact point
  in the same place in the frame. (绘画小屋, 2026-10-13.)
- Write a repeated motion once with its rhythm (`begins a steady, slow rhythm
  and keeps it until the end of the shot`) instead of repeating the verb.
- Literal over evocative. `She slowly lowers her hips until ...` beats `they
  make love`. H3 takes words literally; prompt length is not a problem (about
  7000 characters fit).
- **Clips of 8 s or longer in one shot: time the beats inside the shot**, in
  prose, e.g. `From 0 to 2 seconds, ... From 2 to 5 seconds, ...`. MiniMax's
  own 3D-short skill says H3 follows per-second directives almost verbatim.
  Cover the whole duration without gaps. Do not use the `At 00:03.000` cut
  timestamp for this; that form starts a new shot.
- Do not end on `fades out` or a slow stop unless asked; it wastes the last
  beat.

## 4. Camera

H3 drifts and reframes when the camera is not stated.

- Static (default for explicit content): `The camera holds a static shot; the
  frame never moves: no cut, no pan, no tilt, no zoom, no push-in, no camera
  shake and no reframing.` Listing the excluded moves holds the frame better
  than "static" alone.
- One move at most per shot, as type + amplitude + speed from the official
  table: `The camera pushes in with small amplitude at slow speed toward ...`.
- For contact or penetration, frame a close shot of the contact area; wide
  shots lose the detail.

## 5. Sound

- `overall_soundscape`: 1–4 sentences, one paragraph. Each sound by source,
  material, rhythm and the action it follows: `soft, muffled skin-on-skin
  impacts synchronized with each downward motion`. Never onomatopoeia:
  "slapping"/"啪啪" has come out as explosions.
- Non-verbal voice (breathing, panting, moaning) goes in the soundscape with
  its rhythm: `She breathes heavily and lets out a short, high moan at each
  impact.` Dialogue stays in the description with a speaker ID and `<d>`.
- Anime NSFW LoRA sound words it was trained on: moan, grunt, deep breathing,
  heavy breathing, plopping sound (wet sucking or penetration sound).
- `non_diegetic_music: N/A` unless the user asks for music; otherwise H3 often
  adds cheerful BGM.
- `N/A` does not stop extra voices. Close the soundscape with what must not
  be heard: `Only her breathing and moans are heard; there is no male voice,
  no dialogue, no music and no other ambient sound.` Adjust the list to what
  the user wants.
- Turbo (8–12 step) audio is weak; tell the user that dubbing later is an
  option when sound matters.

## 6. Vocabulary and LoRAs in `i2v_00172_nsfw_v4.json` / `_v5.json`

| LoRA (group) | Default | Prompt |
|---|---|---|
| Anime NSFW V7 (NSFW 动漫风格) | on, 0.7 | Always `2d anime style`. Trained terms: penis, vagina, vaginal, testicles, penetration, insert, pull out, breasts, bouncing, jiggle, anus, anal, buttocks, ass ripple, cowgirl, reverse cowgirl, doggy style, missionary, deep throat, thrust, blow job, suck, cum, cumming, moan, grunt, heavy breathing, plopping sound |
| MysticXXX V4 (NSFW 解剖) | off, 0.3 | No trigger; works through anatomy words. Above 0.5 pulls anime toward realism |
| HMNSFW AIO V2.5 (NSFW 写实动作) | off | For realistic images only, 0.5–0.7 |
| breastplayjiggle (胸部物理) | on, 0.85 | Describe how the breasts bounce, jiggle or sway and what drives it |
| VBVR Pro (动作推理 VBVR) | on, 1.0 | No trigger; helps the model follow the written beats. Lower to 0.5 if motion gets exaggerated |

The v5 base model (PinkCherry) was trained on NSFW captions, and its
heretic text encoder passes explicit words through instead of softening them,
so the plain trained words above matter even more there.

Both workflows run at cfg 1.0 with no negative prompt (a negative prompt has
no effect at cfg 1.0). Write what you want into the positive prompt instead
of listing what to avoid.

For explicit requests use these plain words instead of euphemisms. The base
model alone renders genitals poorly; the LoRAs supply them, which is why the
trained words matter. For a SFW request, write no sexual vocabulary and tell
the user to switch the NSFW groups off and load the official model.

## 7. Ref2VA notes (hybrid / reference workflows)

Follow `ref-en.txt` for the six sections. Practical points:

- When `<Picture 1>` is the opening frame, the summary type is `[keyframe
  completion]` (combine with `reference generation` if other assets guide
  look or motion). Using the reference image as the first frame keeps anime
  style closer than FL2VA for some images.
- `retention_analysis`: list every identity and outfit attribute that must
  survive (hair, eyes, horns, accessories, each clothing item, art style) and
  one line for camera and framing. Lists hold better than "same character".
- A reference video that drives motion: define the person as `<Subject 1>`
  whose look comes from `<Picture 1>` and whose motion comes from `<Video 1>`;
  its sound becomes `<Audio N>` only if the workflow feeds the audio track.
- Long videos: see §8. Do not chain segments by feeding the previous
  segment's last frame alone; the blur compounds (Discord, 2026-10-05).

## 8. Long videos (chain cards)

One H3 clip is at most 15 s (328 frames). Longer videos come in two kinds:

- **One continuous take** (the same place and framing, the action goes on):
  a chain card in H3 Studio with a chain workflow (§1). Each segment
  continues from the last 22 frames of the previous segment's latent, so
  motion and sound carry over. Tested 2026-10-29/30: 20 s chains with joins
  that are hard to see; the Ref2VA chain holds identity and colors, while a
  single 13.7 s shot reframed itself after about 9 s.
- **A cut film** (different shot sizes, places or times): plan separate shots
  of up to 15 s each and edit them together afterwards, the way the official
  `3d-animation-short-generator` skill does. That is not a chain card.

### 8.1 Plan the segments first

Write a short segment table before any prompt, one row per segment
(adapted from the official 3D-short shot table's Continuity Handoff column):

| Segment | Frames | Starts from | Action | Ends on | Sound |
|---|---|---|---|---|---|
| 1 | 175 | `<Picture 1>` as is | … | exact pose, hands, prop, gaze, position | … |
| 2 | 175 | segment 1's end state, word for word | … | … | … |

- Frames per segment: 124 / 175 / 226 / 277 / 328 only (5.17 / 7.29 / 9.42 /
  11.54 / 13.67 s at 24 fps). 5–10 s segments work best; 175 is the default.
- From segment 2 on, the first 22 frames (about 0.92 s) are the overlap with
  the segment before and are cut from the joined video.
- Every "Ends on" must be a state H3 can hold still: where each hand is, what
  the props are doing, which way she faces, where she stands in the frame.
  Reading the "Starts from" column down must give one unbroken chain.

### 8.2 Write each segment

The chain card's prompt holds every segment; a line starting with `---`
separates them. Each segment is a complete Ref2VA prompt (all six sections,
ref-en.txt):

- `subject_definitions` and the attribute list in `retention_analysis` are
  copied word for word into every segment; the reference image goes to every
  segment, and identical wording keeps identity from drifting.
- Segment 1: `<Picture 1>` is the first frame of `[Shot 1]`; summary type
  `[keyframe completion + reference generation]`; `[Shot 1] The shot begins
  from <Picture 1>.` (the workflow pins `<Picture 1>` to frame 0 here only).
- Segment 2 on: no `<Picture N>` frame line; summary type `[reference
  generation]` with "continuing directly from the preceding moment";
  `[Shot 1] The shot continues directly from the previous moment in the same
  framing.`, then restate the previous segment's end state in the same words.
- One shot per segment (no `[Shot 2]` cut; untested with Motion Context).
  The same camera in every segment: static with the exclusion list (§4) is
  the safest; a slow move must continue in the same direction and speed.
  Keep the same location, lighting and color sentence; nobody leaves the
  frame.
- Timed beats (§3) cover the segment from 0 to its exact length (`From 5.5
  to 7.29 seconds`). In segment 2 on, the first beat (0–1 s) holds or
  continues the end state; the new action starts after it. The key action
  goes in the middle of the chain and of each segment, not in the last
  second of either.
- Sound: a continuous sound (wind, a hum, rain) is written in both segments
  at a join, and its rise or fall continues where it left off. A line of
  dialogue never crosses a join. `non_diegetic_music: N/A` in every segment;
  music is added once to the joined video.
- NSFW chains: restate the contact as an absolute state (§3) in every
  segment's end state and start; the LoRA switch applies to the whole chain
  unless a segment sets it (below).

### 8.3 Per-segment settings

The rest of a `---` line sets the segment after it, as `name=value` pairs
separated by spaces; a `---` line before segment 1 sets segment 1:

```text
--- 帧数=226 种子=99
--- 参考图 2=图2 LoRA=开
```

Names: `帧数` (frames), `种子` (seed), `时长`, `参考图` (the input image), a
parameter's name on the card (e.g. `参考图 2`), or a switch with `开` / `关`.
Images are `图N` (the N-th image of the workspace) or a file name; through
video-gen, `--upload <file>` and the file name. Resolution and model cannot
change per segment. A segment that uses `<Picture 2>` sets `参考图 2=…` and
defines `<Picture 2>`'s subject in that segment only.

### 8.4 Check a chain

- Each segment has all six sections; definitions and retention lists are
  identical across segments.
- Segment 1 begins from `<Picture 1>`; every later segment continues and
  restates the previous end state.
- Beats cover each segment exactly; no segment over 328 frames.
- Camera, location and lighting are the same in every segment.
- No dialogue line or key action sits on a join or in the last second.
- The frame count of every segment that differs from the card is set on its
  `---` line.

## 9. Troubleshooting

| Symptom | Fix |
|---|---|
| Character too lively, random motion | Static camera with the exclusion list; fewer beats; drop adverbs |
| Cheerful music, extra voices | `non_diegetic_music: N/A`; end the soundscape with what must not be heard (§5) |
| Shallow motion, wrong insertion point | Contact as an absolute state, no hovering words (§3); close shot of the contact area; one insertion or withdrawal per beat; only one partner moves |
| Style turns realistic | Keep `2d anime style, 2D-animated`; HMNSFW off; MysticXXX ≤ 0.3 |
| Identity or outfit drifts | Longer first-frame anchor; 5 s; Ref2VA with a retention list |
| Strange sounds | Source + material + rhythm; dialogue only inside `<d>` |
| Last action missing | Move it to the middle beat; time the beats (§3) |
| Chain: a later segment restarts or jumps instead of continuing | Restate the previous end state word for word and hold it for the first second (§8.2) |
| Chain: identity or outfit drifts segment by segment | Identical subject definitions and retention lists in every segment; Ref2VA chain, not FL2VA |
| Chain: a flash or color jump at a join | Same color/lighting sentence in every segment; Ref2VA chain (FL2VA brightens and desaturates at each join) |
| Chain: the camera drifts or the character walks out of frame | Static camera with the exclusion list in every segment; keep her position in each end state |

## Sources

- MiniMax-AI/MiniMax-H3 `skills/h3-prompt-writing` @ a107547 (2026-08-11):
  `base-en.txt`, `ref-en.txt`; `skills/3d-animation-short-generator`
  (per-second directives).
- `research_reports/minimax-h3/nsfw.md` §6, §8 (Discord/Telegram, to 2026-10-11);
  §18–§20 (Telegram group workflow `h3-i2va-nsfw.json` and the 10-16 update:
  soundscape exclusion, closing consistency line, contact anchoring).
- Civitai pages: 2D Anime Style NSFW (2861135), MysticXXX (2856467), HMNSFW AIO
  (2834417), H3 Eros Max (2851079).
- Group workflow `MiniMax_H3_GUI_Segment_00099_` (August): exclusion-list
  camera lock, timed beats, attribute-by-attribute retention.
- Long videos: `research_reports/minimax-h3/long-video/README.md` (chain
  tests 2026-10-29/30, H3 Motion Context 0.7.1); H3 Studio README (chain
  cards, per-segment settings); MiniMax-AI/MiniMax-H3
  `skills/3d-animation-short-generator` `references/shot-table-spec.md`
  (continuity handoff, per-second coverage, cut films).
