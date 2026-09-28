# Project instructions

This file goes from general to specific. The sections at the top hold generic
instructions that apply across many situations, and each section below narrows
to a more particular scope. Read the top ones as always in force, and the lower
ones as the detail that applies once you are working in that area. When adding
a rule, place it at the level it actually applies to: a habit that shapes every
task belongs at the top, a language or tooling detail belongs further down.

## Working method
- Do not add a `tests` folder. If a temporary `tests` folder is needed for verification, remove it and its generated files before finishing the task.
- On every prompt, if anything is unclear or open to more than one reading, ask for clarification before proceeding. Do not guess at intent and do not start writing code on an assumption. Ask the question first, wait for the answer, and only then act. If part of the request is clear and part is not, say which part you are proceeding with and ask about the rest.

## Code style
- Use blank lines to separate logical sections of code, such as input validation, preparation, processing, and returning results. Keep related statements together; do not write uninterrupted blocks that combine distinct steps.
- Separate individual validation checks and processing steps with blank lines even within the same logical section, loop, or `try` block. Add a blank line after a loop header before its first check, and between preparation, a conditional, and the next step. Keep closely related assignments and operations together. Follow the spacing in `extract_team_members` in `src/helpers/team.py` as the reference style.
- Use one blank line between sections, functions, and classes; do not insert consecutive blank lines.
- Never use em dashes anywhere. This covers comments, code, and terminal output, and it applies just as strictly to every non-code file in the repo: README.md, any other Markdown, documentation, commit messages, and issue or pull request text. Use a regular hyphen or rewrite the sentence.
- Indent with tabs, tab width 4. Do not use spaces for indentation. This includes Dart. (Do not run `black`, `dart format`, or any other formatter that rewrites tabs to spaces.)
- When writing a JSON file, indent it with tabs too. In Python that means `json.dumps(payload, indent="\t")`, not `indent=1` or any number, since a number means spaces.
- Organize files into folders with a stated logic. A flat directory of everything is not organization.
- Prefer small reusable pieces over duplication. The same logic appearing twice is a signal to extract it once.

The language sections below are in alphabetical order.

### JavaScript
- Give every function a JSDoc block so VS Code renders full help on hover: a one-line summary, one `@param {type} name` line per argument stating what it means, and a `@returns {type}` line. JSDoc is the exception to the anti-slop rule against restating the *what*: it exists precisely to render on hover, so describe the arguments and return meaningfully rather than tersely.
- Start every JavaScript file with a file-level JSDoc block (a `/** ... */` comment before the first statement) describing what the file is and its role in the project. A short paragraph is enough.
- Write JSDoc and code prose in the register MDN and the major JavaScript libraries use: imperative summary lines such as "Return the ids of ..." or "Send one message and ...", plain technical vocabulary, American spelling, and no anthropomorphism or metaphor. Name what the code does rather than dramatize it: "attach the listener", not "latch onto"; "run the worker loop", not "own the browser for its whole lifetime".
- Validate arguments at the top of every function a user of the code can reach, before doing any other work. Check both the type of each argument and its allowed values (ranges, emptiness, membership). Throw `TypeError` for a wrong type and `RangeError` for a wrong value, and name the offending argument in the message. Do not use `console.assert` for this, since it only logs and lets the bad value through. Put the checks in shared helpers instead of repeating the same `typeof` and `Array.isArray` chains.

### Python
- Use `ThreadPoolExecutor` for independent operations that can safely run in parallel. Cap `max_workers` at `os.cpu_count()` with a fallback of 1, and choose a lower configurable limit when disk I/O, network access, or memory use warrants it.
- Put one blank line between every function's closing docstring and the first statement of its body.
- Annotate every function argument with its type (e.g. `myvar: str`), and specify the function's return type whenever possible (e.g. `-> list[str]`).
- Give every function a docstring so VS Code renders full help on hover: a one-line summary, a Google-style `Args:` block listing each argument with its type and what it means, and a `Returns:` line. Docstrings are the exception to the anti-slop rule against restating the *what*: they exist precisely to render on hover, so describe the arguments and return meaningfully rather than tersely.
- Start every Python file with a module-level docstring (the first statement, before the imports) describing what the file is and its role in the project. A short paragraph is enough.
- Write docstrings and code prose in the register the major Python libraries use (numpy, pandas, requests, pytorch): imperative summary lines such as "Return the ids of ..." or "Send one message and ...", plain technical vocabulary, American spelling, and no anthropomorphism or metaphor. Name what the code does rather than dramatize it: "attach the listener", not "latch onto"; "run the worker loop", not "own the browser for its whole lifetime".
- Do not split a string across multiple lines with implicit concatenation (no `("..." "...")` blocks). Keep each string literal on one line, even if long.
- Validate arguments at the top of every function a user of the code can reach, before doing any other work. Check both the type of each argument and its allowed values (ranges, emptiness, membership). Raise `TypeError` for a wrong type and `ValueError` for a wrong value, and name the offending argument in the message. Do not use `assert` for this, since assertions are stripped when Python runs with `-O` and the checks would silently disappear. Put the checks in shared helpers instead of repeating the same `isinstance` chains.
- Keep `requirements.txt` in step with the code. Every third party package the project imports belongs in it, pinned to a minimum version, and anything no longer imported comes out. Create the file if it does not exist yet. Do not list packages from the standard library.
