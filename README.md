# Claude Agentic Coding Tutorial

A hands-on agentic coding tutorial for [Claude Code](https://claude.com/claude-code). You give the
agent a spec and let it build, test, visualize, and run a small PyTorch experiment for you.

The experiment itself is deliberately simple: classify points on two concentric circles, and show
that a linear model cannot do it while a tiny MLP can.

[Tutorial slides (PDF)](docs/6_7960_Agentic_Coding_Tutorial.pdf)

## How to use this repo

This repo ships intentionally incomplete. `experiment.py` and `tests/test_experiment.py` are
missing — writing them is the exercise.

1. Install [Claude Code](https://claude.com/claude-code) and open this directory in it.
2. Ask it to create the missing files, e.g. *"Recreate experiment.py and tests/test_experiment.py."*
3. Verify the result:
   ```
   python -m unittest discover -s tests
   python experiment.py --device cpu
   ```
   Then look at `outputs/decision_boundaries.png`.

Two files drive the agent's behavior, so keep both:

- **`AGENTS.md`** — the spec for what the missing files should do. The agent reads this automatically.
- **`.agents/skills/`** — reusable skills (`launch-experiment`, `visualize-experiment`) that
  `AGENTS.md` refers to for running experiments and making plots.

Requires only PyTorch, matplotlib, and the standard library. Nothing is downloaded at runtime.

## Credit

Adapted from [anakhag07/deep-learning-agentic-coding-tutorial](https://github.com/anakhag07/deep-learning-agentic-coding-tutorial),
the original tutorial for MIT 6.7960 (Deep Learning). All credit for the original material and
slides goes to that author.
