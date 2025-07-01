# STYLE_GUIDE.md Maintainer

Ultrathink: 

Please analyze this codebase and create a `docs/STYLE_GUIDE.md` file, which will be given to future instances of Claude Code to operate in this repository.

What to add:

Commands that will be commonly used, such as how to build, lint, and run tests. Include the necessary commands to develop in this codebase, such as how to run a single test.

High-level code architecture and structure so that future instances can be productive more quickly. Focus on the "big picture" architecture that requires reading multiple files to understand

Usage notes:

If there's already a `docs/STYLE_GUIDE.md`, suggest improvements to it.

When you make the initial `docs/STYLE_GUIDE.md`, do not repeat yourself and do not include obvious instructions like "Provide helpful error messages to users", "Write unit tests for all new utilities", "Never include sensitive information (API keys, tokens) in code or commits"

Avoid listing every component or file structure that can be easily discovered

Don't include generic development practices

If there are Cursor rules (in .cursor/rules/ or .cursorrules) or Copilot rules (in .github/copilot-instructions.md), make sure to include the important parts.

If there is a README.md, make sure to include the important parts.

Do not make up information such as "Common Development Tasks", "Tips for Development", "Support and Documentation" unless this is expressly included in other files that you read.

Be sure to prefix the file with the following text:

```

STYLE_GUIDE.md
This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

```

Walk through the ENTIRE project and:
- determine what programming language is being used
- determine what libraries and platforms are being used
- update `docs/STYLE_GUIDE.md` based on best practices for best practices for those languages/platforms.
- this may be run iteratively, edit best practices that are already present. Do NOT remove ANYTHING, merely append. 
- Consult with Zen if the MCP server is available.
- Consult with context7 if the MCP server is available.

