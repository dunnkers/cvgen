---
name: cvgen-skill
description: "Generate professional CV/resume PDFs from scratch using RenderCV. Use when: (1) a user wants to create or generate a CV/resume, (2) a user provides their background info (via chat, uploaded files like PDFs/Word docs, or MCP context) and wants it turned into a polished CV, (3) a user asks to build, draft, or produce a CV/resume document. This skill handles the full pipeline: gathering context about the person, generating valid RenderCV YAML, and rendering it to PDF."
---

# CVGen

Generate professional CV PDFs using [RenderCV](https://github.com/rendercv/rendercv). Pipeline: gather context → generate YAML → render PDF.

## Workflow

### Step 1: Gather Context

Collect information about the person. Sources:

- **Chat**: Ask the user about their experience, education, skills, publications, etc.
- **Uploaded files**: Read PDFs, Word docs, or text files the user provides (existing CVs, LinkedIn exports, etc.)
- **MCP servers**: Use available MCP tools (e.g. Notion, Craft) to fetch relevant context if configured.

Ask clarifying questions as needed. Key information to collect:
- Name, location, contact details, social profiles
- Work experience (company, position, dates, highlights)
- Education (institution, area, degree, dates)
- Skills, publications, projects, awards — whatever is relevant

### Step 2: Generate the YAML

Create a `<Name>_CV.yaml` file (e.g. `John_Doe_CV.yaml`) following the RenderCV schema.

Read [references/schema-guide.md](references/schema-guide.md) for the human-readable schema overview. The full JSON Schema is at [references/schema.json](references/schema.json) — consult it when unsure about exact field names, types, enums, or validation rules.

Key rules:
- Each section must contain only one entry type
- Entry type is auto-detected from required fields (`company` + `position` = ExperienceEntry, `institution` + `area` = EducationEntry, etc.)
- Text supports markdown: `**bold**`, `*italic*`, `[links](url)`
- Dates: `"2023"`, `"2023-05"`, or `start_date`/`end_date` with `"present"` for ongoing
- Choose an appropriate theme: `classic`, `engineeringresumes`, `sb2nov`, `moderncv`, `engineeringclassic`

### Step 3: Render to PDF

```bash
uvx 'rendercv[full]' render <Name>_CV.yaml --output-folder-name <dist>
```

... where <dist> is the output directory. This can be `dist/`, for example:

```
$ ls -al dist
total 3464
drwxr-xr-x@ 13 dunnkers  staff      416 Feb 11 17:22 .
drwxr-xr-x  14 dunnkers  staff      448 Feb 11 19:33 ..
-rw-r--r--@  1 dunnkers  staff   231878 Jan 28 17:10 John_Doe_CV_1.png
-rw-r--r--@  1 dunnkers  staff     2964 Jan 28 17:10 John_Doe_CV.html
-rw-r--r--@  1 dunnkers  staff      736 Jan 28 17:10 John_Doe_CV.md
-rw-r--r--@  1 dunnkers  staff    34615 Jan 28 17:10 John_Doe_CV.pdf
-rw-r--r--@  1 dunnkers  staff    15888 Jan 28 17:10 John_Doe_CV.typ
```

... with the PDF file also there. If rendering fails, read the error message — RenderCV gives precise validation errors with exact locations.

After successful render, inform the user where the PDF is so they can view it.

### Step 4: Iterate

The user may want to adjust content, styling, or layout. Edit the YAML and re-render. Common adjustments:
- Reword highlights/bullet points
- Reorder sections
- Switch theme or tweak design (colors, margins, fonts)
- Add/remove sections
