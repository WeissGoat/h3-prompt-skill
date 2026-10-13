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
| `i2v_00172_nsfw.json` | I2VA (PinkCherry v1.5 + NSFW LoRAs) | `MiniMaxH3ImageToVideo` #131 |
| `i2v_00172_turbo.json` | I2VA (official FL2VA, Turbo v4) | `MiniMaxH3ImageToVideo` #131 |
| `i2v_fl2va_ref2va_quality_chain.json` | I2VA / T2VA with Motion Context; Ref2VA part bypassed | `User Prompt` #121 |
| `r2v_hybrid_selflift_00268.json` | Ref2VA (Hybrid loader + SelfLift) | `Input Text (Prompt)` #138 |

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
- Turbo (8–12 step) audio is weak; tell the user that dubbing later is an
  option when sound matters.

## 6. Vocabulary and LoRAs in `i2v_00172_nsfw.json`

| LoRA (group) | Default | Prompt |
|---|---|---|
| Anime NSFW v0.5 (NSFW 动漫风格) | on, 1.0 | Always `2d anime style`. Trained terms: penis, vagina, vaginal, testicles, penetration, insert, pull out, breasts, bouncing, jiggle, anus, anal, buttocks, ass ripple, cowgirl, reverse cowgirl, doggy style, missionary, deep throat, thrust, blow job, suck, cum, cumming, moan, grunt, heavy breathing, plopping sound |
| MysticXXX V4 (NSFW 解剖) | on, 0.4 | No trigger; works through anatomy words. Above 0.5 pulls anime toward realism |
| HMNSFW AIO V2.5 (NSFW 写实动作) | off | For realistic images only, 0.5–0.7 |
| breastplayjiggle (胸部物理) | on, 0.85 | Describe how the breasts bounce, jiggle or sway and what drives it |

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
- Long videos: do not chain segments by feeding the previous segment's last
  frame alone; the blur compounds (Discord, 2026-10-05). Give every segment
  the identity reference image and plan each segment as its own shot.

## 8. Troubleshooting

| Symptom | Fix |
|---|---|
| Character too lively, random motion | Static camera with the exclusion list; fewer beats; drop adverbs |
| Cheerful music | `non_diegetic_music: N/A` |
| Shallow motion, wrong insertion point | Close shot of the contact area; one insertion or withdrawal per beat; only one partner moves |
| Style turns realistic | Keep `2d anime style, 2D-animated`; HMNSFW off; MysticXXX ≤ 0.3 |
| Identity or outfit drifts | Longer first-frame anchor; 5 s; Ref2VA with a retention list |
| Strange sounds | Source + material + rhythm; dialogue only inside `<d>` |
| Last action missing | Move it to the middle beat; time the beats (§3) |

## Sources

- MiniMax-AI/MiniMax-H3 `skills/h3-prompt-writing` @ a107547 (2026-08-11):
  `base-en.txt`, `ref-en.txt`; `skills/3d-animation-short-generator`
  (per-second directives).
- `research_reports/minimax-h3/nsfw.md` §6, §8 (Discord/Telegram, to 2026-10-11).
- Civitai pages: 2D Anime Style NSFW (2861135), MysticXXX (2856467), HMNSFW AIO
  (2834417), H3 Eros Max (2851079).
- Group workflow `MiniMax_H3_GUI_Segment_00099_` (August): exclusion-list
  camera lock, timed beats, attribute-by-attribute retention.
