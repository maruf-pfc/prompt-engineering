Antigravity natively supports Agent Skills using the exact same format I already gave you (a `SKILL.md` with YAML frontmatter + markdown body). You don't need to convert anything, just place the file correctly.

**How to install it:**

1. Unzip/locate the folder so it looks like this:
```
ai-ui-to-real-ui/
└── SKILL.md
```

2. Put that folder in one of two places depending on scope: Global Scope (`~/.gemini/config/skills/`): Available across all Antigravity products (Antigravity, Antigravity IDE, Antigravity CLI) and projects. [Medium](https://medium.com/google-cloud/tutorial-getting-started-with-antigravity-skills-864041811e0d)
   - **Project-only:** put it in the equivalent project-level `skills/` directory inside your repo (e.g. `.antigravity/skills/ai-ui-to-real-ui/` — check your Antigravity version's docs/UI for the exact project path, it's shown when you create a skill from the IDE).
   - **Global (all projects):** `~/.gemini/config/skills/ai-ui-to-real-ui/SKILL.md`

3. That's it — no registration step needed. Skills are agent-triggered: the model automatically detects the user's intent and dynamically equips the specific expertise required. So once it's in the skills folder, just prompt normally (e.g. *"integrate this mockup without breaking my existing UI"*) and Antigravity will match it via the `description` field and pull it in automatically. [google](https://codelabs.developers.google.com/getting-started-with-antigravity-skills?hl=en)

**Commands to do it via terminal:**

```bash
mkdir -p ~/.gemini/config/skills/ai-ui-to-real-ui
cp SKILL.md ~/.gemini/config/skills/ai-ui-to-real-ui/
```

(swap the path for your project-level skills folder if you want it scoped to just one repo)

One thing to note: Antigravity automatically selects the most relevant skill based on trigger keywords in the user's prompt — so accuracy of the `description` field matters for triggering. The one I wrote already has explicit trigger phrases ("make this look real," "integrate this mockup," "de-AI-ify this UI," etc.), so it should fire correctly, but if you find it's not triggering on certain phrasings you use often, just add those phrases into the `description` line and it'll match more reliably. [William Spurlock](https://williamspurlock.com/blog/google-antigravity-skills-guide/)
