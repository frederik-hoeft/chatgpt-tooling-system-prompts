# Tooling Reference

## `web`

Accesses up-to-date information from the internet.

### Input Format

The tool input is a single UTF-8 text blob (not JSON, except for `genui_run`). The blob is a sequence of newline-separated records in pipe-delimited format: `<op>|<field1>|<field2>|...`

### Subcommands

#### Search: `fast` / `slow`

Two search engines are available:

* `fast|<q>|<recency?>|<domains?>` — primary, lower-cost search (maps to `system2_search_query`).
* `slow|<q>|<recency?>|<domains?>` — backup, higher-cost search (maps to `system1_search_query`). Use only when `fast` is insufficient.

> Note: `system2_search_query` may be marked out of service at times; in that case only `slow` is available.

#### `product`

Searches for physical products.

**Format:** `product|<search?>|<lookup?>`

* `search` and `lookup` are `;`-separated lists; at least one must be non-empty.

#### `business`

Searches for local businesses.

**Format:** `business|<location?>|<query?>|<lookup?>|<lat?>|<long?>|<lat_span?>|<long_span?>`

* `query` and `lookup` are `;`-separated lists; at least one must be non-empty.
* Set `location="user"` when the user is the reference point (e.g., "near me").
* Do not use `lat_span` / `long_span` unless explicitly requested.

#### `image`

Searches for images on the web.

**Format:** `image|<q>|<recency?>|<domains?>`

#### `genui_search` / `genui_run`

Interactive widget system.

* `genui_search|<query>` — searches for a widget.
* `genui_run|<widget_name>|<args_json?>` — runs a widget (args as JSON).

Widgets available for: sports, weather, currency, unit conversion, jobs, timers.

#### `open`

Opens a specific search result reference.

**Format:** `open|<ref_id>|<lineno?>`

**Used for:**

* Current events
* Fact-checking
* Market, finance, politics
* Time-sensitive or verifiable information
* Navigational queries
* Product and local business searches
* Image searches

**Safety restrictions (product search):**

* No firearms, explosives, drugs, alcohol, nicotine, or counterfeit goods.

---

## `python`

Executes Python privately (internal reasoning only). Runs in the `analysis` channel and is **not visible** to the user.

### Subcommands

#### `python.exec`

Runs Python code invisibly.

**Input type:** Freeform Python code.

**Environment rules:**

* No internet access
* Persistent `/mnt/data`
* 300-second timeout

**Used for:**

* Internal calculations
* Data parsing
* Structured reasoning
* Image or file inspection

---

## `python_user_visible`

Executes Python code with visible output. Runs in the `commentary` channel.

### Subcommands

#### `python_user_visible.exec`

Runs Python code in a shared notebook environment.

**Input type:** Freeform Python code.

**Environment rules:**

* No internet access
* Persistent `/mnt/data`
* 300-second timeout
* Must use specific libraries for file generation:

  * `reportlab` (prefer `reportlab.platypus`) → PDF
  * `python-docx` → DOCX
  * `openpyxl` → XLSX
  * `python-pptx` → PPTX
  * `pandas` → CSV
  * `pypandoc` (only `convert_text`, must include `extra_args=['--standalone']`) → RTF, TXT, MD
  * `odfpy` → ODS, ODT, ODP
* Charts:

  * `matplotlib` only
  * No seaborn
  * No explicit colors unless requested
  * One plot per chart
* Use `caas_jupyter_tools.display_dataframe_to_user()` for DataFrames
* Use `UnicodeCIDFont` for CJK languages in PDFs

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

Gmail access with read and write capabilities.

### Subcommands

#### `gmail.list_labels`

Lists labels with message counts.

**Parameters:**

* `label_names?: string[]`

#### `gmail.search_email_ids`

Searches for email IDs.

**Parameters:**

* `query?: string`
* `tags?: string[]`
* `max_results?: number`
* `next_page_token?: string`

#### `gmail.search_emails`

Searches for emails with full content.

**Parameters:**

* `query?: string`
* `tags?: string[]`
* `max_results?: number`
* `next_page_token?: string`

#### `gmail.batch_read_email`

Reads full email content by IDs.

**Parameters:**

* `message_ids: string[]`

#### `gmail.read_attachment`

Reads an email attachment.

**Parameters:**

* `message_id: string`
* `attachment_id?: string`
* `filename?: string`

#### `gmail.list_drafts`

Lists email drafts.

**Parameters:**

* `max_results?: number`
* `next_page_token?: string`

#### `gmail.read_email_thread`

Reads an email thread.

**Parameters:**

* `id: string`
* `id_type?: string`
* `max_messages?: number`

#### `gmail.send_email`

Sends an email immediately.

**Parameters:**

* `to: string`
* `subject: string`
* `body: string`
* `cc?: string`
* `bcc?: string`
* `reply_message_id?: string`

#### `gmail.create_draft`

Creates a draft for review.

**Parameters:**

* `to: string`
* `subject: string`
* `body: string`
* `cc?: string`
* `bcc?: string`
* `reply_message_id?: string`

#### `gmail.update_draft`

Updates an existing draft.

**Parameters:**

