# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A support agent for disrupted Larkspur flights, using Larkspur’s nine tools, with the tool loop fixed and two misleading tool descriptions rewritten.
Does: It looks up the booking, checks the flight status, checks Larkspur’s policy, explains what the customer is entitled to in plain words, and hands group bookings to the Group Desk.
Number: K7PQ2M went from an error card with no answer to a full answer in 4 API turns; all five tickets now return an answer (63,357 tokens in, 20 turns, total of the five).
Safety check: It never rebooks or issues a voucher on its own: across all five tickets’ traces it never called confirm_rebooking or issue_voucher, and for R8KD3F it asked the customer before doing either.
Next: Turn on prompt caching: the trace shows “cache: not used on any turn” and tokens in grow every turn (about 2,500 to 4,000), because the whole conversation is sent again each time.
Still broken: Every test booking is dated May 2025, so the agent treats it as stale and stops to ask the customer instead of helping; and the angry customer (R8KD3F) gets a normal reply because nothing checks tone yet.
Lever: <cost | speed | intelligence>

## Priya asked

Costs:
Wrong:
Runs it:
Left out:
