# Workflow diagrams
## Contents
- Start and qualification
- Decisions and execution
- Legend
## Start and qualification
```mermaid
flowchart TD
 P["🧠 Evidra · Load profile"] --> C["📊 Sheets · Create campaign"]
 C --> A{"👤 Campaign authorized?"}
 A -- No --> H["⏸ Hold"]
 A -- Yes --> R["⚙️ Web tools · Research / enrich"]
 R --> S["⚙️ Evidra · Verify evidence / classify structure"]
 S --> O["⚙️ Evidra · Qualification score + every-offer fit score"]
 O --> CR{"Commercial route?"}
 CR -- Local / regional --> M{"Review policy"}
 CR -- Large commercial --> ET["⚙️ Log parent + procurement / personnel routes"]
 ET --> M
 M -- Manual --> U["👤 Sheets · Approve / Deny / Move"]
 M -- Automatic --> Q{"⚙️ Qualification gates"}
 Q -- Pass --> AP["✅ Approved by policy"]
 Q -- Unknown --> E["⚠️ Exception queue"]
 Q -- Below threshold --> U
```
## Decisions and execution
```mermaid
flowchart TD
 T["⚙️ Resync command or verified dispatcher"] --> D{"📊 Latest decision"}
 D -- Approve --> C["⚙️ CRM · Companies + contacts + associations"]
 D -- Deny --> X["📊 Preserve research and reason<br/>cancel queued work for this campaign"]
 X --> C
 D -- Move --> M["📊 Destination campaign; carry evidence"]
 M --> R["⚙️ Refresh gaps; rescore under destination policy"]
 R --> D
 C --> N["⚙️ Normalize every contact<br/>one person per record<br/>person links stay attached"]
 N --> V{"⚙️ CRM readback correct?"}
 V -- No --> E["⚠️ Hold / reconcile"]
 V -- Yes --> A{"⚙️ Approved for this campaign?"}
 A -- No · denied --> HU["📊 Held in Evidra · unassigned<br/>until a campaign claims the contact"]
 A -- Yes --> W["⚙️ Evidra · Offer-specific copy"]
 W --> G["⚙️ Gmail · Addressed unsent draft"]
 G --> K{"⚙️ Recipient/body/status verified?"}
 K -- No --> E
 K -- Yes --> F["📊 Draft Queue · Ready for user review"]
 F --> U["👤 User reviews in mailbox · Skill stops"]
```
## Legend
⚙️ Automatic inside authorized scope; 👤 user decision; ⚠️ exception; 📊 campaign store; 🧠 skill.
A scheduled dispatcher is optional and must be actually configured. Manual resync works without one.
Deny applies to the campaign only: no contact information is removed. Denied contacts are still written to the CRM and held in Evidra, unassigned, until a campaign claims them; they receive no outreach.
Neither diagram includes sending. Mailbox labels are capability-gated; the review queue is the default.

