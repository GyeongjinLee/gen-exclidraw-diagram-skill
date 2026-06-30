# Excalidraw Diagram Skill

[한국어](README.md)

A coding agent skill that generates beautiful and practical Excalidraw diagrams from natural language descriptions. Not just boxes-and-arrows - diagrams that **argue visually**.

Compatible with any coding agent that supports skills (such as [Codex](https://github.com/openai/codex)). For agents that read from `.claude/skills/` (like [Claude Code](https://docs.anthropic.com/en/docs/claude-code) and [OpenCode](https://github.com/nicepkg/OpenCode)), just drop it in and go.

![Example diagram: loop engineering 14-step roadmap](samples/loop-engineering-14-step-roadmap.png)

> The diagram above is an example produced by this skill.

## What Makes This Different

- **Diagrams that argue, not display.** Every shape/group of shapes mirrors the concept it represents — fan-outs for one-to-many, timelines for sequences, convergence for aggregation. No uniform card grids.
- **Evidence artifacts.** As an example, technical diagrams include real code snippets and actual JSON payloads.
- **Built-in visual validation.** A Playwright-based render pipeline lets the agent see its own output, catch layout issues (overlapping text, misaligned arrows, unbalanced spacing), and fix them in a loop before delivering.
- **Brand-customizable.** All colors and brand styles live in a single file (`references/color-palette.md`). Swap it out and every diagram follows your palette.

## Installation

Clone or download this repo, then copy it into your project's `.claude/skills/` directory:

```bash
git clone https://github.com/GyeongjinLee/gen-excalidraw-diagram-skill.git
cp -r gen-excalidraw-diagram-skill .claude/skills/gen-excalidraw-diagram-skill
```

## Setup

The skill includes a render pipeline that lets the agent visually validate its diagrams. There are two ways to set it up:

**Option A: Ask your coding agent (easiest)**

Just tell your agent: *"Set up the Excalidraw diagram skill renderer by following the instructions in SKILL.md."* It will run the commands for you.

**Option B: Manual**

```bash
cd .claude/skills/gen-excalidraw-diagram-skill/references
uv sync
uv run playwright install chromium
```

## Usage

Ask your coding agent to create a diagram:

> "Create an Excalidraw diagram showing how the AG-UI protocol streams events from an AI agent to a frontend UI"

The skill handles the rest — concept mapping, layout, JSON generation, rendering, and visual validation.

## Output

The skill produces two files together:

- **`.excalidraw` file** — an editable Excalidraw JSON file. Open it in [excalidraw.com](https://excalidraw.com) or the Excalidraw editor to fine-tune shapes, text, colors, and layout by hand.
- **PNG file** — a rendered image written next to the `.excalidraw` file, ready to drop into docs or slides.

If you're not happy with the result or want to change just part of it, edit the `.excalidraw` file directly or ask the agent to revise it — the agent re-renders to PNG to confirm the changes.

## Customize Colors

Edit `references/color-palette.md` to match your brand. Everything else in the skill is universal design methodology.

## File Structure

```
gen-excalidraw-diagram-skill/
  SKILL.md                          # Design methodology + workflow
  SKILL.ko.md                       # Korean translation of SKILL.md
  README.md                         # Setup and usage guide (Korean)
  README.en.md                      # English version of README.md
  references/
    color-palette.md                # Brand colors (edit this to customize)
    element-templates.md            # JSON templates for each element type
    json-schema.md                  # Excalidraw JSON format reference
    render_excalidraw.py            # Render .excalidraw to PNG
    render_template.html            # Browser template for rendering
    pyproject.toml                  # Python dependencies (playwright)
```
