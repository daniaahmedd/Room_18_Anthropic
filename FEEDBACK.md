# Client review: Larkspur disruption-care agent

**To:** Room_18_Anthropic  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-17 09:06

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

**1. agent.py build_tools(): search_alternatives now describes seats, and its input_schema takes only pnr.**

The new description reads "Search for an alternative seat for the customer and make sure that seat is available... Call this when the customer wants to change their seat." The schema has one property, pnr. It has no flight, date or cabin field. The other tools describe disruption care, and this one describes a seat change. hold_seat is 73 characters and send_confirmation is 71, and both are still the shipped text.

Run python3 run.py K7PQ2M --trace and paste the search_alternatives input and output block.

**2. readout-trace.json calls next_available_day, a tool that does not appear among the 9 schemas in build_tools().**

The committed run lists five calls in order: lookup_booking, get_flight_status, check_policy, next_available_day, search_alternatives. The static scan finds 9 schemas and none is named next_available_day. EXTRA_TOOLS is 0 and LOCAL_TOOLS has no executors. agent.py imports next_available_day from support and mcp_client.tools() is appended in tool_list(). You have not shown where that tool is declared to the model.

Run python3 run.py --show-tools and paste the full list with the source of next_available_day.

**3. PITCH.md Still broken says every booking is dated May 2025 and the agent stops to ask, while your date fix is the one in get_flight_status.**

You changed the get_flight_status date description from "MM/DD/YYYY" to "YYYY-MM-DD". You also say the agent treats test bookings as stale and asks the customer instead of helping. A bigger model does not change a booking date. runtime_preamble() is prepended to the system prompt on every call and you have not shown what it says about today's date.

Run python3 run.py --all --trace and paste the stale-booking turn for the booking that stops to ask.

**4. PITCH.md Number reports 63,357 tokens in and 20 turns across five tickets with no before run to compare against.**

The only committed wire run is readout-trace.json: 5 turns, 20090 in, 1183 out, 12.1s, cache 0 read and 0 written. PITCH.md says K7PQ2M went from an error card to a full answer in 4 API turns, and the trace shows 5. No bench-before or bench-after output is in the material. Lever is still the literal text "<cost | speed | intelligence>". Priya's model question cannot be answered from one build that has no baseline.

Run python3 bench.py --label before --stage 1 --runs 3 and python3 bench.py --label after --stage 1 --runs 3, then paste the python3 bench.py --compare before after output.

**5. No evals/cases.json exists, and the safety claim in PITCH.md rests on five hand-read traces.**

PITCH.md says confirm_rebooking and issue_voucher were never called "across all five tickets' traces". The repo holds no eval cases that would fail if a later change made the agent call them. Your diff also fixed the loop so messages.append carries response.content, where the shipped file passed text_of(response). That is what lets thinking blocks and tool_use blocks survive between turns, and it is why the five-ticket run now answers. The gates banked in readout.html are 1.2, 1.3, 1.4, 2.1 and 2.2, banked by Paul Baverey, and no gates beyond 2.2 appear.

Run python3 eval_harness.py after adding evals/cases.json with one case per booking shape, and paste the pass and fail summary.

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

- `agent.py (230 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `setup.py`, `pod_sync.py`, `run.py`, `review.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/models.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`, `support/mcp_client.py`, `support/mcp_server.py`, `support/mcp_selftest.py`, `support/access.py`, `support/keyscan.py`, `support/review.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
