# plain-english

This project gives Claude a set of rules for writing in plain, clear English. This README explains how the rules work, how to use them in Claude Code and on claude.ai, and how to change them.

Claude gets the rules in two ways:

- An output style makes every Claude Code reply follow the core rules. It uses only the core rules so replies stay fast.
- A skill applies the full set of rules when you ask Claude to rewrite or draft a piece of text in plain English. The output style also tells Claude to load the skill when it writes a document for other people, such as a README or report.

## What it does

The rules help Claude write for the ordinary person in the street. The reader should understand the text in one quick reading.

## Use in Claude Code

You need Git and Claude Code on your computer. The commands below work on macOS and Linux. Run them in a terminal:

```
git clone https://github.com/tshiyou/plain-english.git ~/.claude/skills/plain-english
mkdir -p ~/.claude/output-styles
ln -s ~/.claude/skills/plain-english/output-style/plain-english.md ~/.claude/output-styles/plain-english.md
```

The first command downloads the rules into the folder where Claude Code looks for skills. The other two commands link the output style into the folder where Claude Code looks for output styles.

Restart Claude Code. You can then use the rules in two ways:

- Type `/plain-english` followed by the text you want rewritten. Claude also uses the skill on its own when you ask it to rewrite text in plain English.
- Type `/output-style` and choose Plain English. Every reply then follows the core rules.

To get the latest rules, run `git -C ~/.claude/skills/plain-english pull`.

## Use on claude.ai

claude.ai cannot read `~/.claude`, so it needs its own copy of the rules.

For the skill, zip the folder:

```
cd ~/Desktop && zip -r plain-english.zip plain-english -x "plain-english/.git/*" "plain-english/output-style/*"
```

Then go to Settings → Capabilities → Skills on claude.ai. Turn on code execution and upload `plain-english.zip`.

claude.ai has no output styles. To apply the core rules to every reply, paste part of `SKILL.md` into one of the places below. Copy the text after the frontmatter, which is the block between the two `---` lines at the top. Stop at the line `<!-- output style ends here -->`.

- A custom style: in the chat box, open the style menu and choose "Create & edit styles". You can turn the style on or off for each chat.
- Project instructions: the rules apply to every chat in that project.

The copy on claude.ai does not update by itself. After any change to `SKILL.md`, upload a new zip and update the style or project instructions.

## Rules

All the rules live in `SKILL.md`. The Core rules section comes first and holds the rules for every reply. A marker line, `<!-- output style ends here -->`, follows it. The sections after the marker hold the rest of the rules, one section for each category.

| Section         | What it covers                                                                                                                                                       |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Scope and speed | Apply the rules to the reply only, and write to them as you go without a checking pass                                                                               |
| Core rules      | The short set used by the output style: answer first, short sentences and paragraphs, plain words, the main AI tells, and a short answer when the reader must decide |
| AI tells        | More habits that make text read as machine-written: signposts, stock transitions, "-ing" importance clauses, inflated claims, showy words                            |
| Concreteness    | Naming things directly instead of using "it", "this" or vague words                                                                                                  |
| Consistency     | Using the same word, person, capitals and hyphens throughout                                                                                                         |
| Layout          | Paragraph openings and punctuation                                                                                                                                   |
| Sentences       | Clauses, negatives, short words and full sentences                                                                                                                   |
| Structure       | Order of ideas, and a reason for each claim                                                                                                                          |
| Tone            | A direct, first-person voice                                                                                                                                         |
| Word choice     | Plain words in place of jargon, legal wording, clichés and empty phrases                                                                                             |

When two rules conflict, the stricter and simpler rule wins. If that does not settle it, the rule that keeps the meaning clearer wins.

Claude never changes some text: direct quotes, code, variable names and commands. Legal, medical and technical terms also stay when precision matters more than simplicity.

If you ask for a different tone, format or length, Claude follows your request over the rules. Claude never removes a warning, caveat or correction to meet a rule.

## Add, change or remove a rule

Edit the rule's section in `SKILL.md`. Put a rule in Core rules only if it should shape every reply. Keep that section short, because each rule there slows every reply. Put all other rules in their category. To add a category, add a `##` section. Give the section a one-sentence description and a `### Rules` list. If needed, add a `### Never use` list as well.

Write specific rules that someone can check, not adjectives. For example, "No sentence exceeds 20 words" works, but "Sounds confident" does not. Start a rule with "When…" if it applies only to some kinds of text.

Then rebuild the output style.

## The output style

An output style cannot read other files, so `output-style/plain-english.md` holds a copy of the core rules. The build joins `output-style/header.md`, which holds the style's frontmatter, to the part of `SKILL.md` above the marker line. Do not edit the copy by hand. After any change to `SKILL.md`, run this command from this folder to rebuild it:

```
{ cat output-style/header.md; awk 'n>=2 && /output style ends here/{exit} n>=2; /^---$/{n++}' SKILL.md; } > output-style/plain-english.md
```
