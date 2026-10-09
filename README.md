# Claude Agentic Coding Tutorial

An introductory tutorial to agentic coding with Anthropic's [Claude Code](https://claude.com/claude-code) command-line interface (CLI). Through this tutorial, you will:

1. Learn what an *agent* and ***agentic* coding** are
2. Set up a development environment for agentic coding
3. Give a Claude Code agent specifications through CLI **prompts** and **skill** Markdown (.md) files
4. Watch the agent build, run, and visualize results of a **coding experiment** for you

At the end of the tutorial, you will complete an experiment with the help of Claude Code to demonstrate your understanding of agentic coding tools. 

* The final experiment is derived from [this repository](https://github.com/anakhag07/deep-learning-agentic-coding-tutorial) created by the **Deep Learning (6.7960)** course at MIT; you can find a copy of their tutorial slides [here](docs/6_7960_Agentic_Coding_Tutorial.pdf)

Start with **Part 1** if *agent* is a new word to you. Skip to **Part 2** to begin installing, or straight to **Part 3** if you already have Claude Code and PyTorch set up.

---

# Part 1 — What an agent and agentic coding are

## From chatbot to agent

A **large language model (LLM)** on its own only produces text. You give it a prompt, it returns an answer, and that is the end of the exchange — it cannot open your files, run your code, or find out whether what it told you was true.

An **agent** is that same model given three additional things:

1. **Tools** — the ability to take real actions *(read a file, write a file, run a terminal command, search the web)*
2. **A loop** — permission to keep going: act, observe the result, decide the next action, repeat
3. **A goal** — the outcome you asked for, which the loop runs until it reaches *(or gets stuck)*

That loop is the whole idea. The model is no longer guessing what your code does; it runs your code and reads the error message.

| | LLM chatbot | Coding agent |
|---|---|---|
| **You provide** | a question | a goal |
| **It returns** | text to read | changed files and command output |
| **Can it check its own work?** | no | **yes** — it runs the tests and sees them fail |
| **Who does the typing?** | you, copying from the chat | the agent, in your actual repository |

## What makes coding *agentic*

***Agentic* coding** is what happens when the tools in that loop are a developer's tools. More specifically, the agent can read your repository, write files, run your test cases, read the traceback, and try again — so a single prompt can turn into a dozen actions you never had to type.

The practical consequence is a shift in what you write. You stop writing **instructions** and start writing **specifications**:

- *Instruction:* "Add a ReLU after the first linear layer on line 34"
- *Specification:* "Train both models on the same split and show me that the MLP beats the linear model by at least 15 points"

You describe the outcome you want and how you will know it worked. The agent figures out the steps. In this tutorial those specifications live in two places:
1. the **prompts** you type
2. the **Markdown files** in the repository that the agent reads on its own

## What does not change

The agent is fast and tireless, and it is also perfectly capable of writing code that looks right and is wrong. Three habits keep you honest:

- **Make it close the loop** – An agent that wrote a **test** AND **ran** it has given you evidence. On the other hand, an agent that *only* wrote a test has merely provided you with a guess, since it has *not been verified* by a test!
- **Check the artifact yourself** – For this experiment, that means opening the PNG and looking at the decision boundary with your own eyes.
- **Review what it changed** – You remain the author of your repository. *Agentic* means you delegated the typing, not all **judgment**.

---

# Part 2 — Set up your agentic coding environment

Two installs, in order: the agent itself *(steps 1–9)*, then the Python environment the experiment needs *(steps 10–13)*.

## Install Claude Code
Each install confirms the one before it, so you will be able to trouble-shoot any installation issues that may arise
* *Based on Anthropic's [Claude Code setup
guide](https://academy.claude.com/courses/building-with-the-claude-api/claude-code-setup)*

Open a new terminal

**1. Install [Homebrew](https://brew.sh/)** *(an open-source package manager)*
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Confirm that Homebrew was installed:
```bash
brew --version
```
*A version number means it worked. We will use this package manager to install Node.js below.*

**2. Install [Node.js](https://nodejs.org/)** *(an open-source environment for running JavaScript
code outside of a web browser)*
```bash
brew install node
```
Confirm that NodeJS was installed:
```bash
node -v
```

**3. Install [Claude Code](https://claude.com/claude-code)** *(Anthropic's closed-source
command-line interface + agentic coding tool)*
```bash
npm install -g @anthropic-ai/claude-code
```
Confirm that Claude Code was installed:
```bash
claude --version
```
*We will use Anthropic's Claude tool, as we can prompt it via terminal commands*

> **On Windows:** Homebrew is macOS/Linux only. Install Node.js from
> [nodejs.org](https://nodejs.org/), then run the same `npm install -g @anthropic-ai/claude-code`
> command. Git for Windows or WSL gives you a shell that behaves like the examples here.

### Login

**4. Change directory** *(to a folder you're comfortable with Claude Code accessing)*
```bash
cd "/Path/To/Your/Folder"
```
Example:
```bash
cd "/Users/ashleyneall/Downloads/MIT/Research"
```
*Claude Code can read and write anything underneath where you start it, so start it at a project
folder rather than your home directory*

**5. Log in to Claude Code**
```bash
claude
```
*This opens your browser the first time. Then agree to workspace access*

### Get Started

Now that we're logged into Claude Code via the terminal, let's set up a local project directory to experiment in:

**6. Make a new directory** *(within the folder that you've given Claude Code access to)*
```bash
mkdir Claude-Test
cd "/Users/ashleyneall/Downloads/MIT/Research/Claude-Test"
```

**7. Choose a Claude model**
```
/model
```
*Then select the model of your choosing with the arrow keys and Enter. Fun fact: this tutorial was partially written
using **Opus 5**!*

| Family | Best for | Latest |
|---|---|---|
| **Opus** | the most capable work | 5.5, 5 |
| **Fable** | complex tasks | 5.1, 5 |
| **Sonnet** | balanced speed and quality | 5.5, 5 |
| **Haiku** | fast, cheap, simple tasks | 5.5 |

*Older point releases (Opus 4.8–4.6, Sonnet 4.6/4.5, Haiku 4.5) still appear in the picker. `/model`
can be run at any time, mid-session.*

**8. Download this codebase**
```
!git clone https://github.com/aneall/claude-agentic-coding-tutorial.git
```
*Now you will see a sub-folder called **claude-agentic-coding-tutorial***

*The reason why we use **`!`** in front of the command is so that **Claude Code will know not to
interpret that as a prompt**, and instead **allow the terminal to simply run the bash command** to
clone this codebase.*

**9. Point Claude Code at the cloned folder.** Exit with `Ctrl+C` twice, then:
```bash
cd claude-agentic-coding-tutorial
claude
```
*Important note: `AGENTS.md` is only loaded automatically for a session started
inside the directory that contains it; if you were to start Claude Code one level up, the agent would never see the
specifications!*

## Useful Commands

Slash commands are typed **inside** a given Claude Code session:

| Command | What it does |
|---|---|
| `/help` | lists every available command |
| `/model` | switches which Claude Code model *(proprietary, closed-source code created by Anthropic)* to use |
| `/clear` | wipes the conversation and starts fresh — the fix for an agent that has gotten confused |
| `/compact` | summarizes the conversation so far to free up context without losing the thread |
| `/init` | writes a starter `CLAUDE.md` describing an existing codebase |
| `/memory` | opens the memory and instruction files the agent loads automatically |
| `/agents` | creates and manages subagents that handle tasks in their own context |
| `/config` | session settings — theme, model, permissions |
| `/cost` | shows token usage and cost for the session |
| `/status` | version, working directory, model, and login account |
| `/doctor` | diagnoses a broken install |
| `/mcp` | manages MCP servers *(external tools you connect the agent to)* |
| `/export` | saves the transcript so you can keep a record of a session |
| `/logout`, `/login` | switches accounts |

Prefixes and keys:

| Input | What it does |
|---|---|
| `!command` | runs the line as a shell command instead of reading it as a prompt |
| `@path/to/file` | pastes a file into the prompt as context *(Tab autocompletes the path)* |
| `#note` | saves a note to memory so it persists across sessions |
| `Esc` | interrupts the agent mid-work without killing the session |
| `Esc` `Esc` | rewinds to an earlier point in the conversation |
| `Shift+Tab` | cycles permission modes — ask every time, auto-accept edits, or plan only |
| `Ctrl+C` ×2 | exits Claude Code |

Terminal flags are passed when **starting** `claude`:

| Flag | What it does |
|---|---|
| `claude -c` | continues the most recent conversation in this folder |
| `claude -r` | resumes an older session, picked from a list |
| `claude -p "prompt"` | prints one answer and exits — for scripts and pipelines |
| `claude --model opus` | starts on a specific model |
| `claude --add-dir ../other` | grants access to a second folder |
| `claude --version` | prints the installed version |

## Set up Python for the experiment

New to Python environments? Follow these four steps once:

**10. Install Miniconda** (skip if `conda --version` already works in your terminal)

macOS, Apple Silicon:
```bash
curl -o ~/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh
bash ~/miniconda.sh -b && ~/miniconda3/bin/conda init zsh
```
macOS on Intel is the same with `MacOSX-x86_64.sh`; Linux uses `Linux-x86_64.sh` and
`conda init bash`. On Windows, download the installer from
[the Miniconda page](https://www.anaconda.com/docs/getting-started/miniconda/install) and use the
"Anaconda Prompt" it adds to your Start menu.

**Close and reopen your terminal** so `conda` is on your path

**11. Create an environment for this tutorial**
```bash
conda create -n circles python=3.11 -y
conda activate circles
```

**12. Install PyTorch and matplotlib into it**
```bash
pip install torch matplotlib
```
On Linux and Windows this pulls a GPU-enabled PyTorch (~2.5 GB). The experiment runs on CPU, so if
you have no NVIDIA GPU, take the much smaller CPU build instead:
`pip install torch matplotlib --index-url https://download.pytorch.org/whl/cpu`

**13. Check that it worked**
```bash
python -c "import torch, matplotlib; print(torch.__version__, matplotlib.__version__)"
```
Two version numbers means you're ready. Run `conda activate circles` in every new terminal before
working on this tutorial.

### What you just installed, and why

- **Miniconda** — creates isolated Python environments. The tutorial's packages live in `circles`
  instead of your system Python, so nothing here can break another project and deleting the
  environment undoes everything.
- **PyTorch** — the deep learning library. It supplies the tensors, the `nn.Linear`/`ReLU` layers,
  and the automatic differentiation that makes training a network a few lines of code.
- **matplotlib** — the plotting library, used to draw the decision boundaries. The point of the
  experiment is visual: you have to *see* the straight line fail and the curved one succeed.

Nothing else is downloaded, and nothing is fetched at runtime — the dataset is generated in code.

---

# Part 3 — Specify the work with prompts and skill files

This coding experiment entails classifying points on two concentric circles, and showing that a linear model cannot do it while a tiny MLP can.

In order to gain experience working with Claude Code in the terminal, this repository is intentionally incomplete: `experiment.py` and `tests/test_experiment.py` are
missing, and thus writing them is the exercise.

The specification for those two files is already written — just not by you, and not in a prompt. It is sitting in the repository, which is what makes the prompt in Part 4 so short.

## The three kinds of files that steer the agent

Keep all of them — they do different jobs for different readers:

| File | Written for | Does Claude read it? |
|---|---|---|
| `README.md` | **You** *(the human)*: setup, context, describe goals via natural language or code. | Only if you ask, or if it goes looking. It is not loaded automatically, so never hide requirements here. |
| `AGENTS.md` | **Agent**: the spec: what to build, which constraints hold, which skills to use. | **Yes, automatically** — Claude Code loads it at the start of every session in this directory and treats it as standing instructions. |
| `.agents/skills/<name>/SKILL.md` | **Agent**: one reusable procedure per folder (here: `launch-experiment`, `visualize-experiment`). | Yes, when the task calls for it — `AGENTS.md` names them, so Claude opens the matching `SKILL.md` and follows those steps. |

Note the singular filename: each skill is a folder containing a `SKILL.md`, with a `name` and a
`description` in its front matter. The description is what tells the agent when the skill applies.

The practical rule: **anything the agent must obey goes in `AGENTS.md` or a skill, not the README.**
The README explains the project to a person; `AGENTS.md` is the contract the agent is held to; a
skill is a checklist it reuses whenever that kind of task comes up.

## Open the two spec files and read them

Before prompting anything, look at what the agent has already been told:

```bash
cat AGENTS.md
cat .agents/skills/launch-experiment/SKILL.md
```

`AGENTS.md` is the **what** — the dataset size, the two model architectures, the training steps, the accuracy thresholds, the output path. The skills are the **how** — the procedure for launching a run and the rules for making a plot, written once and reused for every experiment.

This is the division worth internalizing: a **prompt** is for this one task, right now. A **skill file** is for the kind of task you will ask for again next week.

---

# Part 4 — Watch the agent build, run, and visualize the experiment

1. Start Claude Code in this directory (`claude`), with the `circles` environment active
2. Ask it to build the missing files. This is typically typed in natural language via the CLI, and below is an example:

   > Build the two missing files this project describes: the experiment script and its unittest.
   > Then run the tests and the experiment, and show me the figure it produces.

3. Verify the result yourself:
   ```bash
   python -m unittest discover -s tests
   python experiment.py --device cpu
   ```
   Then look at `outputs/decision_boundaries.png`

### Why that prompt works

It names the goal and stops. It does not restate the circle dataset, the layer sizes, the number of
training steps, the accuracy thresholds, or where the PNG goes — all of that already lives in
`AGENTS.md`, which the agent has loaded before you type anything. Repeating a spec the agent can
already read is wasted effort, and when the two copies drift apart it is actively harmful.

Asking it to *run* the tests and the experiment matters just as much as asking it to write them.
That is the difference between code that looks plausible and code you have watched produce a
number. Make the agent close the loop, then check the figure with your own eyes.

### What you should see

The linear model lands near 50% on the held-out points — a straight line cannot separate an inner circle from a ring around it, so it does no better than guessing. The MLP clears 90%. In the figure, the left panel is split by one straight boundary and the right panel has learned a closed curve around the inner circle.

If your numbers come out very different, that is worth chasing rather than shrugging at: ask the agent why, and make it show you.

### Then steer it

Once it works, try changing one thing at a time:

- Ask what happens with **4 hidden units** instead of 16
- Move the two circles close enough to **overlap**
- Cut training to **20 steps** and see which model suffers more

Changing one thing and rerunning is the whole experimental loop, compressed — and it is the part where the agent actually earns its keep, since each variation costs you one sentence instead of ten minutes of editing.

---

# More Agentic Tools

*Coming soon!*

## Credits

Adapted from [anakhag07/deep-learning-agentic-coding-tutorial](https://github.com/anakhag07/deep-learning-agentic-coding-tutorial),
the original tutorial for MIT 6.7960 (Deep Learning). All credit for the original material and
slides goes to that author.
