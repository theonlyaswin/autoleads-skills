# Follow-up Playbook

People don't reply because they're busy, not because they read and rejected. A good follow-up is short, easy to answer, and gives them something new. A bad one only reminds them that they owe you a reply.

## Cadence

Days are counted from the previous touch (original message or last follow-up).

| Situation | Follow-up 1 | Follow-up 2 | Follow-up 3 (close the loop) |
|---|---|---|---|
| Lead DM, no reply | +3 days | +5 days | +7 days |
| After a call, no next step | +2 days (recap + next step) | +5 days | +7 days |
| Proposal, no answer | +3 days | +5 days | +7 days (before it expires, if `valid_until` is set) |
| Past client | 60–90 days after the project ended, then every ~6 months | | |

Hot leads on X or Reddit move fast: if the post is under 48 hours old, follow-up 1 can be +1–2 days.

If the post said the role is filled, the thread shows a "no", or it's been more than 30 days since the post with no reply, stop and suggest marking it `lost`.

## What to add (pick one per message, never repeat)

1. **A quick idea** for their project, specific to their post. ("One thought on your bundle builder: tiered discounts usually beat fixed bundles for first orders.")
2. **A relevant example**: a proof point or case study that matches their need, with a link if the channel allows it.
3. **An easy question** they can answer in five words. ("Is this still on your list for this month?")
4. **A useful resource**: a short checklist, a template, a teardown of their current site. Only things the user can actually provide.
5. **A real constraint**: the user's availability changing, or the proposal's validity date. Only if true.

## Templates

**Lead DM: follow-up 1 (new idea)**
> Hi Jane, one idea for the subscriptions setup: letting customers skip a month from their account cuts cancellations a lot. Happy to show how I'd set that up on your store.

**Lead DM: follow-up 2 (example)**
> Here's a similar build I did for a coffee brand, subscriptions plus a bundle builder: <link>. Worth a quick chat this week?

**Lead DM: follow-up 3 (close the loop)**
> I'll stop crowding your inbox. If the store project comes back around, I'd be glad to help. Good luck with the launch!

**After a call: recap**
> Thanks for the chat today. Recap: you want subscriptions live before 20 Nov, Recharge is fine, and Sam signs off. I'll send options by Thursday. Anything I missed?

**Proposal: follow-up 1**
> Hi Sam, did you get a chance to look at the proposal? Happy to walk you through the options on a 15-minute call, or adjust the scope if the timing or budget needs to shift.

**Proposal: follow-up 2 (easy question)**
> Quick one: is the Recommended option closest to what you need, or should I put together a smaller first phase?

**Proposal: follow-up 3 (close the loop)**
> The proposal is valid until Friday, so I'll close the file then. If priorities change later, just reply here and I'll pick it back up.

**Past client check-in**
> Hi Mia, it's been a few months since the café site went live. How's it working for you? I've been doing a lot of online ordering setups lately, if that's ever on your list.

## Channel notes

- **LinkedIn**: reply in the same DM thread. 25–60 words. A link is fine from follow-up 2.
- **X**: same DM thread; under 200 characters; casual. If the original was a public reply, follow up by DM only if `can_dm` is true, otherwise skip.
- **Reddit**: same chat; no links unless they asked; 25–50 words.
- **Email**: keep the original subject with "Re:", 40–80 words.

## Never

- "Just following up", "bumping this", "circling back", "did you see my message?", a lone "?".
- Fake urgency, invented discounts, or "last chance" when it isn't.
- Following up after a clear no, after they replied, or more than 3 times.
