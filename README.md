# GESIS PSRT Conference Program Template (Quarto > pandoc > LaTeX)

A simple Quarto template for generating a conference program PDF from two CSV files. Produces a one-page schedule overview followed by abstracts grouped by panel. Originally designed for the GESIS Political Science Roundtable.

## Usage

```bash
quarto render program.qmd
```

The output is `program.pdf` ([preview](program.pdf))

## Input files

### `data/schedule.csv`

The conference schedule. Each row is one schedule item.

| Column | Description | Values |
|--------|-------------|--------|
| `time` | Start time | e.g. `09:00` |
| `title` | Item title | For panels, use the format `Panel: Panel Name` (prefix is configurable) |
| `type` | Item type | `panel`, `break`, or `event` |
| `date` | (optional) Date | Only needed for multi-day conferences. If present with more than one unique value, date headers are shown in the schedule and panel headings include the date. |

Panels are linked to papers via the panel name (title without the `Panel: ` prefix).

### `data/papers.csv`

All papers/presentations. Row order within a panel determines presentation order.

| Column | Description |
|--------|-------------|
| `panel` | Panel name, must match a panel title in `schedule.csv` (without the `Panel: ` prefix) |
| `author` | Author or presenter name |
| `title` | Paper title |
| `type` | Paper type (free text, e.g. `paper`, `presentation`, `lightning`) |
| `abstract` | Abstract text |

## Configuration

All configuration lives in the YAML frontmatter of `program.qmd`.

### Standard Quarto fields

| Field | Description |
|-------|-------------|
| `title` | Conference title |
| `subtitle` | Subtitle (e.g. "Conference Program") |
| `date` | Conference date |
| `date-format` | Date display format (e.g. `long` for "June 15, 2026") |

### Parameters (`params`)

| Parameter | Default | Description |
|-----------|---------|-------------|
| `venue` | | Venue name, shown below the title on page 1 |
| `schedule_csv` | `data/schedule.csv` | Path to the schedule CSV |
| `papers_csv` | `data/papers.csv` | Path to the papers CSV |
| `panel_prefix` | `Panel: ` | Prefix stripped from schedule panel titles when matching to `papers.csv` |
| `panel_clearpage` | `true` | Start each panel's abstracts on a new page |
| `panel_show_time` | `true` | Show the time range next to panel headings in the abstracts section |
| `type_icons` | `{paper: "P", presentation: "T", lightning: "L"}` | Mapping of paper types to icon letters shown in the schedule and abstracts. Add or change entries to match the types in your `papers.csv`. |

## Styling

Visual styling is controlled in `_preamble.tex`, including

- **Page geometry**: `\usepackage[a4paper, margin=2cm]{geometry}`
- **Running header text**: `\fancyhead[L]{...}` and `\fancyhead[R]{...}`
- **Link colors**: `\hypersetup{linkcolor=..., urlcolor=...}`
- **Schedule table row spacing**: `\renewcommand{\arraystretch}{1.4}` (set in `program.qmd`)
- **Abstract entry spacing**: adjust values in `\paperentry`, `\paperseparator`, `\panelsection`
- **Type icon style**: `\typeicon` command controls how icon letters are rendered

