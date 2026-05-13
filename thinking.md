## A — Immediate Response (3am)
Hi Rahul — I’m really sorry about the lack of hot water. I can escalate this immediately to the caretaker so it can be fixed right away. For tonight, we’ll also arrange an appropriate resolution/refund option once the issue is confirmed. Could you please share what time you last tried the hot water and whether the problem is in the master bathroom or all bathrooms?

Why this wording: it acknowledges the issue, promises immediate escalation, avoids argumentative tone, and collects the minimum details needed to act.

## B — System Design (beyond sending a message)
1. Receive inbound WhatsApp payload → normalize → classify as `complaint`.
2. Generate AI draft + confidence/action.
3. Because it’s a complaint, trigger an **incident workflow**: notify on-call caretaker and a human guest-relations agent.
4. Log the inbound message and the AI draft (with confidence + query type).
5. Send the immediate response to the guest (auto_send or fast human approval).
6. Start a 30-minute timer. If no human acknowledges:
   - escalate to the next on-call agent / management queue,
   - optionally send an updated message to the guest with the current status.

## C — Learning (repeat hot-water complaint)
Maintain a **property issue tracker** keyed by `property_id + issue type` (hot water). Each complaint increments a severity score and stores evidence (timestamps, AI confidence, human actions/outcomes). After repeated incidents within a time window, the system should trigger preventive actions: maintenance scheduling, downtime checks, and updated SOPs for guests arriving soon. A lightweight post-incident “root cause” field lets the team prevent recurrence and improve future AI routing.

