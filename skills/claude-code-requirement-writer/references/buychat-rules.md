# BuyChat project rules (as established in the user conversation)

Use this reference **only for BuyChat-related prompts**. These are project-level context and reported requirements, **not independently verified descriptions of deployed functionality**. Tell Claude Code to inspect current code before assuming any module exists. Newer user instructions override older rules here.

## Product and architecture

- BuyChat: multi-tenant WhatsApp AI conversational commerce SaaS. Company business type may be Products, Services, or Both; company goals may be sales, bookings, admissions, leads, consultations, etc.
- Roles: Platform Admin (superadmin), Company Admin, company Staff, customer. Tenant isolation applies to every record, payment account, attachment, AI instruction, notification, and conversation.
- Companies can connect WhatsApp numbers through the relevant approved Meta integration; provider onboarding is a separate concern. Hosting referenced: Railway and PostgreSQL; never infer the *current* application framework without inspecting it.
- Each company has a unique assistant name and configurable system instructions, offerings, services, prices, policy, aim, and subscription plan. Assistant should introduce itself as `[Name] from [Company]`, sound natural, and not pretend to be a real human if explicitly asked.
- Platform Admin configures AI/STT/TTS API credentials and models. Plans control entitlements, including text/voice/both outputs, trials, and company-supplied keys. Legacy Access has been used in previous QA rounds; select the existing valid plan requested for the new round.

## AI conversation quality

- Follow customer's actual language: English → English; Malayalam **script** → Malayalam script; Manglish (Malayalam in Latin letters) → natural Manglish; Arabic → Arabic script, RTL; Hindi → Devanagari. Explicit language switching should be respected. Short ambiguous turns inherit established context.
- **For Malayalam and Manglish only:** AI-generated structured **headings and subheadings must be in English**, while explanatory body remains in Malayalam script or Manglish respectively. All standard WhatsApp **interactive action button labels** must be in English. Keep stable action IDs and business/product names. Do **not** apply this exception to Hindi or Arabic.
- Be emotionally appropriate; never celebrate or sound happy about the customer's losses, problems, declining sales, or complaints. Recognize intent and sentiment from context and respond with proportionate empathy.
- Conversation-first sales: do not dump all offerings/prices in the first response when a user merely describes a problem. Ask a useful question, understand needs, then recommend. **If customer asks directly for products, services, catalogue, prices or summaries, answer directly**; do not withhold requested information.
- Company-specific catalogues/knowledge constrain factual claims; never fabricate prices, stock, policy, discount, technical specs, appointment availability, guarantees, or confirmations. Record genuinely unanswered questions silently as AI Knowledge Gaps; do not tell customer about the internal record.
- Customer-facing AI reply counts: count an actual AI message/voice response as one, independent of token count or text length.

## Text and voice replies

- Pipeline: WhatsApp voice input → configured STT → main conversational AI → conditional TTS or text output; Sarvam was selected for TTS in earlier discussions, subject to actual current provider configuration.
- Voice input normally receives voice for natural conversation **when plan allows**; use **text / interactive / media** for structured information such as full catalogues, price lists, service lists, product comparisons, order/booking summaries, quotation breakdowns, and UPI/payment instructions. Never TTS a long catalogue by default. User explicit preference and plan entitlements matter.
- Minimize unnecessary AI/STT/TTS calls. Voice/transcripts and voice delivery failures must be handled accurately.

## Human attention, handover and control (latest decisions)

- **Need Human** is an attention badge and issue lifecycle, not automatic handover shutdown. Track reasons independently, display one badge and unresolved count, allow assignment, in-review, resolved, reopened, preserve audit. The badge clears only after all underlying reasons resolve; do not auto-resolve merely because AI responds or customer goes silent.
- **Need Attention** is separate from Need Human and Payment Verification Required. It does not stop AI.
- Under normal authorized conversation control, AI **stops only** when (1) staff manually switch AI OFF, or (2) staff explicitly click **Take Over**. A staff manual reply without takeover does not stop AI. Releasing takeover resumes AI only if the manual toggle is ON. Subscription/compliance/platform failures remain separate restrictions.
- Suppress pending/queued replies and follow-ups when AI OFF or Take Over takes effect. Resolve badges without changing AI state. Prevent staff/AI conflicting replies where possible.
- Explicit customer request for human assistance applies Need Human and can trigger independent admin WhatsApp notification, without stopping AI.

## Orders, bookings, prices and payments

- Confirmed product orders and service bookings appear as linked real backend records in the Conversations sidebar. Services companies show Bookings; product companies show Cart/Orders; hybrid show both as relevant. The AI saying "confirmed" is not sufficient evidence.
- Pricing models can be fixed, quantity/weight/area/time based, starting-from, range, inspection, quotation, hourly, monthly, and post-service. Unknown price ≠ zero; estimates ≠ authorized payable amount. Record customer and required staff approvals before payment.
- COD may be available; Manual UPI: Platform Admin enables per company, Company Admin may configure multiple QR/UPI records, **only one active**. Send correct amount, active UPI ID, QR/media, record evidence, and flag **Payment Verification Required**. A screenshot or UTR alone **never proves bank receipt**. Authorized staff verify receipt independently before Verified/Paid state and any consequential confirmation.
- Admin WhatsApp payment verification notification enhancement: notify enabled authorized recipient numbers on new payment evidence; interactive **Payment Received** / **Not Received** actions should invoke existing manual verification approval/rejection logic with signed/opaque action context, sender authorization, stale-action checks, deduplication, audit and concurrency protection. Customer acceptance/rejection response should reuse configured existing messages. Never assume an interactive click proves funds received without the admin's independent check.
- Keep Pending Verification, Verified, Rejected, Booking Confirmed, and Service Completed separate. No unauthorized automatic confirmation, duplicated financial side effects, or tenant leakage.

## Admin notifications and platform oversight

- Platform Admin controls whether **Admin WhatsApp Notifications** is enabled per company and the maximum count of registered notification numbers; Company Admin manages verified, consented active recipients, event preferences and history.
- Events: Product Order Confirmed, Service Booking Confirmed, Service Request Confirmed, Need Attention Flag Applied; later additions Payment Verification Required and Human Assistance Requested. Use verified backend events, not AI text; send through WhatsApp within its applicable window or approved template; deduplicate and log delivery. The recipient's 24-hour window is distinct from the customer's.
- Platform Admin can **view each company's conversations strictly read-only** (messages, media, orders/bookings, statuses, summaries). No send, edit, takeover, AI toggle, payment approval, flag resolution or other mutation. Even viewing must not change unread/read receipts, trigger AI, or alter business state. Enforce at API level and audit access.
- Company subscription and feature controls remain enforced independently of notifications and AI mode.

## Scheduled follow-ups and QA practice

- Follow-ups have had duplicate/race issues. Require at most one active follow-up per company/contact, proper scheduling, cancellation/reset after replies, no inappropriate sales reminders after booking/payment review, suppression during AI OFF / Take Over / opt-out, idempotent jobs.
- User repeatedly requests real multi-company AI conversation QA across five languages; persist actual chats and linked business records in **development Conversations** and Platform Admin viewer so they can inspect them. Use distinct test users and never send live WhatsApp messages or money during automated QA. Preserve evidence.
- For implementation, review actual architecture, propose plan, implement, test and regress as requested. For **test-only** phases: do not alter source code, automatically fix defects, delete production records, or falsely claim any test ran. **Do not commit or push unless the user explicitly instructs it.**
