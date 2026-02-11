# RenderCV YAML Schema Guide

The full JSON Schema is at [schema.json](schema.json) in this directory. Consult it for exact field definitions, enums, and validation rules. This guide provides a human-readable overview.

## Top-Level Structure

```yaml
cv:          # (required) CV content
design:      # (optional) visual styling
locale:      # (optional) language strings
settings:    # (optional) rendercv behavior
```

## `cv` Field

### Header Fields (all optional)

| Field | Type | Notes |
|-------|------|-------|
| `name` | string | Full name |
| `headline` | string | e.g. "Software Engineer" |
| `location` | string | e.g. "New York, NY" |
| `email` | string or list | One or multiple emails |
| `phone` | string or list | Phone number(s) |
| `website` | string or list | URL(s) |
| `photo` | string | File path to photo |
| `social_networks` | list | `network` + `username` |
| `custom_connections` | list | `name` + `url` + Font Awesome `icon` |

### Supported Social Networks

LinkedIn, GitHub, GitLab, ORCID, Google Scholar, X, Bluesky, Instagram, Mastodon, YouTube, StackOverflow, ResearchGate, Telegram, WhatsApp, Leetcode, IMDB

### Sections

Keys are section titles, values are lists of entries. **Each section must contain only one entry type.**

## Entry Types

### EducationEntry
Required: `institution`, `area`
Optional: `degree`, `date`, `start_date`, `end_date`, `location`, `summary`, `highlights`

### ExperienceEntry
Required: `company`, `position`
Optional: `date`, `start_date`, `end_date`, `location`, `summary`, `highlights`

### NormalEntry
Required: `name`
Optional: `date`, `start_date`, `end_date`, `location`, `summary`, `highlights`

### PublicationEntry
Required: `title`, `authors`
Optional: `date`, `doi`, `url`, `journal`, `summary`

### OneLineEntry
Required: `label`, `details`

### BulletEntry
Required: `bullet`

### NumberedEntry / ReversedNumberedEntry
Required: `number` / `reversed_number`

### TextEntry
Just a plain string in the list.

### Date Formats
- Single: `"2023"`, `"2023-05"`, `"June 2023"`
- Range: `start_date` + `end_date` (use `"present"` for ongoing)

### Text Formatting
Markdown: `**bold**`, `*italic*`, `[links](url)`, `` `code` ``
Typst: `$$math$$`, `#emph[text]`

## `design` Field

### Themes
`classic` (default), `engineeringresumes`, `sb2nov`, `moderncv`, `engineeringclassic`

```yaml
design:
  theme: classic
```

Subfields per theme: `page`, `colors`, `typography`, `links`, `header`, `section_titles`, `entries`, `templates`.

### Page Options
`size`: a4, a5, us-letter, us-executive
`top_margin`, `bottom_margin`, `left_margin`, `right_margin`: e.g. "2cm"

### Colors
`body`, `name`, `headline`, `connections`, `section_titles`, `links`, `footer`, `top_note`

### Section Title Types
`with_partial_line`, `with_full_line`, `without_line`, `moderncv`

## `locale` Field

Languages: en, da, nl, fr, de, hi, id, it, ja, ko, zh, pt, ru, es, tr

```yaml
locale:
  language: en
```

## `settings` Field

```yaml
settings:
  current_date: "2025-01-15"
  bold_keywords:
    - Python
    - Machine Learning
  render_command:
    dont_generate_html: true
    dont_generate_png: true
    dont_generate_markdown: true
```
