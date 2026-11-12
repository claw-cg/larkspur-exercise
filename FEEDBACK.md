# Client review: Larkspur disruption-care agent

**To:** Room 8  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-12 09:44

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. agent.py EXTRA_TOOLS: next_available_day is declared in the diff, but PITCH.md names get_entitlements and no tool by that name exists in the scan.**

The static scan finds 11 tool schemas: the 9 shipped plus fare_rules and next_available_day. LOCAL_TOOLS maps exactly those two. PITCH.md Built says "We also have get_entitlements", and no schema, executor or function with that name appears in the scan or the diff.

Run python3 run.py --show-tools and paste the full list, with the line for get_entitlements if it exists.

**2. PITCH.md Number: 'tokens in increased from 53,621 to 82,468' has no bench file or compare output behind it, and the supposition is untested.**

The only committed wire run is readout-trace.json: 15299 in, 1079 out, 4 API turns, 3 tool calls. Neither 53,621 nor 82,468 appears in it. The pitch attributes the rise to adding tools, and fare_rules (426 characters) plus next_available_day (151) are about 600 characters of schema, small next to a 60% jump. The run.py loop change in this diff, passing response.content back, also changes input tokens every turn.

Run python3 bench.py --compare on your before and after labels and paste the output.

**3. agent.py hold_seat (73 characters) and send_confirmation (71 characters) are the shortest descriptions in build_tools(), and neither was touched.**

hold_seat reads "Place a 15-minute hold on one alternative. Reversible. It simply expires." It says nothing about calling search_alternatives first or where option_id comes from. Your diff rewrote search_alternatives, which now names hold_seat and check_policy as the next calls, so that handoff is described from one side only. Better descriptions on the model-facing side of that handoff are a change a bigger model would not make for you.

Run python3 run.py --show-tools and paste the hold_seat and send_confirmation entries, with the sequence you expect between search_alternatives and hold_seat.

**4. agent.py run_agent: the answer now comes from the last response, but the loop exits at turns < MAX_TOOL_CALLS (8) with no handling of a still-pending tool_use.**

Your diff moved answer = text_of(response) after the create() call and appends response.content to messages. If turn 8 is reached while stop_reason is still tool_use, text_of returns the last text block, which may be empty. The committed run used 4 API turns, so the cap has not been exercised in anything you committed.

Run python3 run.py --all --trace and paste the totals footer, noting any PNR that reaches 8 turns.

**5. The repository has no evals/cases.json, so the Safety check line in PITCH.md rests on one trace read for PNR M3XR8T.**

PITCH.md Safety check says the output shows it "won't finalise the change without the client pressing confirm". Only gates 1.2, 1.3, 1.4 and 2.1 are banked in readout.html, with no cases file behind them. confirm_rebooking carries a 216-character description about confirmation_token, and its refusal is enforced by the tool, so a model name changes nothing about it. Nothing committed shows any other booking shape tried against it.

Run python3 verify.py 4.1 and paste the result, then paste the python3 run.py --all --trace totals footer.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (290 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
