# Smart Repository Search Script

An intelligent shell-based repository search tool that scans your codebase and ranks files and folders by relevance using a hybrid of path-based scoring and content matching.

It is designed for fast exploratory navigation of unknown or large repositories, rather than exact text search.

## Features

### Hybrid Relevance Engine
Ranks results using multiple signals:

- Filename matches (highest priority)
- Directory name matches
- Full path matches
- Multi-keyword overlap bonuses
- File content keyword matching
- Depth penalty (prefers higher-level results)

### Content Search (inside files)
- Searches inside text-based files only
- Uses keyword occurrence counting
- Adds score based on frequency
- Displays content score even when it is 0

Example:
└─ Path match: 31 | Content match: 0

### Rich terminal output
- Color-coded scoring tiers
- File-type icons:
  - 📁 Directory
  - 📄 Text / documentation files
  - 📜 JavaScript / TypeScript
  - 🐍 Python
  - 🔧 Shell scripts
  - ⚙️ Config files (JSON/YAML/TOML)
  - 🎨 Web files (HTML/CSS)
  - ☕ Java
  - 🔷 Go
  - 🦀 Rust
  - ❓ Unknown types
- Ranked results with relevance scores

### Noise filtering
Automatically excludes:
- node_modules
- venv
- __pycache__
- dist / build / target
- hidden files (.*)
- the script itself (prevents self-matching results)

## Usage

### Interactive Mode
```bash
./rfind.sh
```

Prompts for:
- search directory
- keywords
- search depth
- max results

### Command Line Mode
```bash
./rfind.sh keyword1 keyword2 keyword3
```

Example:
```bash
./rfind.sh radio rf astronomy
```

## Example Output

🔍 Searching for: radio rf astronomy

📊 Top Results (sorted by relevance):

#1 [Score: 192]
  🎨 rjafari44.github.io/projects/radio_astronomy/small-rt-21cm.html
     └─ Path match: 102 | Content match: 90

#2 [Score: 184]
  📁 rjafari44.github.io/projects/radio_astronomy
     └─ Path match: 184 | Content match: 0

#3 [Score: 73]
  📁 Radio_Telescope
     └─ Path match: 73 | Content match: 0

#4 [Score: 69]
  📄 Inventory-Management-System/src/perform_transaction.cpp
     └─ Path match: 69 | Content match: 0

✨ Found 28 total matches, showing top 20

## Scoring System

### Path scoring
| Match Type | Score |
|------------|-------|
| Exact filename match | +100 |
| Filename contains keyword | +50 |
| Directory contains keyword | +25 |
| Full path contains keyword | +10 |
| Fuzzy match (basename) | +15 |

### Multi-keyword bonus
- +20 per additional keyword matched in same path

### Content scoring
| Condition | Score |
|----------|-------|
| Each match occurrence | +10 |
| Per repetition bonus | +2 |
| High frequency (>5 matches) | +10 bonus |

### Structural penalty
- -2 per directory depth level