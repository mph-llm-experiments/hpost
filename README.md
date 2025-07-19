# Blog Post Management Tools

This repository contains two command-line tools for creating and managing blog posts in different formats:

## Tools

### hpost - Hugo Post Creator

A command-line tool for creating and managing Hugo blog posts with proper front matter, tags, and categories.

👉 [**Full hpost documentation**](README_hpost.md)

### lpost - lmno.md Post Creator

A command-line tool for creating and managing posts in lmno.md format - a monolithic markdown file where all posts are stored chronologically in a single file.

👉 [**Full lpost documentation**](README_lpost.md)

## Installation

Both tools are Ruby scripts. Make them executable:

```bash
chmod +x hpost lpost
```

For hpost, you'll also need:

```bash
gem install slugify
```

## Quick Start

### hpost (Hugo posts)

```bash
# Configure your Hugo posts directory
export HPOST_DIR="~/blog/content/posts/"

# Create a new post
hpost -t "My Blog Post" -T "ruby,programming" -c "Tech"

# List and edit recent posts
hpost list
hpost edit 1
```

### lpost (lmno.md posts)

```bash
# Configure your lmno.md file
export LPOST_FILE="~/blog.md"

# Create a new post
lpost -t "My Journal Entry"

# List and edit recent posts
lpost list
lpost edit 1
```

## License

MIT
