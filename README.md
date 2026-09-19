# TechFarsi Courses

Persian-language course content for [techfarsi.com](https://techfarsi.com). Content only — no
rendering. The website consumes this repo as a submodule and owns the UI, auth, progress
tracking and quiz grading.

## Layout

```
courses/
  <course-slug>/
    meta.json                  course metadata and module order
    <nn>-<module-slug>/
      <nn>-<lesson-slug>.mdx         the lesson
      <nn>-<lesson-slug>.quiz.json   its quiz
```

Folder and file number prefixes set the order. They never appear in URLs — the site builds
paths from `slug` fields, e.g. `techfarsi.com/learn/javascript/install-node`.

## Course metadata

Each course has a `meta.json` at its root:

```json
{
  "slug": "typescript",
  "title": "...",
  "description": "...",
  "language": "fa",
  "level": "beginner",
  "order": 3,
  "prerequisites": ["javascript"],
  "modules": [
    { "slug": "setup", "title": "...", "order": 1, "lessons": ["setup-typescript"] }
  ]
}
```

`order` sets the course's position in the site's course list. `prerequisites` is an optional
array of course slugs the site shows as "take this first"; omit it when a course stands alone.
`modules[].lessons` lists lesson slugs in teaching order, and every lesson file must appear in
exactly one of them.

## Lesson format

Every lesson is the same shape, so writing scales and the site can render it generically:

- one objective, stated in the frontmatter
- five minutes of reading, maximum
- one runnable example
- one exercise, with the solution in a `<Solution>` block
- exactly three quiz questions

### Frontmatter

```yaml
---
title: نصب Node.js
description: یک جمله، برای کارت درس و متای صفحه.
slug: install-node
order: 1
duration: 5            # minutes
level: beginner        # beginner | intermediate | advanced
objectives:
  - بعد از این درس می‌توانید …
tags: [node, setup]
---
```

### Quiz schema

Each lesson has a sibling `.quiz.json` with exactly three questions.

```json
{
  "lesson": "install-node",
  "passScore": 2,
  "questions": [
    {
      "id": "q1",
      "type": "multiple-choice",
      "prompt": "سوال؟",
      "options": ["الف", "ب", "ج", "د"],
      "answer": 1,
      "explanation": "چرا این گزینه درست است."
    }
  ]
}
```

`type` is one of:

| type | what it asks | `answer` |
| :-- | :-- | :-- |
| `multiple-choice` | pick one option | index into `options` |
| `output` | what does this `code` print | index into `options` |
| `fill-blank` | complete the `___` in `code` | array of accepted strings |

`answer` is never free text — grading stays deterministic and client-side.

## Writing rules

- **Persian prose, English technical terms.** Write `array`, `type`, `async`, `function` in
  English. Persian developers read code in English, and invented equivalents make the text
  harder, not easier.
- **One opinion per decision.** pnpm, Vitest, VS Code. Do not offer alternatives or revisit a
  choice in a later lesson.
- **Every example runs.** If a reader pastes it, it works, with no omitted setup.
- No em dashes in Persian prose. Use commas and colons.

## Adding a lesson

1. Create the `.mdx` and its `.quiz.json` in the right module folder, numbered in sequence.
2. Add the lesson to the module's entry in the course `meta.json`.
3. Open a PR. One lesson per PR keeps review fast.

This README is in English because it documents structure and schema for contributors. All
lesson content is in Persian.
