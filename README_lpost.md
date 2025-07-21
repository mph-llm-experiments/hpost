# lpost - lmno.md Post Creator

A command-line tool for creating and managing posts in lmno.md format - a monolithic markdown file where all posts are stored in a single file.

## Features

- Create new posts with automatic date stamping (prepended to file)
- List recent posts with user-friendly index numbers
- Edit specific posts (opens editor at correct line position)
- Preview individual posts with markdown viewers (glow, mdless, bat, etc.)
- Filter post lists by keyword (searches titles and content)
- Daily notes format
- **Publish to lmno.lol** with authentication and confirmation
- Cross-platform file opening support
- Configurable via environment variables or config file
- Support for various editors with line positioning

## Installation

1. Make the script executable:

   ```bash
   chmod +x lpost
   ```

2. (Optional) Add to your PATH or create an alias

## Configuration

### Method 1: Environment Variable

```bash
export LPOST_FILE="~/your-blog.md"
```

### Method 2: Configuration File

Create `~/.config/lpost/config.yml`:

```yaml
posts_file: ~/lmno.md
editor: hx # or vim, emacs, code, subl, etc.
preview: glow # or mdless, bat, default, etc.

# lmno.lol publishing configuration (optional)
lmno:
  email: your-email@example.com
  api_key: your-api-key-here
  blog_name: your-blog-name
  url: https://lmno.lol
```

## Usage

### Create a new post

```bash
# Basic usage
lpost -t "My New Post Title"

# Daily notes
lpost -d
```

Posts are automatically prepended to the top of your lmno.md file with the format:
```markdown
# [2025-07-19 Sat] My New Post Title


[existing content...]
```

### List recent posts

```bash
# List 10 most recent posts (default, sorted by date)
lpost list

# List 20 most recent posts
lpost list 20

# Filter by keyword in title or content
lpost list --keyword hugo
lpost list 20 --keyword programming
```

**Note**: Keyword filters persist! When you use `--keyword` with list, subsequent edit/preview commands remember that filter.

### Edit existing post

```bash
# First list posts to see IDs
lpost list

# Edit post by ID - opens editor at the correct line
lpost edit 3

# Edit when using keyword filtering
lpost list --keyword ruby
lpost edit 2  # Edits 2nd post matching "ruby"
```

### Preview posts

```bash
# Preview a post using configured tool (glow, mdless, bat, etc.)
lpost preview 1

# Preview from filtered results
lpost list --keyword journal
lpost preview 2  # Previews 2nd journal-related post
```

### Publish to lmno.lol

**Requirements**: Configure the `lmno` section in your config file with your credentials.

```bash
# Publish your entire blog to lmno.lol
lpost --publish
```

This will:
1. Show file size and destination blog
2. Require explicit Y/n confirmation  
3. Authenticate with lmno.lol using your credentials
4. Upload your entire posts file as the blog content
5. Provide the blog URL upon success

**Security**: Your API key is stored in your local config file. The publish command requires manual confirmation each time.

## Options

- `-t, --title TITLE` - Post title (required for create)
- `-d, --daily` - Create a daily notes post
- `-k, --keyword KEYWORD` - Filter list by keyword in title or content
- `--clear-filters` - Clear saved filter state
- `--publish` - Publish blog to lmno.lol (requires Y/n confirmation)
- `-h, --help` - Show help message

## Editor Support

lpost automatically detects your editor and opens files at the correct line number:

- **Vim/Neovim**: `vim +304 file.md`
- **Helix**: `hx file.md:304`
- **VS Code**: `code -g file.md:304`
- **Sublime Text**: `subl file.md:304`
- **Emacs**: `emacs -nw +304 file.md`
- **Emacs Client**: `emacsclient -nw +304 file.md`
- **Nano**: `nano +304 file.md`
- **Micro**: `micro +304 file.md`
- **Atom**: `atom file.md:304`

## Platform Support

- **macOS**: Uses `open` command for default editor
- **Linux**: Uses `xdg-open` or `$EDITOR` for default editor
- **Windows**: Uses `start` command for default editor
- **Other**: Falls back to displaying the file path

## Environment Variables

- `LPOST_FILE` - Override the posts file location
- `EDITOR` - Set preferred editor (used when editor is set to 'default' in config)

## lmno.md Format

lpost works with the lmno.md format where posts are stored chronologically in a single file:

```markdown
# [2025-07-19 Sat] Most Recent Post Title

Content of the most recent post...

# [2025-07-18 Fri] Previous Post Title

Content of the previous post...

# [2025-07-17 Thu] Even Older Post

Content continues...
```

Posts can have headers with or without day names:
- `# [2025-07-19 Sat] Title` (with day name)
- `# [2025-07-19] Title` (without day name)

## Examples

```bash
# Create a blog post
lpost -t "Getting Started with Helix Editor"

# Create today's daily notes
lpost --daily

# List last 5 posts
lpost list 5

# Edit the 2nd most recent post
lpost edit 2

# Find posts about Hugo and edit one
lpost list --keyword hugo     # Sets filter context
lpost edit 1                  # Edits most recent Hugo-related post

# Clear filters by listing without options
lpost list                    # Clears any saved filters
lpost edit 1                  # Edits from all posts

# Preview from a filtered list
lpost list --keyword journal  # Sets journal filter
lpost preview 2               # Previews 2nd journal post

# Override saved filter explicitly
lpost list --keyword ruby     # Sets ruby filter
lpost edit 1 --keyword python # Explicitly uses python filter instead

# Publish your blog
lpost --publish                # Shows confirmation prompt
```

## Differences from hpost

Unlike hpost (which manages separate Hugo markdown files), lpost:

- Works with a single monolithic file instead of a directory of files
- Has no front matter (no YAML headers with tags/categories)
- Uses keyword search instead of tag filtering
- Prepends new posts to the top of the file
- Opens editors at specific line numbers for editing
- Creates temporary files for previewing individual posts

## License

MIT