# ghita.org

A minimal Jekyll website powered by the [no-style-please](https://github.com/riggraz/no-style-please) theme.

## Features

- **Homepage**: Welcome message with latest blog posts
- **Contact Page**: Contact information and links
- **Blog**: Multiple blog posts with dates and categories
- **Navigation**: Clean menu to navigate between pages
- **Minimalist Theme**: Focus on content, not style

## Local Development

### Prerequisites

- Ruby 3.x
- Bundler

### Setup

1. Install dependencies:
```bash
bundle install
```

2. Build the site:
```bash
bundle exec jekyll build
```

3. Serve locally:
```bash
bundle exec jekyll serve
```

Visit `http://localhost:4000` to view the site.

## Adding Content

### New Blog Post

Create a new file in `_posts/` with the format: `YYYY-MM-DD-title.md`

```markdown
---
layout: post
title: "Your Post Title"
date: YYYY-MM-DD HH:MM:SS -0000
categories: category1 category2
---

Your content here...
```

### New Page

Create a new `.md` file in the root directory:

```markdown
---
layout: page
title: Your Page Title
permalink: /your-page/
---

Your content here...
```

## Deployment

This site can be deployed to:
- GitHub Pages
- Netlify
- Vercel
- Any static site hosting service

## License

Site content is yours to modify and use as needed.
