# rust-remaster

Rust rewrites of my own projects, built next to the originals and checked
against them on identical inputs. This repo is the index: what each remaster is,
where its code lives, how it was verified, and how to run it.

The Rust code was written by Claude Code under my direction. I chose the
projects and the rules, and I run and test the results. Each remaster has a
learning guide that explains the code for someone new to Rust.

## The remasters

| Project | Original | Rust version | Where | Learning guide |
|---|---|---|---|---|
| [tbjp-oracle](https://github.com/gilbertordx/tbjp-oracle) | Python, LangChain, Chroma, Streamlit | `axum` web server, own chunker, index and keyword search | `rust/` | [RUST_LEARNING.md](https://github.com/gilbertordx/tbjp-oracle/blob/main/docs/RUST_LEARNING.md) |
| [logbook](https://github.com/gilbertordx/logbook) | JavaScript, no framework | Core library plus a Leptos front end compiled to WebAssembly | `rust/` | [RUST_LEARNING.md](https://github.com/gilbertordx/logbook/blob/main/docs/RUST_LEARNING.md) |
| [rhcp-chatbot](https://github.com/gilbertordx/rhcp-chatbot) | Python, FastAPI, NLTK, scikit-learn | `axum` API, hand-written NLP stack, SQLite FTS5 | `rust/` | [RUST_LEARNING.md](https://github.com/gilbertordx/rhcp-chatbot/blob/main/docs/RUST_LEARNING.md) |

The originals are untouched. Each Rust version lives in a `rust/` folder inside
the same repo.

Also here: [sietch](https://github.com/gilbertordx/sietch), a Rust
terminal menu that starts any of these projects and opens it in the browser.

## How each one was checked

The method is in [docs/METHOD.md](docs/METHOD.md): run the original and the
Rust version on the same inputs and compare the outputs.

| Project | What was compared | Result |
|---|---|---|
| tbjp-oracle | Chunks produced by ingesting the same data | 15,609 chunks, identical |
| tbjp-oracle | Search results across scope, sort and exact-match settings | 144 configurations, identical |
| logbook | Core calculations (dates, statistics, storage) against the JavaScript, then the two interfaces in a headless browser | Matching outputs |
| rhcp-chatbot | Class probabilities against scikit-learn | Equal to about 1e-15 |
| rhcp-chatbot | Tokenizer, sentence splitter, stemmer, TF-IDF; FastAPI error shapes; password hashes and login tokens | Matching; the same SQLite file works with either server |

Not ported on purpose: the Garmin and Spotify features of logbook.

## Running them

Easiest is [sietch](https://github.com/gilbertordx/sietch). By hand, from
each project's folder:

```bash
# tbjp-oracle   (http://127.0.0.1:8501)
cargo run --release --manifest-path rust/Cargo.toml -- ingest
cargo run --release --manifest-path rust/Cargo.toml -- serve

# logbook       (http://127.0.0.1:8080; needs `cargo install trunk`)
cd rust/web && trunk serve

# rhcp-chatbot  (chat in the terminal, or an API on http://127.0.0.1:3000)
cargo run --release --manifest-path rust/Cargo.toml -- setup-nltk
cargo run --release --manifest-path rust/Cargo.toml -- chat
cargo run --release --manifest-path rust/Cargo.toml -- serve --host 127.0.0.1
```

Each Rust folder also has its own tests: `cargo test --release --manifest-path rust/Cargo.toml`
(logbook: `--workspace`).

## Known limits

- The comparison scripts need the original Python or Node setup to run.
- Windows is the main platform for daily use, and a few issues only showed up
  there (line endings, a missing `trunk`). They are fixed, but Linux got more
  testing.
