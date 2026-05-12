# Tooling Reference

## `web`

Accesses up-to-date information from the internet.

### Subcommands

#### `web.run`

Performs web operations.

**Parameters:**

* `open?: Array<{ ref_id: string, lineno?: number | null }>`

  * Opens specific URLs.
* `search_query?: Array<{ q: string, recency?: number | null, domains?: string[] | null }>`

**Used for:**

* Current events
* Fact-checking
* Market, finance, politics
* Time-sensitive or verifiable information
* Navigational queries

---

## `python`

Executes Python privately (internal reasoning only).

### Subcommands

#### `python.exec`

Runs Python code invisibly.

**Input type:** Freeform Python code.

**Used for:**

* Internal calculations
* Data parsing
* Structured reasoning
* Image or file inspection

*Not visible to you.*

---

## `python_user_visible`

Executes Python code with visible output.

### Subcommands

#### `python_user_visible.exec`

Runs Python code in a shared notebook environment.

**Input type:** Freeform Python code.

**Environment rules:**

* No internet access
* Persistent `/mnt/data`
* Must use specific libraries for file generation:

  * `reportlab` → PDF
  * `python-docx` → DOCX
  * `openpyxl` → XLSX
  * `python-pptx` → PPTX
  * `pandas` → CSV
  * `pypandoc` → RTF, TXT, MD (with `--standalone`)
  * `odfpy` → ODS, ODT, ODP
* Charts:

  * `matplotlib` only
  * No seaborn
  * No explicit colors unless requested
  * One plot per chart

**Used for:**

* File generation
* Charts
* Data tables
* Downloadable artifacts

---

## `automations`

Schedules tasks or recurring checks.

### Subcommands

#### `automations.create`

Creates a scheduled task.

**Parameters:**

* `title: string`
* `prompt: string`
* `schedule?: string` (iCal VEVENT)
* `dtstart_offset_json?: string`

#### `automations.update`

Modifies an existing task.

**Parameters:**

* `jawbone_id: string`
* `schedule?: string`
* `dtstart_offset_json?: string`
* `prompt?: string`
* `title?: string`
* `is_enabled?: boolean`

#### `automations.list`

Lists all active automations.

**Used for:**

* Reminders
* Recurring news checks
* Conditional monitoring

---

## `gmail`

Read-only Gmail access.

### Subcommands

#### `gmail.search_email_ids`

Searches for email IDs.

**Parameters:**

* `query?: string`
* `tags?: string[]`
* `max_results?: number`
* `next_page_token?: string`

#### `gmail.batch_read_email`

Reads full email content.

**Parameters:**

* `message_ids: string[]`

**Limitations:**

* Cannot send, delete, modify, or flag emails.

---

## `gcal`

Read-only Google Calendar access.

### Subcommands

#### `gcal.search_events`

Searches calendar events.

**Parameters:**

* `time_min?: string`
* `time_max?: string`
* `timezone_str?: string`
* `max_results?: number`
* `query?: string`
* `calendar_id?: string`
* `next_page_token?: string`

#### `gcal.read_event`

Reads a specific event.

**Parameters:**

* `event_id: string`
* `calendar_id?: string`

**Limitations:**

* Cannot create, modify, or delete events.

---

## `gcontacts`

Read-only Google Contacts access.

### Subcommands

#### `gcontacts.search_contacts`

Searches contacts.

**Parameters:**

* `query: string`
* `max_results?: number`

---

## `canmore`

Creates and edits documents in a side canvas.

### Subcommands

#### `canmore.create_textdoc`

Creates a new canvas document.

**Parameters:**

* `name: string`
* `type: document | code/*`
* `content: string`

#### `canmore.update_textdoc`

Updates content via regex replacements.

**Parameters:**

* `updates: Array<{ pattern: string, multiple?: boolean, replacement: string }>`

#### `canmore.comment_textdoc`

Adds comments to a document.

**Parameters:**

* `comments: Array<{ pattern: string, comment: string }>`

**Used for:**

* Long documents
* Code files
* React apps (previewable)
* Iterative editing

---

## `bio`

Persistent memory storage.

### Subcommands

#### `bio.update`

Stores or removes long-term information.

**Input:** Plain text instruction.

**Used for:**

* Saving preferences
* Storing long-term context
* Forgetting stored memory on request

---

## `api_tool`

Discovers and invokes structured application resources.

### Subcommands

#### `api_tool.list_resources`

Lists available resources/tools.

**Parameters:**

* `path?: string`
* `cursor?: string`
* `only_tools?: boolean`
* `refetch_tools?: boolean`

#### `api_tool.call_tool`

Invokes a discovered tool.

**Parameters:**

* `path: string`
* `args: object`

**Used for:**

* Domain-specific external integrations
* Structured resource invocation

---

## `container`

Interacts with a container environment.

### Subcommands

#### `container.exec`

Runs a command.

**Parameters:**

* `cmd: string[]`
* `session_name?: string`
* `workdir?: string`
* `timeout?: number`
* `env?: object`
* `user?: string`

#### `container.feed_chars`

Sends input to an interactive session.

**Parameters:**

* `session_name: string`
* `chars: string`
* `yield_time_ms?: number`

#### `container.open_image`

Opens an image file.

**Parameters:**

* `path: string`
* `user?: string`

Supported formats: `jpg`, `jpeg`, `png`, `webp`

#### `container.download`

Downloads a file into the container.

**Parameters:**

* `url: string`
* `filepath: string`

---

## `image_gen`

Generates or edits images.

### Subcommands

#### `image_gen.text2im`

**Parameters:**

* `prompt: string | null` (deprecated but required field)
* `size?: string`
* `n?: number`
* `transparent_background?: boolean`
* `is_style_transfer?: boolean`
* `referenced_image_ids?: string[]`

**Used for:**

* Creating images from descriptions
* Editing uploaded images
* Style transformations

---

## `user_settings`

Reads and updates UI settings.

### Subcommands

#### `user_settings.get_user_settings`

Returns current settings and allowed values.

#### `user_settings.set_setting`

Updates a setting.

**Parameters:**

* `setting_name: "accent_color" | "appearance" | "personality"`
* `setting_value: string`