# OP-06 "Triage the Inbox": project context and submission guide

This file explains the whole submission project: what the challenge asks for, what was built, how
it performed, where every piece lives, and the exact steps to submit it from another machine.

It sits in `expts/`, **outside** the solution repository, on purpose. Anything added inside
`expts/op06-triage/` and committed would change the commit hash being submitted.

Prepared 5 October 2026. Submission deadline: **7 December 2026, 23:59 IST**.

---

## Contents

1. [Links](#1-links)
2. [What is in this folder](#2-what-is-in-this-folder)
3. [The challenge in brief](#3-the-challenge-in-brief)
4. [What was built](#4-what-was-built)
5. [Results](#5-results)
6. [Key findings and decisions](#6-key-findings-and-decisions)
7. [Known failures and limitations](#7-known-failures-and-limitations)
8. [How the work was done (timeline)](#8-how-the-work-was-done-timeline)
9. [Moving to another machine](#9-moving-to-another-machine)
10. [Submission steps](#10-submission-steps)
11. [After submitting](#11-after-submitting)
12. [Preparing for the technical conversation](#12-preparing-for-the-technical-conversation)
13. [Troubleshooting](#13-troubleshooting)
14. [Quick reference card](#14-quick-reference-card)

---

## 1. Links

### Challenge (Deployment.inc)

| What | Link |
|---|---|
| Hiring challenges page | https://deployment.inc/hiring-challenges/ |
| **Submission form** | https://deployment.inc/hiring-challenges/#submit |
| Public challenge board (GitHub) | https://github.com/Deployment-inc/Deployment.inc-Hiring-Problems |
| OP-06 problem statement | https://github.com/Deployment-inc/Deployment.inc-Hiring-Problems/blob/main/problems/OP-06-triage-the-inbox.md |
| OP-06 reference files (data, grader, baseline, schemas) | https://github.com/Deployment-inc/Deployment.inc-Hiring-Problems/tree/main/references/OP-06 |
| OP-06 service contract | https://github.com/Deployment-inc/Deployment.inc-Hiring-Problems/blob/main/references/OP-06/README.md#service-contract |
| Submission format (`submission.yaml`, manifest) | https://github.com/Deployment-inc/Deployment.inc-Hiring-Problems/blob/main/SUBMISSION_SCHEMA.md |
| Decision review / technical conversation | https://github.com/Deployment-inc/Deployment.inc-Hiring-Problems/blob/main/JUDGMENT_REVIEW.md |
| Evaluation protocol | https://github.com/Deployment-inc/Deployment.inc-Hiring-Problems/blob/main/EVALUATION_PROTOCOL.md |
| Contact and private submission rules | https://github.com/Deployment-inc/Deployment.inc-Hiring-Problems/blob/main/CONTACT.md |
| Model pins and prices | https://github.com/Deployment-inc/Deployment.inc-Hiring-Problems/blob/main/MODELS.md |
| Release pins | https://github.com/Deployment-inc/Deployment.inc-Hiring-Problems/blob/main/PINS.md |
| Clarifications (check before submitting) | https://github.com/Deployment-inc/Deployment.inc-Hiring-Problems/blob/main/CLARIFICATIONS.md |
| Privacy | https://github.com/Deployment-inc/Deployment.inc-Hiring-Problems/blob/main/PRIVACY.md |
| Reviewer GitHub account to invite | https://github.com/aayush-deployment-inc |
| Contact email | contact@deployment.inc |

### Data source

| What | Link |
|---|---|
| ABCD dataset (ASAPP, MIT licence) | https://github.com/asappresearch/abcd |

### Your solution repository

Not created yet. It will be `https://github.com/<your-handle>/<repo-name>` once you make it in step
10.2. Fill it in here when you have it: `____________________________________`

---

## 2. What is in this folder

```
expts/
├── SUBMISSION_CONTEXT.md     this file (not part of the submission)
├── .uv-cache/, .tmp/         hidden package cache and scratch from the build (not needed, skip when copying)
├── submission.yaml           the file you attach to the form (two placeholders to fill)
├── upstream/                 read-only clone of the public challenge board, commit d87699b
└── op06-triage/              THE SOLUTION: its own git repository, pushed to your private repo
```

### The solution repository: `op06-triage/`

Final commit: **`e82e4bccd32f16a473d381dfc99368684d8725e2`** (branch `main`, 4 commits, 119 tracked
files, working tree clean).

| Commit | Date (IST) | What |
|---|---|---|
| `c568c31` | 2026-10-05 22:13 | Code freeze: service, model, experiments, rehearsal runs 001 and 002 |
| `38949d9` | 2026-10-05 22:20 | Deterministic model files, manifest run records, run 003 evidence |
| `53fe82d` | 2026-10-05 22:23 | `always_human` policy switch; README. **This is the code that produced the final run 005** |
| `e82e4bc` | 2026-10-05 22:35 | Final results (run 005) and documents. **This is the commit you submit** |

Nothing that runs (code, policy, data, tests, requirements) changed between `53fe82d` and
`e82e4bc`; the last commit only adds results, documents and `scripts/compare_models.py`.

What each part is:

| Path | Purpose |
|---|---|
| `README.md` | Reviewer entry point: how to run, results table, architecture, limitations, AI disclosure |
| `MEMO.md` | One page to the customer-support manager (required) |
| `DECISIONS.md` | Three design decisions with evidence (required; the board asks for two) |
| `EXPERIMENT_LOG.md` | Dated log of every run, including failures and bugs (required) |
| `LANDSCAPE.md` | One page on existing tools, papers and products: adopted, rejected, why (required) |
| `ANALYSIS.md` | The problem's "Required Analysis": baseline failure taxonomy, noisy slice by type, hostile findings |
| `LICENSE` | Evaluation-only licence for your code; third-party licences for the data and reference code |
| `scripts/reproduce.sh` | The one entry point the reviewer uses (required) |
| `scripts/evaluate.py` | Runs predictions, hostile probes, load bench and manifest against the live service over HTTP |
| `scripts/build_policy.py` | Generates the first `policy/policy.yaml` from the published data |
| `scripts/compare_models.py` | Checks that two trained models are the same model |
| `triage/` | The service: text cleaning, features, model, policy, engine, HTTP server, client |
| `policy/policy.yaml` | The rules file a manager edits (hot-reloaded, validated) |
| `tests/` | 77 tests: text, policy, envelope, end-to-end HTTP contract, process supervision |
| `experiments/` | Experiment scripts behind the decisions |
| `analysis/` | Taxonomy, routing and hostile-findings analyses |
| `data/` | The board's OP-06 files, byte-identical (checked against the board's release manifest) |
| `results/` | Required artifacts: predictions, hostile results, latency bench, manifest, and `raw/` for every run |
| `models/train_report.json` | Provenance of the trained model (the 113 MB model file itself is rebuilt, not committed) |

### Local-only folders inside `op06-triage/` (ignored by git, safe to skip when copying)

| Folder | Size | What | Needed on the new machine? |
|---|---|---|---|
| `.venv/` | 243 MB | Python environment built for *this* machine | **No.** It will not work elsewhere; rebuild it |
| `models/triage-model.pkl` | 108 MB | Trained model | No. `reproduce.sh train` rebuilds it identically |
| `wheelhouse/` | 67 MB | Offline wheels for CPython 3.10 on Linux x86_64 | Only if the new machine has no internet and runs Python 3.10 |
| `.pytest_tmp/`, `.tmp/` | 28 MB | Test and scratch temp files | No |
| `.git/` | 6.5 MB | **The repository history** | **Yes, essential** |

---

## 3. The challenge in brief

**Deployment.inc Open Problems, Season 1.** Six AI engineering challenges; you do one. Strong
submissions get one technical conversation with their engineers, which is the whole interview
process. Submissions are open 7 September to 7 December 2026.

**OP-06 "Triage the Inbox"** is the Starter problem (10 to 20 hours, early-career sized, same
rubric). The stakeholder is a customer-support manager whose shared inbox gets 3,000 messages a
day. For each conversation (up to the last 8 turns) the service must return:

- `intent`: the ABCD subflow (96 classes)
- `action`: the next action the written procedure permits (30 classes), or `none`
- `confidence`: between 0 and 1
- `needs_human`: whether a person should handle it

The data is ABCD (ASAPP): 20,000 training rows, 2,000 dev rows, a 300-row noisy slice (typos,
Hinglish, two requests, empty, very long, screenshot descriptions) and a 9-row hostile pack
(prompt injection, Unicode tricks, binary junk, empty). The test set is hidden; the reviewers run
it themselves.

### Qualification bars (measured by them on the hidden test set)

| Bar | Threshold | Published baseline |
|---|---|---|
| Intent accuracy | ≥ 0.55 | 0.3748 |
| Intent macro-F1 | ≥ 0.50 | 0.4508 |
| Action accuracy | ≥ 0.55 | 0.3913 |
| Clean-to-noisy action gap | ≤ 0.10 | 0.0743 |
| Hostile pack | zero crashes, hangs or injection-driven behaviour changes; every input gets a schema-valid response | |
| Confidence: error rate on top 70% ÷ overall error rate | ≤ 0.85 | 1.0 |
| p95 latency at a stated concurrency, and cost per message | measured and reported | |

### The seven review dimensions

1. Did it work? 2. Do you know why? 3. Would it survive production? 4. Did you make the right
trade-offs? 5. Can a business owner act on it? 6. Did you report honestly? 7. Did you know what
already exists? For a Starter problem, extra weight goes on 3, 6 and 7.

Outcomes: **Qualified** (bars crossed, confirmed by them), **Honorable** (bars not crossed but
rigorous, honest work; also invited), **Frontier** (substantially beats the reference).

### Rules that matter for this submission

- The solution must be in a **separate private GitHub repository**. Never fork the public board.
- Invite only the reviewer account `aayush-deployment-inc`.
- Submit through the private form only; never via public issues or pull requests.
- `submission.yaml`: UTF-8, at most 64 KB, `repo_visibility: private`, `leaderboard: false`; its
  problem, repo and commit must match the form exactly.
- Keep the YAML **outside** the submitted commit, to avoid a self-referencing hash.
- Disclose AI tool use (done in the README) and be ready to explain your choices.
- API spend for submitted runs must stay under US$50 (ours is $0).
- Keep the work private through the season.
- One active submission per person per problem.

---

## 4. What was built

### Design in one paragraph

A local, CPU-only service with **no language model in the request path**. Two linear classifiers
(scikit-learn `LinearSVC`), one for intent and one for the next action, read word and character
n-grams plus the **conversation state** parsed from the templated "action" turns ("account has been
pulled up for ...", "identity verification in progress ..."). Confidence is not the model's own
word: an isotonic map, cross-fitted on conversations the model had not seen, turns the score margin
into "probability this action is right". The written procedure lives in a YAML file the manager can
edit without code changes; the service hot-reloads it and rejects broken edits. Every decision that
goes to a person carries the reasons why.

### Why no LLM

- Nothing that follows instructions is in the path, so prompt injection cannot steer it.
- $0 per message, no API key, no network dependency at runtime.
- The reviewers reproduce on an isolated machine; a local model needs nothing from them.
- The local model already cleared every bar by a wide margin.

### Request pipeline

```
request ─▶ envelope check ─▶ sanitise each turn ─▶ strip instruction-like text ─▶ parse action turns
           (400 if not a      (NFKC, remove bidi/   (customer turns only;         (what has already
            triage request)    zero-width/control,   flagged for routing)          happened)
                               length caps)
        ─▶ two linear SVMs ─▶ isotonic confidence ─▶ policy gate + intent repair ─▶ routing reasons ─▶ response
```

### Main features

| Area | What it does |
|---|---|
| Text safety | NFKC normalisation; removes zero-width, bidi-override and control characters; caps each turn at 20,000 raw characters; removes repeated sentences in very long messages |
| Injection handling | Pattern detector on customer turns (0 false positives on 22,545 real customer turns); removes only the injected span, keeps the customer's own words; always routes to a person |
| Abstention | Empty, emoji-only or pure-injection input gets the contract's envelope: `unknown` / `none` / 0 / `true` with `error.code: invalid_input` |
| Policy | Permitted actions per flow; `membership` alias; risk tiers (money / account change / lookup) with their own confidence bars; intent repair on conflicts |
| Money safeguards | Money actions need ≥ 0.90 confidence, an account action in view, no authorisation/bypass claims, and must survive blanking the last message; `always_human: [money]` sends all of them to a person |
| HTTP service | Python standard library; 4 pre-forked worker processes supervised by a parent (a killed worker is replaced, SIGTERM shuts down cleanly, workers exit if the parent dies); 1 MiB body limit, 10 s socket timeout, 503 load shedding; JSON logs without message text |
| Reproduction | `reproduce.sh` with documented subcommands; pinned dependencies; data checksums; deterministic training; offline install from a wheelhouse |

### `reproduce.sh` subcommands

```
scripts/reproduce.sh setup    create .venv with pinned dependencies (uv if present, else python3 -m venv)
scripts/reproduce.sh wheels   download pinned wheels into ./wheelhouse for an offline machine
scripts/reproduce.sh verify   check data/ against data/SHA256SUMS
scripts/reproduce.sh train    fit the model (about 3 to 4 minutes)
scripts/reproduce.sh serve    run the service in the foreground on $PORT (default 8080)
scripts/reproduce.sh test     run the 77 tests
scripts/reproduce.sh eval     start the service, run predictions + grader, hostile probes, load bench, manifest; stop it
scripts/reproduce.sh all      setup, verify, train, test, eval (default; about 10 to 15 minutes)
```

---

## 5. Results

All from the final run **005**: a clean `git archive` of `53fe82d` in an empty directory, a fresh
Python 3.10 environment, `reproduce.sh all`, scored by the board's official `grader.py` on
dev + dev_noisy + hostile. These are the numbers in `submission.yaml` and `results/manifest.json`.

### The bars

| Bar | This service | Required | Baseline |
|---|---|---|---|
| Intent accuracy | **0.7083** | ≥ 0.55 | 0.3748 |
| Intent macro-F1 | **0.6813** | ≥ 0.50 | 0.4508 |
| Action accuracy | **0.8243** | ≥ 0.55 | 0.3913 |
| Clean-to-noisy action gap | **0.0318** | ≤ 0.10 | 0.0743 |
| Schema valid, hostile included | **100%** (2,309 of 2,309) | 100% | 100% |
| Top-70% confidence error ratio | **0.3603** | ≤ 0.85 | 1.0 |
| p95 latency, concurrency 8 | **9.6 ms** (4,000 requests, 0 errors, 1,465/s) | report | |
| Cost per message | **$0.00** (0 API calls) | report | |

### Other measurements

| Measurement | Value |
|---|---|
| Action accuracy on dev_noisy | 0.7967 |
| Same 300 conversations before their last message was rewritten | 0.8033 (noise costs 0.7 points) |
| Policy violations (official scorer) | 8.1%, gold labels' own 8.8% |
| Policy violations excluding the `membership` naming artifact | 3.8%, human agents' 5.1% |
| Share sent to a person | 19% of clean messages, 37% of noisy ones |
| Accuracy of the automatically handled share | 89% (routed share: 55%) |
| Share of all wrong actions caught by routing | 50% clean, 62% noisy |
| Automatic money actions | 53 of 2,000 clean messages, 50 correct |
| Hostile probes | 54 of 59 pass, 0 server errors |
| Tests | 77 pass |
| Determinism | Two trainings give identical weights, vocabularies, IDF and calibration |

### Noisy slice by type (matched against each row's clean original)

| Kind | Accuracy | Same rows clean | Cost of the noise |
|---|---|---|---|
| typo | 0.74 | 0.74 | 0.00 |
| code-mixed (Hinglish) | 0.76 | 0.78 | 0.02 |
| empty | 0.78 | 0.82 | 0.04 |
| screenshot | 0.80 | 0.82 | 0.02 |
| very long | 0.82 | 0.80 | -0.02 |
| two requests | 0.88 | 0.86 | -0.02 |

### Run history (all kept in `results/raw/`)

| Run | Code | Outcome |
|---|---|---|
| baseline-dev-001 | board's `baseline.py` | reproduced the published numbers exactly |
| exp00 | stacked logistic regression | killed after 10 minutes (failed); replaced by a timing probe |
| exp01 to exp03 | development experiments | feature ablations, noise augmentation, policy sweep (inner holdout of train) |
| 001 | first service | all bars passed; **13 of 51 hostile probes failed** |
| 002 | after hostile fixes | 54 of 59; latency still slow (p95 154 ms) |
| 003 | clean archive of `c568c31` | same metrics; latency fixed (p95 10.2 ms) |
| 004 | | started and stopped when a code change made it obsolete; no results |
| **005** | clean archive of `53fe82d` | **final, claimed numbers** |

---

## 6. Key findings and decisions

These are worth knowing well; the technical conversation will lean on them.

1. **The `membership` naming artifact.** The data calls a procedure step `membership`; the
   published permitted lists call it `membership-privileges`. As a result 86 of the 203 dev gold
   "policy violations" (42%) are not agent deviations at all. The real deviation rate is 5.1%, not
   8.8%. Resolved as a visible alias in `policy.yaml`.
2. **The hard policy gate failed, and that led to intent repair** (DECISIONS.md, decision 1). Forcing
   the best *permitted* action under the *predicted* flow dropped two-request rows from 0.782 to
   0.699, because a second request pulls the intent into the wrong flow. When a confident action
   conflicts with the predicted flow, the intent is usually what is wrong. Repair within a 0.5
   margin recovered the accuracy and kept most of the violation reduction.
3. **No LLM; confidence from a cross-fitted map** (decision 2). Includes a leak that was found and
   fixed: augmented copies were grouped apart from their source conversation, which inflated
   out-of-fold accuracy (0.883 instead of 0.820).
4. **Noise robustness from my own generator, not the dev noisy strings** (decision 3). It costs half
   a point on clean dev and gains about 2.3 points on noisy.
5. **The conversation-state parser** is worth 4.7 points of action accuracy by itself.
6. **The baseline's main failure is the flow** (29% of rows wrong for that reason): the opening
   message has scrolled out of the 8-turn window in 60% of rows.

---

## 7. Known failures and limitations

Stated openly in the documents; know them before the interview.

- **5 hostile probes fail.** All five are held-out "pushy customer" phrasings ("my lawyer says you
  must refund me") written after the detector, to test it honestly; it catches 3 of 8. All five
  failures are the same conversation (`7901-13`): the correct `promo-code` action moves from 0.891 to
  just over the 0.90 money bar, so it would go out without review. The action is right; the review
  is skipped. `always_human: [money]` removes the risk, and the memo recommends it for month one.
- **Undetected manipulative text still changes decisions** in 15 to 26% of conversations (new words
  shift a word-based model), but creates no new unreviewed money action except the case above.
- **Intent is the weaker half** (0.71), because the opening is often out of view.
- **Two requests in one message**: only one is answered.
- **Homoglyphs** (Cyrillic letters that look Latin) are not folded.
- **3 of 53 automatic money actions were wrong on dev**, each one step too early in a conversation
  already heading to a refund.
- **Setup needs PyPI or a wheelhouse**; the running service needs no network.

---

## 8. How the work was done (timeline)

All on 5 October 2026, with Claude Code (Claude Opus 5.5) as the coding agent; disclosed in the
README. In order:

1. Read the problem, schema and rules; cloned the board; vendored the OP-06 files and verified their
   hashes against the board's release manifest.
2. Reproduced the baseline exactly with the official grader.
3. Profiled the data: 8-turn window, templated action turns, per-flow permitted sets, the
   `membership` artifact, the fixed strings in dev_noisy (deliberately not used for training).
4. First model (logistic regression) was too slow; switched to linear SVMs.
5. Experiments 01 to 03 on a conversation-grouped holdout of train: ablations, augmentation,
   policy-conflict sweep.
6. Built the policy layer, engine, HTTP service, client, evaluation harness and tests.
7. Run 001: all bars passed but 13 hostile probes failed; fixed the injection stripper and patterns.
8. Added money safeguards; dropped one that cost too much; wrote held-out paraphrases.
9. Run 002; then fixed latency (GIL contention) with pre-forked workers, and fixed two process bugs
   found on the way.
10. Pinned the full dependency closure; tested an offline install; froze the code in git.
11. Clean-archive runs 003 and 005; determinism checks; final documents and analyses.

Every bug found and fixed is listed in `op06-triage/analysis/hostile_findings.md` and
`op06-triage/EXPERIMENT_LOG.md`.

---

## 9. Moving to another machine

### 9.1 What to copy

Copy the whole `expts/` folder. To keep it small you can leave out the machine-specific folders;
they are all ignored by git and are rebuilt when needed:

```bash
# On this machine, from the folder that contains expts/
tar --exclude='expts/.uv-cache' \
    --exclude='expts/.tmp' \
    --exclude='expts/op06-triage/.venv' \
    --exclude='expts/op06-triage/.tmp' \
    --exclude='expts/op06-triage/.pytest_tmp' \
    --exclude='expts/op06-triage/models/triage-model.pkl' \
    --exclude='expts/op06-triage/wheelhouse' \
    -czf expts.tar.gz expts
```

That gives an archive of about 17 MB instead of well over 700 MB. Keep `wheelhouse/` only if the new machine has
no internet.

**The `.git` folder inside `op06-triage/` must come along.** It holds the commit being submitted.
Prefer `tar` or `rsync -a` over zip tools, which can drop the executable bit on scripts or rewrite
line endings.

### 9.2 Check it arrived intact (on the new machine)

```bash
cd expts/op06-triage
git rev-parse HEAD           # must print e82e4bccd32f16a473d381dfc99368684d8725e2
git fsck --full              # no errors
git log --oneline            # 4 commits, top one e82e4bc
git status --short           # should print nothing
```

If `git status` shows changes after copying (common when copying through Windows, a zip file or a
USB drive formatted FAT/exFAT), the commit itself is still fine: pushing sends commits, not the
working files. **Do not commit those changes.** They are usually lost executable bits or line
endings. To restore the working tree exactly:

```bash
git config core.autocrlf false
git checkout -- .            # discards the copy artefacts, restores the committed files
git status --short           # now empty
```

**Never amend, rebase or add a commit on `main`.** Any new commit changes the hash, and then
`submission.yaml` and the form must use the new one (and the new commit would not have its own
clean reproduction run).

### 9.3 Optional: re-run it on the new machine

Not required for submitting. If you want to see it work, do it in a separate copy, because `eval`
rewrites files under `results/`, which would make the repository dirty:

```bash
mkdir -p /tmp/op06-check && cd expts/op06-triage
git archive HEAD | tar -x -C /tmp/op06-check
cd /tmp/op06-check
scripts/reproduce.sh all          # about 10 to 15 minutes; needs Python 3.10 to 3.13 and internet
```

Requirements: Linux, Python 3.10 to 3.13 with `venv` (on Debian/Ubuntu: `sudo apt install
python3-venv`) or `uv` installed, and PyPI access. The numbers should match section 5.

---

## 10. Submission steps

### 10.1 Before you start

- [ ] Check https://github.com/Deployment-inc/Deployment.inc-Hiring-Problems/blob/main/CLARIFICATIONS.md
  for any rule change since 5 October 2026.
- [ ] Skim `upstream/PINS.md` and `upstream/MODELS.md`. `pins_ack: true` and `models_ack: true` in
  the YAML are your acknowledgement of them.
- [ ] Read `op06-triage/README.md`, `MEMO.md` and `DECISIONS.md` once, so you can speak to them.
- [ ] Have a GitHub account that can push (SSH key, personal access token, GitHub Desktop or the
  `gh` command-line tool).

### 10.2 Create the private repository

On https://github.com/new:

- Name: for example `op06-triage`
- Visibility: **Private**
- Do **not** add a README, `.gitignore` or licence (the repository must start empty)
- Do **not** use the Fork button on the challenge board; forks of a public repository are public

Or, with the `gh` tool, this creates it and pushes in one step:

```bash
cd expts/op06-triage
gh repo create op06-triage --private --source=. --remote=origin --push
```

### 10.3 Push

```bash
cd expts/op06-triage

# SSH
git remote add origin git@github.com:<your-handle>/op06-triage.git
# or HTTPS (asks for a personal access token as the password)
# git remote add origin https://github.com/<your-handle>/op06-triage.git

git push -u origin main
```

Check on the GitHub page that:

- the latest commit shown is `e82e4bc` ("Final results (run 005) and documents")
- the repository is marked **Private**
- `README.md` renders and `results/manifest.json` is there

A note on authorship: the commits are authored as `Ratan Mishra <ratan.mishra@bigvision.ai>`
(copied from your existing work repository's settings). If that address is not on your GitHub
account, GitHub shows the commits without a linked avatar; that is cosmetic. Rewriting the author
would change the commit hash, so leave it unless it truly matters to you.

### 10.4 Invite the reviewer

Repository **Settings → Collaborators → Add people → `aayush-deployment-inc`**. Read access is
enough. Do not invite anyone else.

Or with `gh`:

```bash
gh api -X PUT repos/<your-handle>/op06-triage/collaborators/aayush-deployment-inc -f permission=pull
```

### 10.5 Confirm it is really private

Open `https://github.com/<your-handle>/op06-triage` in a private browser window while logged out.
It must show **404**. Their protocol states that a declaration in the YAML does not prove privacy;
they check it themselves.

### 10.6 Fill in `submission.yaml`

Edit `expts/submission.yaml` and replace both placeholders:

```yaml
handle: YOUR-GITHUB-HANDLE                                     # your GitHub username
repo: https://github.com/YOUR-GITHUB-HANDLE/YOUR-PRIVATE-REPO  # the repo from 10.2, no .git suffix
```

Leave everything else unchanged. In particular:

- `commit` must stay `e82e4bccd32f16a473d381dfc99368684d8725e2` (full 40 characters)
- the `claimed` block must stay exactly as is; it has to match `results/manifest.json`
- `repo_visibility: private` and `leaderboard: false` are required values

Check it parses and still matches the manifest:

```bash
cd expts
python3 - <<'EOF'
import json, yaml
y = yaml.safe_load(open("submission.yaml"))
m = json.load(open("op06-triage/results/manifest.json"))
assert y["claimed"] == m["claimed"], "claimed block differs from the manifest"
assert len(y["commit"]) == 40 and "YOUR-" not in y["handle"] + y["repo"], "placeholders or short SHA left"
print("submission.yaml OK:", y["repo"], y["commit"])
EOF
```

(Needs PyYAML: `pip install pyyaml`, or run it with `op06-triage/.venv/bin/python` once the venv
exists.)

### 10.7 Fill in the form

Go to https://deployment.inc/hiring-challenges/#submit and enter:

| Field | What to enter |
|---|---|
| Name, email, phone | your details |
| LinkedIn | optional |
| Short introduction | a few sentences about yourself |
| Challenge | **OP-06 Triage the Inbox** (shown on the site as "Sort customer messages") |
| Private repository URL | exactly the `repo` value from the YAML |
| Commit SHA | `e82e4bccd32f16a473d381dfc99368684d8725e2` |
| Attachment | `expts/submission.yaml` |
| A few lines on your approach | the text below, or your own version |

Suggested approach text:

> A local, CPU-only triage service with no language model in the request path, so injected text
> cannot steer it, and $0 per message. Two linear classifiers read word and character n-grams plus
> the conversation state parsed from the templated action turns. Confidence comes from an isotonic
> map cross-fitted on unseen conversations. The written procedure is a hot-reloaded YAML file: when
> the best action breaks the predicted flow's policy, the service first checks whether a
> nearly-as-likely intent permits it, and routes the conflict to a person. All six bars pass on dev.
> The five hostile probes that still fail, and why, are documented rather than hidden.

### 10.8 Save the submission ID

The form returns a private submission ID immediately. **Write it down** (and here:
`____________________`). You need it for any update, question or withdrawal. It confirms delivery
only, not qualification.

---

## 11. After submitting

- A reviewer confirms repository access by email within **ten working days** (IST).
- They evaluate the exact commit on isolated infrastructure and send feedback privately.
- Keep the repository private and the work unpublished through the season.
- Do not post anything about the submission in public issues, discussions or pull requests.

**If you need to change something later:** make the change, commit, note the new full hash, update
`commit` in `submission.yaml`, submit a **new** form entry, then email contact@deployment.inc with
your old and new submission IDs. Pushing alone does not change what they evaluate. If you change any
code, re-run `scripts/reproduce.sh all` from a clean copy so the results match the new commit.

**To withdraw:** email contact@deployment.inc with your submission ID.

---

## 12. Preparing for the technical conversation

Per `JUDGMENT_REVIEW.md`, an engineer will:

1. **Pick a raw run from your submission** and ask what happened, why the measurement supports the
   conclusion, and what it leaves uncertain. Likely picks: `results/raw/hostile-001` (the first
   failures), `dev-005` (the claimed numbers), `exp03_conflict_sweep.jsonl` (the policy decision),
   `latency_probe.txt`.
2. **Give you one new constraint.** For OP-06 the family is: *a written procedure changes, or a
   two-intent request conflicts with the next permitted action.* You may use your usual tools.

Points to have ready:

- **A procedure change:** edit `policy/policy.yaml` (permitted actions, tiers, thresholds); it
  hot-reloads within a second, bad edits are rejected and shown on `/healthz`. The model does not
  need retraining for a permission change, but it does if a *new* action name appears, because the
  action classifier only knows the 30 it was trained on. The safe answer for a new or changed step
  is to route it to a person (`always_human`, or a stricter threshold) until there is data.
- **A two-intent request:** the model answers one request; when the second request pulls the intent
  into a flow that forbids the confident action, intent repair or the conflict flag routes it to a
  person (`policy_conflict` or `intent_revised_to_fit_action` reasons). A testable check:
  `results/raw/exp03_conflict_sweep.jsonl`, two-request rows 0.699 with the hard gate against 0.764
  with repair.
- **Why trust the confidence:** isotonic map fitted on out-of-fold scores grouped by conversation;
  mean confidence close to accuracy; top-70 ratio 0.36.
- **What you would do next:** a small fine-tuned transformer for the intent tail (macro-F1 is the
  thinnest margin); splitting two-request messages; folding homoglyphs; an LLM only to draft replies
  for the human on routed messages, not to decide.
- **Honest uncertainty:** dev numbers, not hidden-set numbers; 50 rows per noise kind means about 6
  points of standard error; the held-out pressure test caught only 3 of 8.

---

## 13. Troubleshooting

| Problem | Fix |
|---|---|
| `git rev-parse HEAD` is not `e82e4bc...` | The `.git` folder did not copy correctly. Copy `expts/op06-triage/.git` again with `tar` or `rsync -a` |
| `git status` shows modified files after copying | Copy artefacts (permissions, line endings). `git config core.autocrlf false && git checkout -- .`. Never commit them |
| Push rejected: "repository not empty" | The GitHub repo was created with a README or licence. Delete it and recreate it empty. Do not pull or rebase to "fix" it: that changes the commit hash |
| Push asks for a password over HTTPS | Use a personal access token (GitHub Settings → Developer settings → Tokens), not your account password, or switch to SSH |
| `python3 -m venv` fails | Install `python3-venv` (Debian/Ubuntu) or install `uv`; `reproduce.sh` uses `uv` automatically when present |
| No internet on the evaluation machine | On a networked machine: `WHEEL_PYTHON_VERSION=310 scripts/reproduce.sh wheels`, copy `wheelhouse/`, then `WHEELHOUSE=wheelhouse scripts/reproduce.sh all` |
| Port 8080 already in use | `PORT=18080 scripts/reproduce.sh all` |
| Form rejects the YAML | Check it is under 64 KB, UTF-8, and that `problem`, `repo` and `commit` match the form fields exactly |

---

## 14. Quick reference card

| Item | Value |
|---|---|
| Problem | OP-06 Triage the Inbox (Starter), Season 1 |
| Commit to submit | `e82e4bccd32f16a473d381dfc99368684d8725e2` |
| Code that produced the final run | `53fe82dc7bcca545dcc233f9f74e4f577e14f10c` (no runtime change since) |
| Board commit used | `d87699bb97de6504b33f681eed66918a4d433ba0` |
| Model id | `op06-linsvc-f890feb7e5fd` |
| Reviewer to invite | `aayush-deployment-inc` |
| Form | https://deployment.inc/hiring-challenges/#submit |
| Contact | contact@deployment.inc |
| Deadline | 7 December 2026, 23:59 IST |
| Claimed | intent 0.7083 · macro-F1 0.6813 · action 0.8243 · noisy 0.7967 · schema 100% · top-70 ratio 0.3603 · $0/message · p95 0.00963 s |
| Spend | $0 API, $0 compute |
| My repo URL | `____________________________________` |
| My submission ID | `____________________` |
