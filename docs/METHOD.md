# Method: prove the rewrite matches

A rewrite that "looks right" is not enough. For each project:

1. **Keep the original.** The Rust version sits in `rust/` next to it. Nothing is
   deleted, so both can be run on the same machine.
2. **Pick the outputs that matter.** For a search tool: the chunks it indexes and
   the results it returns. For a classifier: the probabilities. For an API: the
   responses, including error bodies.
3. **Feed both versions identical inputs.** Real data where possible, plus
   randomised or fuzzed inputs for parsers and tokenizers.
4. **Compare the outputs automatically.** A script runs both and reports any
   difference. Differences are either bugs in the port or documented, intentional
   changes.
5. **Fix, then repeat** until the comparison is clean. Golden files freeze the
   agreed outputs so `cargo test` keeps checking them.
6. **Test the whole program too.** Headless-browser runs for web front ends, and a
   pseudo-terminal run for terminal programs.

## What this caught

- A `<select>` and an `<input>` that kept stale values after the page re-rendered
  (logbook, Leptos).
- Off-by-one error positions in JSON error messages (rhcp-chatbot).
- A terminal program that crashed after running an interactive action because it
  asked the terminal for the cursor position (sietch).
- A front-matter parser that failed on Windows line endings (portfolio).
