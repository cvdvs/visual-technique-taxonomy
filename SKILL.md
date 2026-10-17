---
name: visual-technique-taxonomy
description: Label images in a reference library by the technique that makes them work (operation, domain, technique), with a carrier, a confidence, and four look fields. Use when indexing images, working a design brief through techniques, or critiquing a layout by mechanism.
---
# Labeling images with the visual technique taxonomy

Load `taxonomy/taxonomy.json` before you start. It holds the four operations, eight domains, 90 techniques and the fixed look lists.

## What a technique is

A technique is a mechanism: the specific thing in an image that gets the viewer to notice, group, follow or infer something. It is never a style or a subject. "Brutalism" and "portrait" go in a description. "One lit face against black" is a technique (chiaroscuro).

Test: if you removed this mechanism, would the image stop working? If not, don't label it.

Each technique sits under an operation (what the image asks the viewer to do: differentiate, group, sequence, infer) and a domain (what it works through: feature, space, gestalt, social, semantic, cultural, time, encoding), written as a path such as `differentiate/feature`. The first path listed is the main one. Use the path that matches how this image uses the technique. Each entry also gives what changes (the visual variable that moves), how to spot it, when to use it, when it fails, and, for most entries, "not to confuse with" notes, which are the tie-breakers between neighbors.

## Labeling rules

- At most three techniques per image. Most get one or two.
- Label only what does real work. Every photo has light and dark and every layout has size differences. Label chiaroscuro or size hierarchy only when that contrast is the idea.
- "None" is a valid answer: plain screenshots, documentation photos, images kept for one small detail, and moodboards of unrelated pieces.
- Name the carrier: the specific element doing the work, in a few words ("the single lit face against black").
- When two techniques fit one element, use their "not to confuse with" notes to choose. Two labels on one element are allowed only when they do two different jobs (a sheer veil hides and tints: occlusion and transparency). Two readings of one job get one label. When a newer technique names the exact job, keep that one.
- For collages, grids and moodboards, label the arrangement (repetition, similarity, color blocking). If one idea carries the board, add it as a second label.
- For video you see one frame. Label what the frame shows. Use time techniques only when the frame shows them or the description plainly states the motion.
- Never label aesthetics, styles or subjects.

## Confidence

| Score | Meaning |
| --- | --- |
| 0.9 | Obvious and central. |
| 0.7 | Clearly present and doing work. |
| 0.5 | A real supporting mechanism you would defend. |

Under 0.5, leave it out.

## The four look fields

These describe how the image looks. Techniques describe how it works. A low camera angle is always recorded as an angle, and labeled "viewpoint" only when the angle is what the image depends on. Pick one value per field from the lists in `scene_fields`:

- **medium:** photo, painting, drawing, print, illustration, comic, animation, 3d-render, interface, graphic-design, typography, diagram, generative, collage, mixed
- **angle:** eye-level, low, high, overhead, worms-eye, tilted, flat
- **framing:** extreme-close-up, close-up, medium, full, wide, all-over, layout
- **lighting:** high-key, low-key, soft, hard, backlit, golden-hour, night, lamplight, neon, self-lit, flat

## Prompt for an AI assistant

Give the assistant `taxonomy.json` first, then:

```
For each image: look at it once. Write a title (2-6 words); a one-paragraph
description (kind of image; what it shows and its composition; how it's
made; palette in named colors; mood; quote legible text; name artists or
brands only when visible); and 3-8 lowercase tags. Give at most three
techniques that are among the main reasons the image works, each with its
path, carrier, what changes, and a confidence of 0.5-0.9, or none. Give the
four look fields from their fixed lists. Never label styles or subjects. If
the image needs a technique the taxonomy lacks, give the closest fit or none
and describe the mechanism in "candidate".
```

## Output shape

```json
{
  "id": "your-own-image-identifier",
  "title": "Lit face, black room",
  "description": "A photograph of one face lit from the left against a black ground ...",
  "tags": ["portrait", "low-key", "studio"],
  "techniques": [
    {
      "technique": "chiaroscuro",
      "path": "differentiate/feature",
      "carrier": "the single lit face against black",
      "varies": "value",
      "confidence": 0.9
    }
  ],
  "look": { "medium": "photo", "angle": "eye-level", "framing": "close-up", "lighting": "low-key" },
  "candidate": null
}
```

To keep results in a spreadsheet, use `templates/labels-template.csv`: one row per image.

Check every result: technique ids exist, look values are on their lists, no more than three techniques, confidence between 0.5 and 0.9. Review AI labels yourself. Models over-label.

## Using it without AI

- **Moodboard:** for each cluster of images you like, name the technique they share. If nothing is shared in how they work, the label is "none" and the board is about taste.
- **Brief:** name the operation the viewer must perform (the `brief_words` on each operation map client language to it); choose the domain the material points to; pick two or three candidate techniques and say why each fits; read their "fails when" notes; look at references that use them; design, and note the technique behind each decision.
- **Critique:** ask what the main technique is and what carries it. If people can't agree, the image probably isn't doing one thing well. Check the technique's "fails when" note against the work.

## Adding techniques over time

Labeling passes will produce misfits. Treat each as a candidate.

1. Record the image, the carrier and why nothing fit. Don't invent labels on the spot.
2. When several candidates describe the same mechanism, write an entry with every field, including "not to confuse with" notes against its nearest neighbors.
3. Add it in a new version, set `since` to that version, and add tie-breaker notes to the neighbors it overlaps.
4. Keep the previous version's file so old labels still resolve. Re-label a sample to confirm the new entry doesn't steal labels it shouldn't.

A new technique or look value raises the minor number (1.5 to 1.6). Wording fixes raise the patch number (1.4 to 1.4.1). Nothing is deleted.
