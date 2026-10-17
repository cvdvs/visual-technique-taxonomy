# Visual Technique Taxonomy

A way to describe how an image works, not how it looks. Ninety techniques, each with a definition, why it works, how to spot it and when it fails. Most also note the neighbors they get confused with. Version 1.6.

Published by GoodGlyph Studio under CC BY 4.0.

## Who it is for

**Designers** who want to label moodboards, briefs and critiques by mechanism ("one vivid object in a gray scene") instead of style ("minimal").

**Builders** who want to make their own reference app that stores images, labels them by technique and searches by mechanism.

## Use it in three steps

1. Read [SKILL.md](SKILL.md). It holds the rules for labeling an image, by hand or with an AI assistant.
2. Look techniques up in [taxonomy/TAXONOMY.md](taxonomy/TAXONOMY.md). The data is in [taxonomy/taxonomy.json](taxonomy/taxonomy.json).
3. Building an app? Read [building-an-app.md](building-an-app.md).

## Use it as a Claude skill

The repository is a ready-made skill: `SKILL.md` sits at the top and the taxonomy travels with it.

- **Claude Code:** copy the folder to `~/.claude/skills/visual-technique-taxonomy/`, or to `.claude/skills/` inside a project.
- **Claude app:** zip the folder and upload it in Settings, under Skills.
- **Other AI assistants:** paste `SKILL.md` in as instructions and attach `taxonomy/taxonomy.json`.

Once it's installed, asking Claude to label or index your images is enough. It loads the rules itself.

## Keeping the results

To keep labels somewhere you can filter without building an app, open [templates/labels-template.csv](templates/labels-template.csv) in any spreadsheet. It has one row per image, with columns for up to three techniques and the four look fields.

## Foundations

The taxonomy draws on long traditions, cited per entry and in the `foundations` list of `taxonomy.json`: Gestalt grouping (Wertheimer, Koffka, Kanizsa, Rubin, Palmer), color and value theory (Albers, Itten, Chevreul), attention research (Treisman and Gelade, von Restorff), information design (Tufte, Cleveland and McGill, Neurath), and comics, film and image semiotics (McCloud, Barthes, Peirce, Branigan, Zettl). Check the exact reference before quoting any of them.

## License

CC BY 4.0, GoodGlyph Studio, 2026. See [LICENSE](LICENSE).

To credit it: "Visual Technique Taxonomy v1.6 by GoodGlyph Studio, CC BY 4.0", with a link to this repository. Say if you changed it.
