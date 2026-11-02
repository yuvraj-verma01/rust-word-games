# rust-word-games

Assignments in Rust for CS-2102 Programming Lab. Had fun making these :)

This repository contains Boggle, Spelling Bee, and Wordle. All three use the existing Rust trie and the supplied dictionary. The original trie command-line program is included because it is part of the shared Cargo workspace.

## Run the games

Rust and Cargo must be installed. Clone the repository, then open its Cargo workspace:

```sh
git clone https://github.com/yuvraj-verma01/rust-word-games.git
cd rust-word-games/word-trie
cargo run --locked --offline -p boggle -- ../dictionary.txt
cargo run --locked --offline -p spelling-bee -- ../dictionary.txt
cargo run --locked --offline -p wordle -- ../dictionary.txt
```

Run one game at a time. Each command reads the included dictionary. No third-party crates or network access are needed to build the project once Rust is installed.

## Boggle

Choose a 4x4, 5x5, or 6x6 grid. Enter lowercase words of at least three letters, separated by commas, whitespace, or new lines. Consecutive letters must be neighbouring cells horizontally, vertically, or diagonally, and a cell cannot be reused within one word.

Enter `:done` to evaluate the collected guesses or `:quit` to report the current results and exit. `:help` explains the rules. After scoring, guesses cannot be changed. Different word forms can score separately; repeated copies of the same word score once.

The program uses the PDF's score table: 3-4 letters earn 1 point, 5 earn 2, 6 earn 3, 7 earn 5, and 8 earn 11. The final table row is applied to nine or more letters at 2 points per letter. Each cell holds one letter, including Q.

The path search uses a fixed visited array with mark, search, and unmark backtracking. The board stays unchanged.

## Spelling Bee

The seven distinct letters are shown with the main letter first. Enter one dictionary word of at least four letters per line. Every word must include the main letter and use only the displayed letters; letters may repeat. Uppercase and lowercase input are accepted.

Four-letter words earn 1 point. Longer words earn their length in points. A pangram uses all seven different letters and earns 7 additional points. The complete target list is computed before play starts.

Found words are displayed alphabetically in three columns. The letters are repeated directly above every interactive word prompt. Finding every target awards Queen Bee and ends the game automatically. `:quit` reports the score and all missed words; `:help` shows the rules.

## Wordle

Guess a five-letter word in six accepted guesses. Invalid input and words absent from the trie do not use an attempt. Enter one word per line; uppercase and lowercase input are accepted.

Feedback is shown as `G` for green/correct position, `Y` for yellow/wrong position, and `-` for grey/no unused matching letter. Interactive terminals also show coloured letters. Exact matches are reserved first, then other matches use only remaining answer letters.

A correct sixth guess still wins. `:help` displays the rules, and `:quit` exits and reveals the answer.

## Files and checks

- `dictionary.txt`: supplied dictionary, containing 178,691 unique words.
- `word-trie/trie`: shared trie library and its original tests.
- `word-trie/trie-cli`: original trie command-line program.
- `word-trie/boggle`, `word-trie/spelling-bee`, `word-trie/wordle`: game libraries, terminal programs, and tests.
- `word-trie/trie_Dictionary.pdf`: existing trie documentation report.
- `TESTING.md`: testing strategy, verification commands, and results for the verified submission.

Keep `dictionary.txt` next to the `word-trie` folder, as packaged, so the existing dictionary tests can find it.

From `word-trie`, run:

```sh
cargo fmt --all -- --check
cargo check --workspace --all-targets --locked --offline
cargo test --workspace --all-targets --locked --offline
cargo build --workspace --release --locked --offline
```
