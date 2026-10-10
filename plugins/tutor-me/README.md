# tutor-me

A Claude Code skill that acts as a Socratic tutor. Instead of writing the solution for you, Claude asks questions, challenges your decisions and gives graduated hints so you work the problem out yourself.

It's for junior engineers building fundamentals, experienced engineers learning a new language, framework or system, and anyone who wants to stay sharp and avoid skill atrophy from leaning too heavily on AI coding tools.

## What it does

- **You write the code.** Claude won't edit your files or paste the fix.
- **One open question at a time.** No multiple choice, and no walls of questions.
- **It asks why.** You'll be asked to justify your choices and think through trade-offs and edge cases.
- **Hints get bigger gradually.** A hint ladder goes from a gentle nudge, to where to look, to the concept's name, to the docs, to an analogous example, and only then to a full explanation.
- **Safety comes first.** Claude flags security issues, leaked secrets and destructive commands straight away.
- **You can opt out.** Say "just tell me" or "exit tutor mode" at any point.

## Requirements

- [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) installed and signed in
- Git, if you're installing from this repo

## Installation

Claude Code finds skills as folders containing a `SKILL.md` file. You can install this skill for every project or for a single project.

### Option 1: Personal install (all projects)

This makes the skill available in every Claude Code session on your machine.

```bash
git clone https://github.com/isaac-r/tutor-me.git
mkdir -p ~/.claude/skills/tutor-me
cp tutor-me/SKILL.md ~/.claude/skills/tutor-me/SKILL.md
```

To pick up future updates with a plain `git pull`, symlink the file instead of copying it:

```bash
git clone https://github.com/isaac-r/tutor-me.git ~/src/tutor-me
mkdir -p ~/.claude/skills
ln -s ~/src/tutor-me ~/.claude/skills/tutor-me
```

### Option 2: Project install (one repo, shared with your team)

This commits the skill into a repository, so everyone who uses Claude Code in that repo gets it. It works well for onboarding juniors onto a team codebase.

From the root of the target repository:

```bash
mkdir -p .claude/skills/tutor-me
curl -o .claude/skills/tutor-me/SKILL.md \
  https://raw.githubusercontent.com/isaac-r/tutor-me/main/SKILL.md
git add .claude/skills/tutor-me/SKILL.md
git commit -m "Add tutor-me skill"
```

### Resulting layout

```
~/.claude/skills/            # personal
└── tutor-me/
    └── SKILL.md

<repo>/.claude/skills/       # project
└── tutor-me/
    └── SKILL.md
```

## Verify the install

1. Start a new Claude Code session, or restart an existing one, so it picks up the new skill.
2. Ask Claude: `What skills do you have available?`
3. Check that `tutor-me` appears in the list.

## Usage

Claude loads the skill automatically when your request matches its description. You can also ask for it directly:

```
Tutor me through fixing this failing test.
```

```
I'm learning Go. Help me write a function to parse this config file, but don't give me the answer.
```

```
Use the tutor-me skill. I want to understand why my React component re-renders on every keystroke.
```

Recent versions of Claude Code also let you invoke the skill as a slash command:

```
/tutor-me
```

### Tips for getting the most out of it

- **Be honest about your level** when Claude asks. It uses your answer to pitch every question that follows.
- **Answer in your own words**, even if you're unsure. A wrong guess teaches you more than a skipped question.
- **Run things.** When Claude asks what you expect to happen, run the code and find out.
- **Ask for a bigger hint** if you're stuck and getting frustrated. You don't have to drop tutor mode entirely.
- **Do the wrap-up.** Summarising what you learned at the end is where most of it sticks.

## Leaving tutor mode

Say any of these, and Claude will give you the answer directly:

- `just tell me`
- `exit tutor mode`
- `stop tutoring, I need to ship this`

## Customising

Edit `SKILL.md` to change how the tutor behaves. Some common tweaks:

- **`description`** in the frontmatter controls when Claude loads the skill automatically. Add the trigger phrases your team actually uses.
- **The hint ladder** can be shortened if you want faster escalation.
- **"When to break the rules"** is where to add team-specific exceptions, such as production incidents.

Start a new session after editing so the changes are picked up.

## Uninstall

```bash
# Personal
rm -rf ~/.claude/skills/tutor-me

# Project
git rm -r .claude/skills/tutor-me
git commit -m "Remove tutor-me skill"
```

## Inspiration

Inspired by Matt Pocock's `grill-me` skill, reworked to coach rather than solve.