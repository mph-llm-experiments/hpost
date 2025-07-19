# hpost - Hugo Post Creator

A command-line tool for creating and managing Hugo blog posts with proper front matter.

## Features

- Create new Hugo posts with automatic date stamping and slug generation
- List recent posts and operate on user-friendly index numbers
- Preview posts with markdown viewers (glow, mdless, bat, etc.)
- Filter post lists by tag
- Daily notes format
- Cross-platform file opening support
- Configurable via environment variables or config file
- Automatic front matter generation

## Installation

1. Install the required Ruby gem:

   ```bash
   gem install slugify
   ```

2. Make the script executable:

   ```bash
   chmod +x hpost
   ```

3. (Optional) Add to your PATH or create an alias

## Configuration

### Method 1: Environment Variable

```bash
export HPOST_DIR="~/your-blog/content/posts/"
```

### Method 2: Configuration File

Create `~/.config/hpost/config.yml`:

```yaml
posts_dir: ~/blog/content/posts/
editor: code # or vim, emacs, default, etc.
preview: glow # or mdless, bat, default, etc.
default_tags: []
default_category: []
default_draft: true # false to publish posts by default
```

See `config.yml.example` for a complete example.

## Usage

### Create a new post

```bash
# Basic usage
hpost -t "My New Post Title"

# With tags and category
hpost -t "My Post" -T "ruby,programming" -c "Tech" -s "A brief summary"

# Create as published post (not draft)
hpost -t "My Post" --publish

# Daily notes
hpost -d
```

### List recent posts

```bash
# List 10 most recent posts (default, sorted by date prefix)
hpost list

# List 20 most recent posts
hpost list 20

# List posts sorted by modification time instead
hpost list --modtime
hpost list 20 --modtime

# Filter by tag
hpost list --tag ruby
hpost list 20 --tag programming
hpost list --tag journal --modtime
```

**Note**: Filters persist! When you use `--tag` or `--modtime` with list, subsequent edit/preview commands remember those filters:

### Edit existing post

```bash
# First list posts to see IDs
hpost list

# Edit post by ID (uses same sort order as list)
hpost edit 3

# Edit when using modtime sorting
hpost list --modtime
hpost edit 3 --modtime
```

### Preview posts

```bash
# Preview a post using configured tool (glow, mdless, bat, etc.)
hpost preview 1

# Preview with specific sorting
hpost preview 3 --modtime
```

## Options

- `-t, --title TITLE` - Post title (required for create)
- `-T, --tags TAGS` - Comma-separated tags
- `-c, --category CATEGORY` - Comma-separated categories
- `-s, --summary SUMMARY` - Post summary
- `-d, --daily` - Create a daily notes post
- `-p, --publish` - Create post as published (not draft)
- `--draft` - Create post as draft (default)
- `-m, --modtime` - Sort list/edit by modification time (default: date prefix)
- `-g, --tag TAG` - Filter list by tag (case-insensitive)
- `--clear-filters` - Clear saved filter state
- `-h, --help` - Show help message

## Platform Support

- **macOS**: Uses `open` command
- **Linux**: Uses `xdg-open` or `$EDITOR`
- **Windows**: Uses `start` command
- **Other**: Falls back to displaying the file path

## Environment Variables

- `HPOST_DIR` - Override the posts directory
- `EDITOR` - Set preferred editor (used when editor is set to 'default' in config)

## Examples

```bash
# Create a blog post
hpost -t "Getting Started with Ruby" -T "ruby,tutorial" -c "Programming"

# Create today's daily notes
hpost --daily

# List last 5 posts
hpost list 5

# Edit the 2nd most recent post
hpost edit 2

# List all posts with 'ruby' tag and edit one
hpost list --tag ruby     # Sets filter context
hpost edit 3              # Automatically uses ruby filter!

# Clear filters by listing without options
hpost list                # Clears any saved filters
hpost edit 1              # Edits from all posts

# Preview from a filtered list
hpost list --tag journal  # Sets journal filter
hpost preview 2           # Previews 2nd journal post

# Override saved filter explicitly
hpost list --tag ruby     # Sets ruby filter
hpost edit 1 --tag python # Explicitly uses python filter instead
```

## License

MIT
