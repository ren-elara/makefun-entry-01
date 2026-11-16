# Elara 4088

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/ren-elara/makefun-entry-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/ren-elara/makefun-entry-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A deterministic Python 3.13 chess engine written from scratch in one recorded
agent session. The standalone main.py is 4,088 bytes, including the official
make.fun LZMA/Latin-1 unpacker. Only Python's standard library is used.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:PORT python3.13 main.py
```

The engine POSTs JSON to /game and /move. It reconstructs the supplied complete
move history for repetition detection. Search uses a fixed 40,000-node maximum,
no clock, and the last completed iteration's move. Exhausting the node iterator
unwinds every move through finally before returning.

The 0x88 board supports castling, en passant, all promotions, and king-safety
filtering. Incremental Zobrist hashes use a fixed seed; repetition keys include
en passant only when legally available. Threefold repetition, the 100-ply rule,
stalemate, and the competition's insufficient-material cases are recognized;
checkmate takes precedence over draw counters.

Search combines iterative deepening, aspiration windows, PVS, alpha-beta,
check extensions, late move reductions, reverse futility, quiescence, MVV-LVA,
and killers. A bounded 16,384-slot transposition table includes reversible-history
context. Evaluation uses all published PeSTO middlegame/endgame values, tapered
by material, plus bishop pairs and passed pawns. The PeSTO endgame king table
encourages king activity. Precomputed move rays and a bounded incremental pawn-hash cache avoid repeated work.

Only main.py, README.md, LICENSE, and .makefun/owner.json are committed.
The new engine_new.py and test_new.py remain local; the official transcript
bundle includes the readable source and the session that wrote it. Rebuild:

```sh
node /tmp/kit/cli/makefun.mjs compress engine_new.py --out main.py
```

Evaluation data: Ronald Friederich's
[PeSTO tables](https://www.chessprogramming.org/PeSTO%27s_Evaluation_Function).
Move generation, search, HTTP client, and tests were independently authored
in this session; no previous engine implementation was used.

---

<sub>Entered on [make.fun](https://make.fun/?ref=f2nqa25y&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
