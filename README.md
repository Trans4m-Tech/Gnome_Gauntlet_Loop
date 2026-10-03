<p align="center">
  <img src="assets/banner.png" alt="gauntlet loop" width="100%">
</p>

<h1 align="center">Gnome Gauntlet Loop</h1>

<p align="center">
  A skill that turns any goal into one short, paste-ready prompt — a prompt that makes your agent
  set a real quality bar, split the work into small judgeable pieces, run a builder and a
  separate harsh critic on each, compare blind against the bar, and loop until it wins.
</p>

Most agent output stops at "good enough" because nothing is holding it to a standard. This gives it a standard it cannot argue with.

> The gauntlet loop is [Matt Shumer's](https://github.com/mshumer) idea. He wrote the original prompt and named the technique while building [Claude of Duty](https://github.com/mshumer/Claude-of-Duty). This repo packages that pattern as a reusable skill.

---

## Quick start

```bash
git clone https://github.com/Trans4m-Tech/Gnome_Gauntlet_Loop.git
cd Gnome_Gauntlet_Loop
```

Copy the skill folder into your project:

```bash
cp -r skills/gauntlet-loop your-project/.claude/skills/
```

Then in your agent:

```
/gauntlet-loop build me a pricing page for my SaaS
```

It offers you 2 or 3 quality bars to aim at, you pick one, and it hands back a single prompt you paste into a fresh session.

---

## Installation

The skill is a single `SKILL.md` with no dependencies, so installing it is a copy. Pick whichever method fits how you work.

### Option 1 — project only

Scopes the skill to a single repository. Committed to that repo's git history, so it works for everyone on the team and survives a fresh clone.

```bash
# from inside your project
mkdir -p .claude/skills
cp -r /path/to/Gnome_Gauntlet_Loop/skills/gauntlet-loop .claude/skills/
```

### Option 2 — personal, all projects

Makes `/gauntlet-loop` available everywhere you work. Not shared with anyone else.

```bash
# Claude Code
mkdir -p ~/.claude/skills
cp -r /path/to/Gnome_Gauntlet_Loop/skills/gauntlet-loop ~/.claude/skills/
```

```bash
# Claude Desktop / other harnesses that read ~/.agents
mkdir -p ~/.agents/skills
cp -r /path/to/Gnome_Gauntlet_Loop/skills/gauntlet-loop ~/.agents/skills/
```

### Option 3 — as a Claude Code plugin

This repo is also a plugin marketplace, so you can install and update it from inside Claude Code instead of copying files.

```
/plugin marketplace add Trans4m-Tech/Gnome_Gauntlet_Loop
/plugin install gauntlet-loop@gnome-gauntlet-loop
```

The marketplace name is `gnome-gauntlet-loop` and the plugin name is `gauntlet-loop`, so that is the `plugin@marketplace` pair. Both are declared in `.claude-plugin/`.

### Option 4 — Claude Code web

```
/plugin marketplace add Trans4m-Tech/Gnome_Gauntlet_Loop
```

Add the marketplace in the web UI's plugin settings, then install `gauntlet-loop` from the marketplace list.

### Verifying the install

For a manual copy:

```bash
ls .claude/skills/gauntlet-loop/SKILL.md
```

If that prints the path, the skill is in place. For a plugin install:

```
/plugin list
```

Either way, **start a fresh agent session** and type `/gauntlet-loop` — it should appear in the slash-command list. Skills are read at session start, so a session that was already running will not pick up a newly copied skill.

---

## What's included

```
Gnome_Gauntlet_Loop/
├── skills/
│   └── gauntlet-loop/
│       └── SKILL.md        # the whole skill, one file
├── .claude-plugin/
│   ├── plugin.json         # plugin manifest (used by Option 3/4)
│   └── marketplace.json    # marketplace manifest
├── assets/
│   └── banner.png
├── LICENSE                 # CC BY 4.0
├── NOTICE.md               # attribution
└── README.md
```

The skill lives under `skills/gauntlet-loop/` so it works both as a plugin and as a plain copy. There is only one copy of `SKILL.md` — the plugin loader reads it in place, and manual installs copy that same folder.

---

## How it works

1. **You give a goal.** Anything. A site, an essay, a CLI tool, a research brief.
2. **It offers 2 or 3 bars.** Each one is a specific, real thing your agent can actually fetch and compare against. Not "award-winning design", but a named page, a named post, a named repo.
3. **You pick one.** It writes one short prompt, around 150 words, and stops.
4. **You paste it into a fresh session.** That agent splits the work, runs builder and critic pairs, and loops.

The critic is the part that matters. It is a separate agent with fresh context, it opens the actual output, it puts your work next to the bar with the labels stripped, and it says which one is better. Not a score out of 10, which drifts upward every round. A pick.

The loop exits when your work wins the blind comparison, or when you stop the run. Never after a fixed number of rounds.

---

## Why a bar and not a rubric

A rubric asks the agent to grade itself against words it wrote. A bar makes it compare against something that already exists and is undeniably good.

The skill will not accept a vague bar. It checks three things before it writes anything:

- **Named.** A specific thing, not a category.
- **Fetchable.** The critic can screenshot it, read it, run it, or open it. If the agent cannot get the reference, it hallucinates the comparison and approves everything.
- **Comparable.** Both can sit side by side and a judge can pick one.

---

## Examples

```
/gauntlet-loop a landing page for my running brand, dark and green, has to feel alive
```
Bar becomes a specific brand's live campaign page, screenshotted at desktop and mobile.

```
/gauntlet-loop a 2000 word explainer on vector databases for non-engineers
```
Bar becomes a named writer's actual published posts, judged on which one a non-engineer understands faster.

```
/gauntlet-loop a CLI that formats JSON logs
```
Bar becomes a named tool's implementation plus its benchmark, so taste and a number both have to win.

---

## Works with any agent

`/loop` and `ultracode` are Claude Code features. `/loop` reruns a prompt until you stop it, and `ultracode` opts a turn into multi-agent orchestration.

For any other agent, the skill swaps those two lines for plain instructions: keep looping until the critic picks ours, and run the builders and critics as parallel subagents. The structure is identical.

---

## What breaks it

- A vague bar. The critic invents a comparison and approves everything. By far the most common failure.
- The builder judging its own work. The critic needs fresh context and no knowledge of how hard the builder tried.
- A soft critic. Give it a binary job, not a score.
- A fixed round count. The exit is winning, or you calling it.

---

## Credit

The gauntlet loop technique is **[Matt Shumer's](https://github.com/mshumer)**. He built [Claude of Duty](https://github.com/mshumer/Claude-of-Duty), wrote the original prompt, and named the loop. Every idea underneath this skill — the harsh critic, the blind comparison, the refusal to stop until the work wins — comes from that prompt.

The skill itself was written by **Jay E at [RoboNuggets](https://robonuggets.com)** and originally published at [robonuggets/gauntlet-loop](https://github.com/robonuggets/gauntlet-loop). This repository is maintained by **Trans4m Tech**.

Neither the technique nor this skill is that repository. The skill writes a gauntlet loop prompt for you, for any goal, so you do not have to hand-write one each time.

Related reading: [Anthropic on building effective agents](https://www.anthropic.com/engineering/building-effective-agents), which covers the evaluator pattern the loop is built on.

---

## License

CC BY 4.0. Free to use with attribution. See [LICENSE](LICENSE) and [NOTICE.md](NOTICE.md).