# tiny-chess-01 starter

A working entry to fork: `main.py` plays every game through the env and
picks the move that captures the most valuable piece. It is a template,
not a contender. Replace its one-line choice with a real search.

## The size rule

- `main.py` is the whole engine: at most 4,096 bytes as committed,
  Python 3.13, standard library only. It reads, imports or execs no other
  file in the repository.
- Every byte counts: comments, whitespace, docstrings.
- Compression is allowed and counted. A `main.py` that unpacks and execs
  a compressed payload (`lzma`, `zlib`) is fine; the payload and the
  unpacker are both part of its 4,096 bytes.
- The rest of the tree (this README, LICENSE) is at most 8,192 bytes in
  all, and no file over 4,096. MIT, BSD, ISC and 0BSD licences fit.
- Search to a fixed depth or node count, never a clock: the trial is
  replayed and must play the same moves.

The search walks an 0x88 board with alpha-beta pruning.
Quiescence search settles captures before scoring.
A transposition table remembers searched positions.
Killer and history scores order moves for cutoffs.
Null-move pruning and late reductions trim the tree.
Iterative deepening raises the depth one ply at a time.

## Grade it locally

You need Docker and the `makefun` CLI. Download the competition's dev
kit, unpack it, and from inside it run:

    makefun grade-local --kit . --bot <your repository> --driver docker

It plays one practice trial against the house engines, replays it, and
prints the Elo the scorer gives it. Before you submit, `makefun check`
runs the site's own pre-submit checks, size caps included.

## Final trial results

This entry is engine13: 0x88 alpha-beta with quiescence search, a
transposition table, killer/history heuristics, null-move pruning (R=2),
LMR and iterative deepening, at a fixed 100k nodes per move. The packed
`main.py` is 4,057 bytes. Env-server practice trials:

- trial-0: 9/16 (6W 6D 4L), Elo 1464 (baseline 1326, +138)
- trial-1: 9.5/16 (7W 5D 4L), Elo 1497 (baseline 1464, +33)
- trial-2: 8.5/16 (7W 3D 6L), Elo 1430 (baseline 1290, +140)

Total 27/48. Mean Elo 1463.7 vs baseline mean 1360.0 (+103.7).

Three trials ran against the baseline at 100k nodes.
The engine took 9, 9.5 and 8.5 points from the trials.
Its mean rating was 1463.7 against a baseline of 1360.0.
The paired interval runs from +0.5 to +206.9, so the gate holds.

## Prize gate

90% paired t-interval of the per-instance Elo lead: [+0.5, +206.9].
The lower bound is above 0, so the prize gate is met: the entry's mean
Elo is above the baseline mean (sunfish 4 KB entry) with the 90% paired
t-interval of its per-instance lead above 0.

## Build notes

The entrypoint spreads its packed payload across several lines.
Each line carries one slice of the compressed engine.
The bytes on disk match the generated packed file exactly.
