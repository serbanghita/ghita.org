# ghita.org

Personal blog powered by Jekyll.

## Local Development

### Using Docker (recommended)

```bash
docker run --rm -v "$PWD:/srv/jekyll" -p 4000:4000 jekyll/jekyll:4 jekyll serve --host 0.0.0.0
```

Visit http://localhost:4000 to view the site.

### Using Ruby

```bash
bundle install
bundle exec jekyll serve
```

## Adding Content

Create a new file in `_posts/` with the format `YYYY-MM-DD-title.md`:

```markdown
---
layout: post
title: "Your Post Title"
---

Your content here...
```
