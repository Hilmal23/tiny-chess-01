# tiny-chess-01

A compact Python chess entry for the make.fun environment-server chess task. The entry point is `main.py`: it stores the engine as base85-encoded, LZMA-compressed Python source and executes that source at startup. It uses the Python standard library and does not load another repository file.

## Running the entry

The competition harness supplies the game environment and calls `main.py` to select moves. Running the file without that environment is not a local chess game or a UCI session. Searches use a fixed node budget, rather than a clock, so replaying a position produces the same choice.

## Engine

The decoded engine searches a 0x88 board with alpha-beta and quiescence search. It uses iterative deepening, a transposition table, killer and history ordering, null-move pruning, and late-move reductions. Zobrist keys use the fixed seed `20261111`. The encoded payload is part of the engine: changing it changes play.

## Size and license

`main.py` must fit in 4,096 bytes; the entire submitted snapshot must fit in 8,192 bytes, with no file larger than 4,096 bytes. MIT license; see `LICENSE`.

