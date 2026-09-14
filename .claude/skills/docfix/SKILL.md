---
name: docfix
description: "Work one documentation fix end to end: verify it against the product, edit, update screenshots, run the Mintlify checks, and open a PR with a preview. Run with a Docs Site Tracker item, a page URL, or a description of the problem."
argument-hint: "<tracker item, page URL, or description of the problem>"
disable-model-invocation: true
---

# /docfix: one docs fix, end to end

The problem to fix: $ARGUMENTS

The person running this maintains the docs and may not be a developer. Say what you're doing in plain words, and stop at every step marked **Ask**: those need a human decision. Follow `.claude/CLAUDE.md` throughout.

## 1. Start clean

- Run `git status`. If there are uncommitted changes, stop and ask what to do with them.
- `git switch main`, then `git pull`.
- Create a branch named for the problem, e.g. `docs/fix-undo-button-label`.

## 2. Understand the problem

- Find the page. A docs URL maps to a file: `https://docs.gainable.dev/prompting/refining` is `prompting/refining.mdx`.
- Read it, then search every `.mdx` file and `fin-ai-training-questions.csv` for the same label, phrase or instruction. List every place that needs the same fix.

## 3. Check it against the product

- If Claude in Chrome is connected, open https://build.gainable.dev in a new tab (the user is already signed in) and walk through what the page describes. Read labels exactly as they appear on screen.
- If Chrome isn't connected, or a step needs something you can't reach (a paid feature, a particular app state), give the user a short checklist of exactly what to look at, and wait for the answer.
- Work in a demo or test app. Don't click anything that sends, publishes, deletes, invites or changes settings.

## 4. Classify it (**Ask**)

Tell the user which of these it is, and what you saw:

- **Docs error**: the product is right, the docs are wrong or outdated.
- **Missing docs**: the product does it, the docs don't say so.
- **Unclear docs**: correct, but hard to follow.
- **Product bug**: the product doesn't do what it should.
- **Can't verify**: you couldn't confirm the behavior.

For a product bug or can't-verify, don't edit anything. Write a short note the user can paste into the tracker (what the page says, what actually happens, steps to reproduce, screenshot path if you took one) and stop.

Also say if the fix touches billing, sign-in, security, the API or MCP reference, navigation, or describes a new capability. Those need review from Rickard, the product owner, before merge. Changed steps and new or replaced screenshots need review from Malcolm, the docs owner.

## 5. Plan the edit (**Ask**)

In a few lines: which files change and what the new text says. Wait for an OK before editing.

## 6. Edit

Make the change everywhere step 2 found it. Follow CLAUDE.md for components, navigation, redirects and names.

## 7. Screenshots (**Ask**)

If the page has a screenshot of what changed, or would be clearer with one, say which image and why, and agree with the user who captures it:

- **You capture it** with Claude in Chrome, as described in CLAUDE.md (`zoom` with `save_to_disk`, so it saves as PNG).
- **The user captures it** with Snipping Tool, when it needs an annotation or a state you can't reach.

Save it to `images/<name>.png`, open it to check it matches the text, and embed it with `<Frame>` and real alt text.

## 8. Check

- Run `mint broken-links` and `mint validate`. Fix anything this change broke.
- Start `mint dev` in the background and give the user the local link to the changed page (`http://localhost:3000/<page path>`). If Chrome is connected, open it and look it over too. Stop the server when you're done.

## 9. Commit and open the PR (**Ask**)

- Show `git diff --stat`, summarize the change, and wait for a go-ahead.
- Commit as `docs(<area>): <what changed>`, push the branch, and open a PR with `gh pr create`, filling in every section of `.github/pull_request_template.md`.
- Mintlify comments a preview link on the PR within a few minutes. Check with `gh pr view --comments` and give the user the link.
- Don't merge. The user merges after checking the preview, or after review if step 4 flagged the fix.

## 10. Hand back

Finish with:

- the PR link and the preview link
- what you verified in the product, and how
- what to put in the Docs Site Tracker item (status and PR link)
- anything left open, such as a product bug to report or a reviewer to ask
