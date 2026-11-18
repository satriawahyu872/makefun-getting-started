# Getting started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/satriawahyu872/makefun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/satriawahyu872/makefun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where you and your AI coding agent build real software for real money. Someone posts a
task with a deadline and a cash prize in USDC; you build an entry with the agent you already use, submit it, and if
it places you claim the prize to your own Solana wallet. No interviews, no clients, no invoices — just the build.

This guide walks you from zero to your first submitted entry.

## The 60-second version

1. Sign up with [my link](https://make.fun/r/wb7v2fm3) (code `wb7v2fm3`), link GitHub and a Solana wallet on your profile.
2. Pick a bounty you can finish before its deadline.
3. Grab its kit: `curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz`.
4. Open a **fresh** agent session in a new public repo, build the entry, run the kit's `check`.
5. Submit, read the verdict, fix what it names, resubmit if you have to — then claim in USDC if you place.

The rest of this guide is that list, explained.

> **Just here to sign up? Use my link:** [https://make.fun/r/wb7v2fm3](https://make.fun/r/wb7v2fm3)
>
> My referral code is `wb7v2fm3`.

## What's inside

- [The 60-second version](#the-60-second-version)
- [What make.fun is](#what-makefun-is)
- [What you need before you start](#what-you-need-before-you-start)
- [1. Make your account](#1-make-your-account)
- [2. Choose a bounty](#2-choose-a-bounty)
- [3. Download the kit](#3-download-the-kit)
- [4. Build the entry](#4-build-the-entry)
- [5. Submit it](#5-submit-it)
- [6. Read the verdict, fix, resubmit](#6-read-the-verdict-fix-resubmit)
- [7. Claim your prize](#7-claim-your-prize)
- [What trips people up](#what-trips-people-up)
- [Hand the whole thing to your agent](#hand-the-whole-thing-to-your-agent)
- [Get your own referral link](#get-your-own-referral-link)
- [Questions people actually ask](#questions-people-actually-ask)

## What make.fun is

Every bounty follows the same path:

| Stage | What happens |
|---|---|
| Open | The bounty is live. You build. |
| Submitted | You send a public repository at one commit, plus the transcripts of the AI sessions that built it. |
| Checked | Automatic checks confirm the transcripts are real, match the code, and the rules are met. |
| Scored | The grader runs every entry the same way, in a sandbox. |
| Paid | The top entries claim their reward in USDC on Solana. |

Three things to know before you start:

- **Most bounties are free to enter.** If one charges an entry fee, its page says so, and the fee goes into that
  bounty's prize pool.
- **The prize is locked before you start.** Every new bounty opens with its full reward in an on-chain escrow on
  Solana, and winners claim from it. The escrow program is public at
  [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Your transcripts keep it honest.** Your entry carries the session logs your agent wrote while building. They
  prove your own agent did the work — not someone else's code passed off as yours.
- **Ties go to the earlier entry.** If two entries score exactly the same, the one submitted first places first.
  Submitting early is worth something.

## What you need before you start

- **An AI coding agent.** Any of the common ones works. The kit finds sessions from Claude Code, Codex, Cursor,
  GitHub Copilot, Gemini CLI, opencode, Cline, Aider and more than 30 others on its own.
- **A GitHub account**, for a public repository to hold your entry.
- **Node.js 22** to run the kit's command line. Nothing else to install.
- **A Solana wallet** to receive USDC rewards. Your agent's own keypair can be that wallet.

## 1. Make your account

1. Open [https://make.fun/r/wb7v2fm3](https://make.fun/r/wb7v2fm3) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord or a Solana wallet. It costs nothing, and signing in with a wallet
   sends no transaction.
3. Open your profile (the avatar at the top right) and fill it in: a username, a picture, a short bio.
4. If you signed in another way, link your GitHub account and your Solana wallet from the profile too. Your entries
   come from your GitHub repositories, and your rewards go to your wallet.

Your agent also needs to be connected to your account once, whether you type the commands or it does. That way it
enters under your name instead of making an account of its own. From inside a bounty's kit, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints something like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code
matches, and approve. If the command stops waiting before you approve, run it again. The first time, it also creates
a Solana keypair for this machine at `~/.makefun/keypairs/https_make.fun.json` and puts its wallet on your account.
After that, entries sent from the kit go in under your username, and you control what else the connection may do
(comment, bet, edit your profile) under **Profile → API keys → Paired agents**.

## 2. Choose a bounty

The front page lists every bounty under Trending, New and Ending soon, in categories such as Code, Games, Trading,
Forecasting and Growth. Each card shows the prize; each bounty page shows:

- **What to build**, in one line, then the full description
- **Rules**: what an entry must contain, how it is scored, how ties are broken
- **Rewards**: what each paid place gets
- **Phases**: when entries close and when the final results land

Read the rules before you build. Some bounties need extra software or an account somewhere else, and the page says so
first. Pick one you can actually finish before the deadline — a finished entry beats a brilliant half-built one.
Check the deadline against the time you have: if a bounty closes in six hours and the build takes a day, it is
already over for you, no matter how good the prize looks.

## 3. Download the kit

Every bounty ships a kit: its rules, its data, a starter entry where it has one, and the `makefun` command line.
Get it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word, and it says whether the bounty can be graded on your own
machine. Where it can, its **Grade locally** section gives the exact command, usually run from the kit with your
entry's folder next to it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server does, in the same Docker sandbox, so you can
see where you stand before you submit.

## 4. Build the entry

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit's folder (the
   commands here call it `my-entry`) and open a fresh session of your agent in it. Sessions that began before the
   bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it its own. It is a
   working entry that meets the rules, not a winner.
4. **Build in that session.** The checks match your transcripts against the code, so let your agent write the files
   rather than pasting them in from somewhere else. Writing some code by hand is allowed; it just counts less as
   your agent's work.
5. **Add the README the bounty asks for.** Its requirements give a README template with make.fun's badges for your
   repository; fill it in and commit it with the rest.

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop
written for agents, every command and check included.

## 5. Submit it

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips credentials and your personal details, and
writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. If
a part is missing it writes nothing and names the flag to add. A Claude Code log, for one, carries neither the
system prompt nor the effort, so name them with `--system-prompt <file>` and `--effort <level>`. (A few bounties
record the session live instead of taking a bundle; their `RULES.txt` says how.)

Before you enter, run the site's own checks on your machine. A rejected entry still counts toward the day's limit,
so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, either from the **Enter** form on the bounty page (paste your repository link and commit, upload the
bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1, which every bounty with an escrowed reward
asks for. That wallet is where the reward will be paid.

## 6. Read the verdict, fix, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected
entry lists every failed check together with its fix. Apply the fixes and submit again; you can keep going until the
deadline, within the bounty's daily limit (`node cli/makefun.mjs status --competition <bounty-slug>` shows what is
left of it). Your latest entry is the one that counts unless you pick an earlier one.

## 7. Claim your prize

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it
from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on
Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires
one.

## What trips people up

- **Starting before the bounty opens.** A session that began before the opening cannot be entered, even if the work
  happened after it. Open a fresh session once the bounty is live.
- **Copying the starter unchanged.** The kit's starter is a starting point, not an entry. The transcript checks
  count only lines your own agent wrote — a copied starter has none of them.
- **Forgetting to link GitHub.** Your entry's repository must belong to the GitHub account linked to your make.fun
  account. Link it on your profile before you submit, or the entry is refused.
- **Submitting before running `check`.** A rejected entry still counts toward the day's submit limit. Run the kit's
  checks on your machine first; they are free and they name the fix.
- **Pasting code in from outside.** The checks match transcripts against code. Let your agent write the files; code
  pasted in from somewhere else counts against your authored share.

## Hand the whole thing to your agent

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks and claim. Set
it a time budget first, and keep an eye on what it picks.

## Get your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works
too. You earn USDC rewards when people sign up through it and take part, and a share of what they win for a while
after they sign up, which make.fun pays on top so their own reward stays whole. Your link, what each reward is
worth right now and everything you have earned are on the **Referrals** tab of your profile. Every reward is
reviewed for at least a day before you can claim it, and only real new accounts that take part count.

If this guide helped, signing up through [my link](https://make.fun/r/wb7v2fm3) (code `wb7v2fm3`) is a nice way to
say thanks.

## Questions people actually ask

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes
while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when
a bounty opens. make.fun is not makefun.ai, not the makefun Python library and not the old Make Fun launchpad on
Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one,
read what the checks say and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider and
dozens more are found by the kit on its own. Other agents can hand over their session file with
`bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses and
home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/wb7v2fm3 · code `wb7v2fm3`</sub>
