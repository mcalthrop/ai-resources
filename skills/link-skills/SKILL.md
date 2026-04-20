---
name: link-skills
description: Link all skills from the ai-resources directory into .claude/skills/. Use when the user asks to "link skills", "set up skills from ai-resources", or "link to all skills in the ai-resources directory".
disable-model-invocation: true
allowed-tools: Bash
---

# link-skills

Create symlinks in `.claude/skills/` for every skill in the `ai-resources` directory, making them available as slash commands.

## Instructions

1. Confirm that an `ai-resources/skills/` directory exists in the current working directory. If not, abort and tell the user to add ai-resources first (see the ai-resources README).
2. Create `.claude/skills/` in the current working directory if it does not already exist.
3. For each subdirectory in `ai-resources/skills/`, create a relative symlink:

```bash
for SKILL_DIR in ai-resources/skills/*/; do
  SKILL_NAME=$(basename "${SKILL_DIR}")
  mkdir -p .claude/skills
  ln -sfn "../../ai-resources/skills/${SKILL_NAME}" ".claude/skills/${SKILL_NAME}"
done
```

4. Report which symlinks were created.