* `draft_id: string`
* `to?: string`
* `subject?: string`
* `body?: string`
* `cc?: string`
* `bcc?: string`

#### `gmail.send_draft`

Sends a saved draft.

**Parameters:**

* `draft_id: string`

#### `gmail.forward_emails`

Forwards existing emails to another recipient.

**Parameters:**

* `message_ids: string[]`
* `to: string`
* `cc?: string`
* `bcc?: string`
* `note?: string`

#### `gmail.archive_emails`

Archives emails (removes from inbox, keeps in Gmail).

**Parameters:**

* `message_ids: string[]`

#### `gmail.delete_emails`

Moves emails to Trash.

**Parameters:**

* `message_ids: string[]`

#### `gmail.create_label`

Creates a new label.

**Parameters:**

* `name: string`
* `message_list_visibility?: string`
* `label_list_visibility?: string`

#### `gmail.apply_labels_to_emails`

Adds or removes labels by name.

**Parameters:**

* `message_ids: string[]`
* `add_label_names?: string[]`
* `remove_label_names?: string[]`
* `create_missing_labels?: boolean`

#### `gmail.bulk_label_matching_emails`

Labels all emails matching a search query in one step.

**Parameters:**

* `query: string`
* `label_name: string`
* `create_label_if_missing?: boolean`
* `archive?: boolean`

#### `gmail.batch_modify_email`

Modifies labels by raw Gmail label IDs.

**Parameters:**

* `message_ids: string[]`
* `add_labels?: string[]`
* `remove_labels?: string[]`

---

## `gcal`

Google Calendar access with read and write capabilities.

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

#### `gcal.get_colors`

Returns calendar and event color palettes.

#### `gcal.create_event`

Creates a new calendar event.

**Parameters:**

* `title: string`
* `start_time: string`
* `end_time: string`
* `attendees: string[]`
* `timezone_str?: string`
* `description?: string`
* `location?: string`
* `color_id?: string`
* `recurrence?: string[]`
* `reminders?: { use_default: boolean, overrides?: Array<{ method: string, minutes: number }> }`
* `visibility?: string`
* `transparency?: string`
* `event_type?: string`
* `auto_decline_mode?: string`
* `decline_message?: string`
* `chat_status?: string`
* `self_attendance?: string`
* `add_google_meet?: boolean`

#### `gcal.update_event`

Updates an existing event.

**Parameters:**

* `event_id: string`
* `title?: string`
* `start_time?: string`
* `end_time?: string`
* `timezone_str?: string`
* `description?: string`
* `location?: string`
* `color_id?: string`
* `reminders?: { use_default: boolean, overrides?: Array<{ method: string, minutes: number }> }`
* `visibility?: string`
* `transparency?: string`
* `attendees_to_add?: string[]`
* `attendees_to_remove?: string[]`
* `update_scope?: string`
* `recurrence?: string[]`
* `event_type?: string`
* `auto_decline_mode?: string`
* `decline_message?: string`
* `chat_status?: string`
* `add_google_meet?: boolean`

#### `gcal.respond_event`

Responds to a calendar invitation.

**Parameters:**

* `event_id: string`
* `response_status: string`
* `reason?: string`
* `notify?: boolean`

#### `gcal.delete_event`

Deletes a calendar event.

**Parameters:**

* `event_id: string`

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
* `type: "document" | "code/<language>"` (e.g., `code/python`, `code/react`, `code/html`, etc.)
* `content: string`

Supported code types: `bash`, `zsh`, `javascript`, `typescript`, `html`, `css`, `python`, `json`, `sql`, `go`, `yaml`, `java`, `rust`, `cpp`, `swift`, `php`, `xml`, `ruby`, `haskell`, `kotlin`, `csharp`, `c`, `objectivec`, `r`, `lua`, `dart`, `scala`, `perl`, `commonlisp`, `clojure`, `ocaml`, `powershell`, `verilog`, `dockerfile`, `vue`, `react`, `other`.

Types `code/react` and `code/html` can be previewed. Default to `code/react` for apps/games/websites.

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

## `personal_context`

Retrieves user-specific personal context from linked accounts, prior interactions, and other personal context streams. Runs in the `analysis` channel.

### Subcommands

#### `personal_context.search`

Searches across personal context sources.

**Parameters:**

* `query: string` (must be entirely self-contained — no access to current conversation)

**Used for:**

* Recalling previous personal details
* Continuing or updating prior workflows, plans, or projects
* Referencing earlier preferences or constraints

> Note: This tool may be disabled; when disabled, the user is directed to enable memory in Settings > Personalization > Memory.

---

## `file_search`

Searches and views files uploaded in the current conversation. Runs in the `analysis` channel.

### Subcommands

#### `file_search.msearch`

Searches across uploaded files.

**Parameters:**

* `queries?: string[]`

#### `file_search.mclick`

Expands uploaded-file search results previously returned by `msearch`.

**Parameters:**

* `pointers?: string[]`

**Used for:**

* Searching uploaded file content
* Expanding and viewing file search results

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

* Domain-specific external integrations (e.g., GitHub, etc.)
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
* `yield_time_ms?: number` (default: 100)

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

* `prompt?: string | null`
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
