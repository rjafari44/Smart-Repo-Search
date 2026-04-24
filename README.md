# Smart Repository Search Script

An intelligent shell script that searches through your GitHub repositories with smart scoring and relevance ranking.

## Features

- **Smart Scoring Algorithm**: Ranks results based on:
  - Exact filename matches (highest priority)
  - Partial filename matches
  - Directory name matches
  - Full path matches
  - Fuzzy matching
  - Multiple keyword matches in same path (bonus)
  - File content matching with keyword frequency analysis
  - Depth penalty (prefers higher-level matches)

- **Content Search**: Searches inside text files for keyword matches
- **Visual Output**: Color-coded results with icons for different file types
- **Configurable**: Adjust search depth and result count
- **Fast**: Excludes common ignored directories (node_modules, .git, venv, etc.)

## Usage

### Interactive Mode
Simply run the script without arguments:
```bash
./smart-repo-search.sh
```

You'll be prompted for:
- Directory to search (defaults to current directory)
- Keywords to search for (space-separated)
- Max search depth (optional)
- Max results to show (optional)

### Command Line Mode
Provide keywords as arguments:
```bash
./smart-repo-search.sh keyword1 keyword2 keyword3
```

This searches the current directory for the provided keywords.

## Examples

### Example 1: Finding a React project
```bash
$ ./smart-repo-search.sh react dashboard
```
This will find all files/directories containing "react" and "dashboard", prioritizing:
- Exact matches like `react-dashboard/`
- Files with both keywords
- Recent or top-level matches

### Example 2: Finding a specific config file
```bash
$ ./smart-repo-search.sh webpack config
```
Will rank:
- `webpack.config.js` (exact match in filename - highest score)
- `config/webpack.js` (both keywords in path)
- Files containing webpack configuration code

### Example 3: Finding documentation
```bash
$ ./smart-repo-search.sh api documentation
```
Will find:
- `api-documentation.md`
- `docs/api/`
- Files containing API documentation

## Scoring System

The script uses a sophisticated scoring algorithm:

| Match Type | Score |
|------------|-------|
| Exact basename match | +100 |
| Basename contains keyword | +50 |
| Directory name contains keyword | +25 |
| Fuzzy match | +15 |
| Path contains keyword | +10 |
| Multiple keywords in same path | +20 per extra keyword |
| Content match (per occurrence) | +10 + 2 per occurrence |
| High frequency in file (>5 occurrences) | +10 bonus |
| Depth penalty | -2 per directory level |

## Configuration

You can modify these variables at the top of the script:

- `MIN_SCORE`: Minimum score to include in results (default: 0)
- `MAX_RESULTS`: Maximum number of results to display (default: 20)
- `SEARCH_DEPTH`: How deep to search in directory tree (default: 10)

## Excluded Directories

The script automatically excludes:
- Hidden directories (.*) 
- node_modules
- venv
- __pycache__
- dist
- build
- target

## Tips for Best Results

1. **Use specific keywords**: More specific keywords = better results
2. **Combine keywords**: Use 2-3 relevant keywords for best precision
3. **Use partial words**: The script handles partial matches well
4. **Try variations**: If you don't find it, try synonyms or related terms

## Installation

1. Download the script:
   ```bash
   curl -O https://your-repo/smart-repo-search.sh
   ```

2. Make it executable:
   ```bash
   chmod +x smart-repo-search.sh
   ```

3. (Optional) Add to your PATH:
   ```bash
   sudo mv smart-repo-search.sh /usr/local/bin/repo-search
   ```

## Sample Output

```
🔍 Searching for: react dashboard

📊 Top Results (sorted by relevance):

#1 [Score: 175]
  📁  projects/react-dashboard
     └─ Path match: 150 | Content match: 25

#2 [Score: 142]
  📜  apps/admin-dashboard/src/App.jsx
     └─ Path match: 85 | Content match: 57

#3 [Score: 98]
  📄  docs/react-components.md
     └─ Path match: 75 | Content match: 23

✨ Found 8 total matches, showing top 3
```

## Requirements

- Bash 4.0+
- Standard Unix utilities (find, grep, file, sort)
- Works on Linux, macOS, and WSL

## License

MIT