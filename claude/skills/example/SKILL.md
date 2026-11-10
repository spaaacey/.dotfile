---
name: example
description: Print a bare-minimum, runnable code example directly in the chat response. Use when the user asks for a quick example, a minimal snippet, or types '/example'. Never writes files.
---

Output exactly one fenced code block containing the smallest idiomatic, runnable snippet that demonstrates what was asked. Nothing before or after the code block — no preamble, no explanation, no caveats.

Do not use Write, Edit, or any file-creation tool. The code goes only in the chat response.

Keep it minimal but real: use actual required imports/idioms so it would run as-is, but skip error handling, comments, CLI wrappers/argparse, logging, and tests.

If the language isn't specified or clear from context, default to TypeScript.
