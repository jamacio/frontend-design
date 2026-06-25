---
name: doc
description: 'Read, create, and edit .docx documents with formatting and layout fidelity
  via AI provider''s document skill.

  '
triggers:
- AI provider doc
- docx fidelity
- word doc edit
- layout doc
od:
  mode: prototype
  category: documents
---
# Doc

## Overview
Creates structured documents in Markdown and HTML format. Supports technical documentation, product specs, API references, and guides.

## Document Types
- **README**: Project overview, setup, usage
- **Technical spec**: Architecture, data flow, API contracts
- **API reference**: Endpoints, parameters, examples, responses
- **User guide**: Step-by-step instructions with screenshots
- **Changelog**: Version history with breaking changes

## Structure
- Table of contents for documents over 3 sections
- Consistent heading hierarchy (no skipped levels)
- Code blocks with language annotation
- Links to related documents
- Version and date in footer
