# Blog Skills — AI Content Pipeline

Open source AI agent instructions for content creation. These are prompt-based skills that any LLM (Claude, GPT, Gemini, etc.) can use to research topics, write blog posts, create social media content, and review everything against your brand guidelines.

## What's included

| Skill | File | Description |
|---|---|---|
| **Researcher** | `skills/researcher.md` | Crawls sources, extracts relevant findings, and produces a structured research brief for content writers |
| **Blog Writer** | `skills/blog-writer.md` | Turns a topic or research brief into a full blog post with SEO metadata (title, slug, tags, meta description) |
| **LinkedIn Writer** | `skills/linkedin-writer.md` | Creates thought-leadership LinkedIn posts from a blog post or topic, optimized for the feed algorithm |
| **X (Twitter) Writer** | `skills/x-writer.md` | Writes tweets and short threads that are direct, technical, and drive traffic to the blog |
| **Brand Reviewer** | `skills/brand-reviewer.md` | Reviews any draft content against your BRAND.md and returns a compliance score, violations, and auto-corrected version |
| **Brand Template** | `templates/BRAND.md` | A complete brand guide template — customize it with your identity, voice, terminology, and visual identity |

## How to use

These are **prompt instructions** — you feed them as system prompts or context to any LLM.

1. **Copy `templates/BRAND.md`** and fill it in with your brand's identity, tone, terminology, and values.
2. **Pick a skill** from the `skills/` folder.
3. **Send the skill file + your BRAND.md as context** to your LLM of choice (Claude, GPT, etc.).
4. **Provide the input** each skill expects (a topic, a research brief, a draft to review, etc.).
5. **Get structured output** — every skill returns a well-defined format (JSON or Markdown) ready for your CMS.

### Example workflow

```
Researcher  -->  Blog Writer  -->  Brand Reviewer  -->  Publish
                  |                     |
                  +--> LinkedIn Writer --+
                  |                     |
                  +--> X Writer --------+
```

The researcher finds a topic. The writers create content for each channel. The brand reviewer checks everything before it goes live.

## License

MIT -- see [LICENSE](./LICENSE)

## Built by

[Agentikas Labs](https://agentikas.ai) — Agentic software lab. Open source. Powered by Claude.
