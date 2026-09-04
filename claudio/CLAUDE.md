# Project instructions

## Working method
- If an ambiguity would change the structure of the code, ask before writing anything and wait for the answer. If it would only change a detail, state the assumption in one line and proceed.
- Do not add command line arguments, flags, or options unless explicitly asked. If you think one is needed, ask first. When an argument is dropped, drop the parameters that only existed to carry it.

## Style, all files
- Never use em dashes, in code, comments, output, Markdown, commit messages, or PR text. Use a hyphen or rewrite the sentence.
- Indent with tabs, width 4, in every language including Dart and JSON (`json.dumps(payload, indent="\t")` in Python). Do not run formatters that convert tabs to spaces.
- Organize files into folders with a stated logic, not a flat directory.
- Extract logic that appears twice into one reusable piece.
- Comments explain *why*. Do not add comments that restate what the code visibly does.

## Documentation and validation, all languages
- Start every file with a header comment (JSDoc block in JS, module docstring in Python) saying what the file is and its role in the project.
- Give every function a hover-friendly doc block: one-line imperative summary, one line per argument stating its meaning and type, and a return line. Doc blocks are the exception to the rule above: describe arguments and returns meaningfully, not tersely.
- Write in the register of MDN, numpy, or requests: imperative summaries ("Return the ids of ..."), plain technical vocabulary, American spelling, no metaphor or anthropomorphism. "Attach the listener", not "latch onto".
- Validate arguments at the top of every function a caller can reach: check type and allowed values (range, emptiness, membership), name the offending argument in the error message, and put the checks in shared helpers instead of repeating type-check chains. Do not use `assert` or `console.assert` for this; the first is stripped under `-O`, the second only logs.

| | JavaScript | Python |
|---|---|---|
| Doc block | JSDoc with `@param {type} name` and `@returns {type}` | Google-style docstring with `Args:` and `Returns:` |
| Wrong type | `TypeError` | `TypeError` |
| Wrong value | `RangeError` | `ValueError` |

## Python
- Annotate every argument and return type (`x: str`, `-> list[str]`).
- Keep each string literal on one line; no implicit concatenation across lines.
- Keep `requirements.txt` in step with imports: every third party package pinned to a minimum version, nothing from the standard library, remove what is no longer imported. Create the file if missing.