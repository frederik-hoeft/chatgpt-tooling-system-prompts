# Assistant Tool Reference (Current Session)

This is a practical reference of the tools I can use in this chat: what each tool does, its subcommands/functions, key parameters, and when I should use it.

---

## 1) Web (`web.run`)
Used for live internet access, current info, local businesses, products, images, widgets (weather/sports/jobs/etc.).

### `slow|query|recency?|domains?`
High-quality web search (slower, more expensive).

**Parameters:**
- `query` *(required)* — search query
- `recency` *(optional)* — days back (e.g. `1`, `7`, `30`)
- `domains` *(optional)* — restrict domains (`nytimes.com;github.com`)

**Use when:**
- Accuracy matters
- Time-sensitive info
- Legal/medical/financial verification

---

### `fast|query|recency?|domains?`
Faster search (normally preferred).

Same parameters as `slow`.

**Use when:**
- General web lookup
- Recent news
- Quick fact checks

---

### `product|search?|lookup?`
Physical product search.

**Parameters:**
- `search` — category/product query
- `lookup` — exact product name

**Use when:**
- Buying advice
- Product comparisons
- Shopping recommendations

---

### `business|location?|query?|lookup?|lat?|long?|lat_span?|long_span?`
Local business search.

**Parameters:**
- `location` — city OR `user`
- `query` — search terms
- `lookup` — exact business names
- optional coordinates

**Important:** If user says *near me*, I must use `location=user`.

---

### `image|query|recency?|domains?`
Searches for real-world images.

**Use when:**
- Visual references
- Places
- real objects

---

### `genui_search|query`
Searches available widgets.

**Use when:**
- Weather
- Sports
- Jobs
- Currency conversion
- Timers
- Utilities

---

### `genui_run|widget_name|args_json?`
Runs a widget returned by `genui_search`.

---

### `open|ref_id|lineno?`
Open a previously returned source.

---

# 2) Python (`python.exec`)
Private computation tool.

**Use when:**
- Math
- Parsing
- Data analysis
- Internal reasoning
- File inspection

**Not visible to you.**

---

# 3) Automations
Schedules future reminders/searches.

## `create`
Creates scheduled tasks.

**Parameters:**
- `title`
- `prompt`
- `schedule`
- OR `dtstart_offset_json`

---

## `update`
Modify existing automation.

**Parameters:**
- `jawbone_id`
- `schedule`
- `prompt`
- `title`
- `is_enabled`

---

## `list`
Lists current automations.

---

# 4) File Search
Searches uploaded files only.

## `msearch`
Search uploaded files.

**Parameters:**
- `queries[]`

---

## `mclick`
Open file search results.

**Parameters:**
- `pointers[]`

---

# 5) Gmail
Email access + actions.

## Read/Search
- `list_labels`
- `search_email_ids`
- `search_emails`
- `batch_read_email`
- `read_attachment`
- `read_email_thread`

---

## Draft/Send
- `create_draft`
- `update_draft`
- `send_draft`
- `send_email`
- `forward_emails`

---

## Organization
- `archive_emails`
- `delete_emails`
- `create_label`
- `apply_labels_to_emails`
- `bulk_label_matching_emails`
- `batch_modify_email`

---

# 6) Google Calendar (`gcal`)
Calendar search + modification.

## Read
- `search_events`
- `read_event`
- `get_colors`

---

## Write
- `create_event`
- `update_event`
- `respond_event`
- `delete_event`

### Common Parameters
- `title`
- `start_time`
- `end_time`
- `attendees`
- `location`
- `description`
- `reminders`
- `recurrence`

---

# 7) Google Contacts (`gcontacts`)
Read-only contact lookup.

## `search_contacts`

**Parameters:**
- `query`
- `max_results`

---

# 8) Canvas (`canmore`)
Creates editable documents/code files beside chat.

## `create_textdoc`
Creates document.

**Parameters:**
- `name`
- `type`
- `content`

---

## `update_textdoc`
Regex-based edits.

**Parameters:**
- `updates[]`
  - `pattern`
  - `replacement`
  - `multiple`

---

## `comment_textdoc`
Adds comments.

**Parameters:**
- `comments[]`
  - `pattern`
  - `comment`

---

# 9) Python (User Visible)
Runs code you can directly see.

**Use when:**
- Charts
- Tables
- CSVs
- PDFs
- DOCX
- PPTX
- Data exports

Creates downloadable files.

---

# 10) Container
Shell/container access.

## `exec`
Run shell commands.

Parameters:
- `cmd[]`
- `workdir`
- `timeout`
- `env`

---

## `feed_chars`
Send input to active shell session.

---

## `open_image`
Open generated image files.

---

## `download`
Download remote file.

---

# 11) Personal Context
Currently disabled.

Would normally search memory/personal context.

---

# 12) Memory (`bio`)
Stores persistent preferences.

## `update`
Save or forget long-term preferences.

---

# 13) API Tool
Explores connected external resources.

## `list_resources`
Browse available APIs/resources.

---

## `call_tool`
Invoke discovered external tool.

---

# 14) Image Generation (`image_gen`)
Generate/edit images.

## `text2im`

**Parameters:**
- `prompt`
- `size`
- `n`
- `transparent_background`
- `is_style_transfer`
- `referenced_image_ids`

---

# 15) User Settings
Reads/changes ChatGPT UI settings.

## `get_user_settings`
View settings.

---

## `set_setting`
Modify settings.

**Parameters:**
- `setting_name`
  - `accent_color`
  - `appearance`
  - `personality`
- `setting_value`

---

# UI Features I Can Also Use
These are not tools, but response formatting features:

- Entity references
- Business cards
- Product carousels
- Image groups
- News nav lists
- URL citations
- Writing blocks

---

# Practical Rules I Follow

### I MUST use tools for:
- Current events
- Live pricing
- Local recommendations
- Travel details
- shopping
- recent company/person updates

### I should NOT use tools for:
- Basic reasoning
- Writing help
- General coding advice
- Simple explanations

---

# Fastest Path for You
If you want me to move quickly, explicitly say things like:

- “search the web”
- “check my calendar”
- “draft this email”
- “make this into a document”
- “generate an image”
- “run the numbers”

That removes ambiguity and usually gets you a better result.

