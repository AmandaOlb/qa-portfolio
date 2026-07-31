# Exploratory & Functional Testing Case Study — Last-Mile Delivery Platform

## Context

As part of a technical assessment for a QA role, I conducted exploratory and functional testing on a live last-mile delivery/logistics web platform. The goal was to identify defects across core workflows — delivery creation and editing, user management, billing-related flows, and settings — within a limited timeframe, simulating the kind of rapid, self-directed testing expected in a real QA role.

This case study documents my approach, findings, and reasoning, organized by severity.

## Methodology

- **Approach:** Exploratory testing combined with structured functional test cases, focused on core user flows (delivery lifecycle, user management, billing-related actions)
- **Environment:** Desktop, Windows 10, Chrome
- **Process:** For each issue found, I documented steps to reproduce, expected vs. actual results, reproducibility rate (number of attempts), and severity/priority justification
- **Prioritization:** Findings were triaged by potential business and financial impact first, then by user experience impact

## Findings

### 🔴 Critical

**Payment bypass via Edit flow**
When changing a delivery from self-managed to platform-managed, the system correctly warns the user that credits will be spent — but only through the primary action button. Using the Edit flow instead to make the same change bypasses this warning entirely, allowing the delivery type to change with no confirmation and no visibility into the financial impact.

- **Reproducibility:** Consistent (3/3 attempts)
- **Why Critical:** This is a business logic vulnerability — a secondary path around the system's own safeguard, with direct financial consequences for the user.

### 🟠 High

**Duplicated stops allowed without warning**
Users could add the exact same stop to a delivery multiple times with no alert — and since every stop is billed, this creates a direct financial impact. When routes were optimized, duplicates were silently consolidated instead of being flagged, removing the user's chance to catch the mistake beforehand.
*Reproducibility: Consistent (5/5 attempts)*

**Required phone number could be deleted post-registration**
Phone number is mandatory at registration, but the same validation wasn't enforced when editing an existing user — allowing the field to be cleared entirely. This breaks downstream functionality dependent on it, including driver contact and delivery notifications.
*Reproducibility: Consistent (3/3 attempts)*

**Invalid cargo size silently applied to self-managed deliveries**
The cargo size field auto-filled with the last used value, including "Extra Small" — an option that isn't valid for self-managed deliveries. The system accepted this invalid state without warning, creating a mismatch that could affect pricing.
*Reproducibility: Consistent (3/3 attempts)*

### 🟡 Medium

**Cargo size edit not applied correctly on first attempt**
Changing a delivery's cargo size from Large/Cargo Van to Extra Small displayed a false confirmation, silently reverting the value to Medium. The correct value only saved on a second edit attempt, creating a misleading confirmation that could result in incorrect billing if unnoticed.
*Reproducibility: Consistent (5/5 attempts)*

**No error feedback for deleted users attempting login**
When an admin removed a user, that user could still attempt to log in — and while the system correctly blocked access, it gave no visible error message, only a silent console error invisible to the end user.
*Reproducibility: Consistent (2/2 attempts)*

### 🟢 Low

**No character limit guidance on a text field**
A settings field lacked any visible character limit, and exceeding it (1500 characters) triggered a vague, generic error message rather than clear guidance.

**Inconsistent minimum search characters**
Customer search required a different minimum number of characters depending on the search term, with no consistent or documented threshold — a minor but confusing UX inconsistency.

**UX observation: pre-filled location field lacked indication**
The pickup location field auto-filled with the last used address with no visual cue that it was pre-filled, creating risk of users unknowingly submitting an incorrect location.

## Reflection

Testing a live, production-grade platform under time constraints reinforced a few things I look for now as a matter of habit:

- **Business logic gaps hide in secondary paths.** The Critical payment bypass wasn't in the "main" flow — it was in an alternate route (Edit) that simply hadn't inherited the same validation. This is a reminder to always test the same action through every entry point, not just the obvious one.
- **Silent failures are often worse than visible ones.** Several of the High/Medium findings involved the system doing the *wrong* thing quietly — consolidating duplicates, reverting a value, blocking access without explanation — rather than failing loudly. Silent failures erode trust because users have no way to know something went wrong.
- **Severity should track financial and business impact first.** Several findings here were prioritized by their billing/financial implications rather than surface-level UX friction, which I think is the right lens for a platform handling real transactions.

---
*Company name intentionally omitted; this platform was tested as part of a job application process, with no confidentiality restrictions communicated at the time.*
