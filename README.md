# @Doc (M@rkdown)

> Markdown reimagined with @semantic nodes for humans and AI.



M@rkdown is an AI-native structured markdown language designed for:

Human readability

Deterministic parsing

AST-first architecture

Token efficiency

Streaming-safe rendering

Semantic document structures


Unlike traditional Markdown, M@rkdown introduces fixed semantic slots using @nodes.


---

Example

Basic Markdown

# Hello World

This is normal markdown.

M@rkdown

@card(featured){w:full}[
  @title[AI Native Language]
  @text[
    Structured markdown with deterministic grammar.
  ]
  @btn(primary)[Get Started](install)
]


---

Core Grammar

@node(modifier){style}[content](action)

Slot	Meaning

@node	Semantic node type
()	Behavior / modifier
{}	Style / metadata
[]	Content
() tail	Action / reference



---

Design Philosophy

1. Deterministic Grammar

Fixed grammar slots make parsing stable and predictable.


---

2. Human Readable Compression

Compact syntax without becoming cryptic.


---

3. AI-Native Structure

Designed for:

LLM generation

Semantic parsing

AST mapping

Streaming rendering

Structured editing



---

4. AST-First

Every syntax structure maps cleanly into a canonical AST.

Example:

@btn(primary){#000}[Confirm](submit)

↓

{
  "type": "button",
  "modifier": ["primary"],
  "style": {
    "color": "#000"
  },
  "content": "Confirm",
  "action": "submit"
}


---

Why M@rkdown?

Traditional Markdown was designed for:

Human → HTML

M@rkdown is designed for:

Human ↔ AI ↔ AST ↔ Renderer


---

Goals

Markdown compatibility

Semantic document structures

Deterministic parsing

AI-friendly tokenization

Lossless AST conversion

Incremental parsing support

Streaming-safe rendering



---

Status

Early experimental design phase.

Currently focusing on:

Grammar design

AST specification

Parser architecture

Token optimization

Streaming parsing

AI-native document workflows



---

License

MIT
