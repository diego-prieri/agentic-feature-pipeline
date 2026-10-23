# Ticket assignment

**Context:** a small internal ticket tracker. Today every ticket is unassigned; team leads coordinate in chat.

**Goal:** let a team lead assign a ticket to one team member, and let anyone filter the list by assignee.

## Requirements

1. On the ticket detail page, a lead can pick an assignee from the team's members, or clear the assignment.
2. The ticket list shows the assignee's name in a new column; unassigned tickets show "—".
3. The list can be filtered by assignee, including "Unassigned".
4. Non-leads see the assignee but cannot change it.
5. Assigning a ticket records who did it and when, shown in the ticket's activity.

## Out of scope

- Notifications to the assignee.
- Assigning several people to one ticket.
