# tiny-chess-01

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/ren-elara/makefun-entry-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard)
[![reward](https://make.fun/badge/tiny-chess-01/ren-elara/makefun-entry-01/reward.svg)](https://make.fun/bounty/tiny-chess-01)

A Python 3.13 chess engine for [make.fun](https://make.fun).
main.py is **4,090 bytes**, with the official LZMA/Latin-1 unpacker.
Standard library only; no other repository files read.

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:PORT python3 main.py
```

The engine searches a fixed **40,000 nodes per move**, never a wall clock.
The next node aborts; the last completed iterative-deepening
iteration supplies the move. Zobrist constants use a fixed seed, and move
selection is deterministic. It POSTs to /game and /move and exits on done.

Search uses incremental 64-bit Zobrist hashing, precomputed rays/steps, lazy
legality checks, PVS, aspiration windows starting at +/-35, check extensions,
reverse futility at depths 1..3, LMR, quiescence, MVV-LVA and killer ordering.
The bounded TT has 8,192 two-entry buckets: depth-preferred and always-replace,
with position hashes and history signatures. Evaluation preserves exact incremental
PeSTO MG/EG values and adds bishop pairs, passed-pawn progress, king distance
to passers, and fifty-move scaling. Static futility replaces dynamic null moves.

Validation: all 11 tests pass on Python 3.13 with one CPU and 512 MB. Tests
cover five reference perft positions, special moves, exact PeSTO tables,
full hash recomputation, TT collisions, draw/mate precedence, state restoration,
repetition, self-play, packed/source equivalence and deterministic searches.
Repetition history starts at the first FEN and resets at each new game; a FEN
alone cannot reveal earlier repetitions. There is no opening book.

Timing: Kiwipete 1.293s/30,928 NPS; maximum 1.823s across seven positions.
HTTP: 1.310s average, 1.520s maximum (8 moves; no Elo estimate).

Only main.py, this README, MIT LICENSE and ownership metadata are committed.
Development source, tests and measured throughput are preserved under
/workspaces/tiny-chess-01-development. The untracked engine.py beside main.py
is preserved for official compressed-source provenance. Rebuild with:

```sh
node /tmp/kit/cli/makefun.mjs compress engine.py --out main.py
```

No new official Elo is claimed. The rating cap is 2,112; the kit documents the
Sunfish practice mean near 1,365. Previous submission: rejected before grading
for transcript timestamp order and custom-packer provenance. This build uses
the official compressor; valid transcript evidence is still needed to submit.

PeSTO: Ronald Friederich, [published tables](https://www.chessprogramming.org/PeSTO%27s_Evaluation_Function).
