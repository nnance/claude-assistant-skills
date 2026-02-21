# Claude Assistant Skills Registry

An optional skills registry for [claude-assistant](https://github.com/nnance/claude-assistant). These skills extend the assistant with integrations for Apple services, Google Workspace, and personal knowledge management. Install only what you need.

## Available Skills

| Skill | Description | Trigger Phrases |
|-------|-------------|-----------------|
| [apple-services](#apple-services) | macOS Calendar, Contacts, Notes | "calendar", "schedule", "contacts", "notes" |
| [google-workspace](#google-workspace) | Gmail, Drive, Docs, Sheets | "email", "Google Doc", "spreadsheet", "Drive" |
| [vault](#vault) | A Second Brain built on Obsidian | "save a note", "add to vault", "show my tasks" |

## Installation

Use the built-in `skill-registry` skill in your assistant to browse and install skills:

```
# List available skills
"what skills are available in the registry?"

# Install a skill
"install the apple-services skill"

# Update an installed skill
"update the vault skill"
```

Or install manually by copying a skill directory into your `.claude/skills/` folder:

```bash
gh api repos/nnance/claude-assistant-skills/git/trees/main?recursive=1 \
  --jq '.tree[] | select(.path | startswith("skills/apple-services/")) | .path'
```

---

## Skills

### apple-services

**Location**: `skills/apple-services/`

**Description**: Manages Apple Calendar, Contacts, and Notes on macOS via shell scripts. Uses a native Swift CLI (`apple-services`) for Calendar and Contacts with shell script fallbacks.

**Triggers**:
- Calendar: "what's on my calendar", "schedule a meeting", "upcoming events"
- Contacts: "find contact", "phone number for", "email for"
- Notes: "personal information", "professional information", mentions of Apple Notes

**Capabilities**:
- List, search, create, and delete calendar events
- Search, list, get, and create contacts
- Read, search, create, and edit Apple Notes

**Scripts**:
- `scripts/calendar.sh` - Calendar operations via AppleScript
- `scripts/contacts.sh` - Contacts operations via AppleScript
- `scripts/notes.sh` - Notes operations via AppleScript

**Configuration**: Optional `APPLE_CALENDAR_NAME` environment variable sets the default calendar (defaults to "Calendar").

---

### google-workspace

**Location**: `skills/google-workspace/`

**Description**: Accesses Gmail, Google Drive, Google Docs, and Google Sheets via the `gog` (gogcli) CLI.

**Triggers**:
- "email", "Gmail", "send email", "check my inbox"
- "Google Doc", "create a doc", "read a document"
- "spreadsheet", "Google Sheets", "update the sheet"
- "Google Drive", "search my drive", "upload file"

**Capabilities**:
- Search, read, send, and reply to Gmail messages
- Browse, upload, download, and share Google Drive files
- Create, read, and edit Google Docs
- Create, read, and write Google Sheets

**Prerequisites**:
1. Install gogcli: `brew install steipete/tap/gogcli`
2. Authenticate: `gog auth add <your-email>`

---

### vault

**Location**: `skills/vault/`

**Description**: A Second Brain built on Obsidian. Captures, organizes, and retrieves notes with automatic folder classification and a daily log system.

**Triggers**:
- "save a note", "capture this", "remember that"
- "add to vault", "list notes", "read note"
- "find in vault", "show my tasks", "log this"
- "mark task complete", "I finished", "task is done"

**Capabilities**:
- Create notes with automatic classification (Tasks, Ideas, Reference, Projects, Inbox)
- Search and retrieve notes by content or tags
- Update existing notes in place (for task completions)
- Maintain daily interaction logs

**Configuration**: Requires `$VAULT_PATH` environment variable pointing to your Obsidian vault directory.

**Reference Files**:
- `references/tags.md` - Tag system documentation
- `references/note-format.md` - Note formatting standards
- `references/examples.md` - Usage examples

---

## Adding Skills

Contributions welcome! To add a skill:

1. Create a directory under `skills/<skill-name>/`
2. Add a `SKILL.md` file with YAML frontmatter (`name`, `description`) and markdown instructions
3. Follow [Anthropic's skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
4. Update this README with the new skill details
5. Open a pull request
