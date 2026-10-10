# isaac-r Claude Code plugins

A Claude Code plugin marketplace containing my skills and plugins. Add the marketplace once, then install whichever plugins you want.

## Plugins

| Plugin | What it does |
| :- | :- |
| [tutor-me](plugins/tutor-me) | Socratic coding tutor that guides you to the answer instead of giving it to you |

## Requirements

- [Claude Code](https://code.claude.com/docs) installed and signed in

## Install

Add the marketplace, then install a plugin by name.

From your shell:

```bash
claude plugin marketplace add isaac-r/claude-plugins
claude plugin install tutor-me@isaac-r-plugins
```

Or from inside a Claude Code session:

```
/plugin marketplace add isaac-r/claude-plugins
/plugin install tutor-me@isaac-r-plugins
```

Running `/plugin` on its own opens a panel where you can browse everything in the marketplace.

### For a whole team

To have everyone who works in a repository offered these plugins, add this to that repository's `.claude/settings.json` and commit it:

```json
{
  "extraKnownMarketplaces": {
    "isaac-r-plugins": {
      "source": {
        "source": "github",
        "repo": "isaac-r/claude-plugins"
      }
    }
  },
  "enabledPlugins": {
    "tutor-me@isaac-r-plugins": true
  }
}
```

Teammates are prompted to install the marketplace and listed plugins the next time they open Claude Code in that repository. Add a line under `enabledPlugins` for each plugin you want the team to have.

## Verify

```bash
claude plugin list
claude plugin details tutor-me
```

## Update

```bash
claude plugin marketplace update isaac-r-plugins
```

You can also manage updates from the `/plugin` panel.

## Uninstall

```bash
# One plugin
claude plugin uninstall tutor-me@isaac-r-plugins

# The whole marketplace and everything installed from it
claude plugin marketplace remove isaac-r-plugins
```

## Repository layout

```
claude-plugins/
├── .claude-plugin/
│   └── marketplace.json        # lists every plugin in this repo
├── plugins/
│   └── tutor-me/
│       ├── .claude-plugin/
│       │   └── plugin.json     # plugin metadata and version
│       ├── skills/
│       │   └── tutor-me/
│       │       └── SKILL.md
│       └── README.md
└── README.md
```

## Adding a new plugin

1. Create the plugin folder with its manifest and skill:

   ```
   plugins/<plugin-name>/
   ├── .claude-plugin/plugin.json
   ├── skills/<skill-name>/SKILL.md
   └── README.md
   ```

   `plugin.json` needs at least a `name`, which must match the folder and the marketplace entry. Copy `plugins/tutor-me/.claude-plugin/plugin.json` as a starting point.

2. Add an entry to `.claude-plugin/marketplace.json`:

   ```json
   {
     "name": "<plugin-name>",
     "source": "./plugins/<plugin-name>",
     "description": "One line shown when browsing the marketplace"
   }
   ```

3. Add a row to the Plugins table above.

4. Validate and test locally (see below), then push.

A plugin can hold several related skills. Add more folders under its `skills/` directory. Use a separate plugin when the skill is unrelated, so people can install only what they want.

## Development

Validate after any change:

```bash
claude plugin validate .
```

Test locally without pushing:

```bash
claude plugin marketplace add ./
claude plugin install <plugin-name>@isaac-r-plugins
```

With a local marketplace, edits are picked up at the next session start, or straight away with `/reload-plugins`. To load a single plugin without the marketplace, run `claude --plugin-dir ./plugins/<plugin-name>`.

### Releasing a change

Each `plugin.json` sets a `version`, which keeps users on that version until it changes. When you push a change you want people to receive, bump that plugin's version in the same commit. Plugins are versioned independently.