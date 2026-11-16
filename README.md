# tiny-chess-01

Self-contained Python 3.13 chess entry for
[make.fun tiny-chess-01](https://make.fun/bounty/tiny-chess-01).

`main.py` is 3,837 bytes, including its Latin-1/LZMA unpacker. It uses only
the standard library and reads/imports no other repository file. Run with:

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:PORT python3 main.py
```

The engine uses a signed-piece 10x12 mailbox, legal castling, en-passant,
all promotions, repetition tracking and the fifty-move clock. Evaluation is
incremental PeSTO MG/EG with tapering and a bishop-pair bonus. Search includes
iterative deepening, PVS, TT bounds/best moves, null-move pruning, LMR,
capture/promotion quiescence, MVV-LVA and killer ordering.

Search uses a **fixed 4,000-node budget**, never a wall clock or randomness.
The last completed iteration supplies the move; a legal fallback is retained.
It POSTs to `/game` and `/move` at MAKEFUN_ENV_URL, and exits on status `done`.
Repetition history begins with the first supplied FEN; earlier history cannot
be recovered from FEN alone. There is no opening book or opening-specific code.

Validation: 11 tests passed on Python 3.13 with one CPU and 512 MB, covering
five perft positions, special moves, incremental evaluation, draw/mate
precedence, tactics, state restoration, self-play and deterministic searches.
Two measured searches took 0.927s and 1.507s. These are local measurements,
not a guarantee of latency on every host. No official Practice Elo is claimed.

Only the packed entry, this README, MIT LICENSE and ownership metadata are
included to satisfy the 8,192-byte repository and 4,096-byte per-file limits.
Development source, packer and tests are preserved outside this submission
checkout at `/workspaces/tiny-chess-01-development`; run `python3 build.py --test`
there to reproduce the build. Practice trials use six games; final trials use
eight. AI-session transcripts are supplied separately with submission.

PeSTO tables: Ronald Friederich,
[published evaluation tables](https://www.chessprogramming.org/PeSTO%27s_Evaluation_Function).