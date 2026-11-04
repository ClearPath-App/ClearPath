# ClearPath

An iPhone application under development for individual and shared financial management in Costa Rica.

**Owner:** Natalia Martínez Villegas  
**Context:** TPA-5001 Proyecto Empresarial, Software Application Development technical program.  
**Status:** Incremental implementation. This document defines planned requirements, not completed functionality (see [Implementation status](#implementation-status)).

## Authoritative project context

This README is the repository's self-contained summary of the project scope and its functional authority. An implementation agent may not have access to the anteproyecto: use the requirements and boundaries below without inventing missing decisions. AGENTS.md defines working instructions and technical boundaries; design.md defines visual and interaction guidance. Neither may expand the scope defined here.

**Scope source and approval.** An earlier version of the project proposal was approved; it included offline shared synchronization, expense allocation, notifications, a calendar, and debt simulations as committed work. This README now follows the **updated anteproyecto (2026)**, which replaces that scope with four modules in individual and shared profiles. This repository holds no record that the updated anteproyecto has received academic approval; do not describe it as approved until that is confirmed. Any later change to the committed core is reviewed with the academic advisor.

ClearPath serves individuals, couples, families, and groups who record financial information manually. It brings **budget, savings, manual investment tracking, and basic debt tracking** together, in both individual and shared financial profiles. It is an independent entrepreneurship project, not software commissioned by an existing business. It does not execute payments, transfers, or real investments, and it does not provide financial advice.

The first release targets **iOS/iPhone only**, including every member of a shared profile.

## Languages and development environment

- Code, identifiers, technical comments, README, and technical documentation: English.
- Application interface, validation messages, and participant user guide: Spanish for Costa Rica.
- Development computer: Lenovo running Windows, with VS Code and PowerShell. There is no assumed access to a Mac, Xcode, an iPhone simulator, or an Apple Developer Program membership.
- Planned on-device execution: **Expo Go on real iPhones**. Web preview helps review the interface but does not replace iPhone testing.

## Committed first-version requirements

"Financial profile" (perfil financiero) distinguishes the individual context from a shared one; it does not mean a separate account per module. All four modules are available in both profile types. The interface shows one profile at a time.

### 1. Profiles, access, and members

- Each person has a private **individual profile** that works locally and does not require an account or internet.
- A person can create or join **shared profiles** with two or more members. Authentication is required for shared profiles, and creating, viewing, and modifying shared information requires internet.
- The creator of a shared profile is its initial **administrator**. The administrator invites, adds, and removes members. People join with an **invitation code** entered while signed in; the administrator can invalidate the code.
- Removing a member revokes their access but preserves their records and authorship.
- Two roles only: administrator and member. No custom permissions and no automatic administration transfer.

### 2. Budget

- Create, view, edit, and delete limits by **period, category, and currency**. A period uses start and end dates, which can represent a monthly or twice-monthly (quincenal) period without depending on income records. Periods cannot overlap for the same category and currency within a profile.
- Deleting a budget preserves its expenses.
- Create, view, edit, and delete expenses with **date, amount, currency, and category**.
- Show the used and available amount for the period. When a limit is exceeded, show the exceeded amount. No automatic notification is committed.
- Initial categories: alimentación, vivienda y servicios, transporte, salud, educación, entretenimiento, otros.

### 3. Savings

- Create, view, edit, and delete goals with name, currency, and target amount.
- Record contributions and withdrawals; view balance and progress.
- A withdrawal is a manual record, not a money transfer, and cannot exceed the recorded balance.

### 4. Investments (manual tracking)

- Create, view, edit, and delete investment records with name, currency, and initial value.
- Record contributions, withdrawals, and manual value updates; view history and recorded value.
- A withdrawal must be positive and cannot exceed the recorded value.
- No market quotes, no external data, no promised returns.

### 5. Debts (basic tracking)

- Create, view, edit, and delete debts with name, currency, initial balance, installment amount, and due date.
- Record payments (abonos) and view the outstanding balance and payment history. A payment cannot exceed the outstanding balance.
- Record manual **balance adjustments** when the real debt changes (for example, new charges). An adjustment has date, reason, author, and amount of change, and is not a payment.
- **Not committed:** automatic interest, late charges, amortization tables, projected payoff date, and extra-payment simulation.

### 6. Record integrity

- Movements have an identifier, date, currency, amount, and author where applicable.
- Editing or deleting a contribution, withdrawal, or payment recalculates the affected balances.
- Deleting requires confirmation. A savings goal, investment, or debt with movements is **archived** instead of deleted, to preserve history; an empty one can be deleted. Archived records remain viewable and accept no new movements.
- A shared record is shown as saved only after the service confirms the operation.
- Data validation and permissions are also enforced in storage, not only in the interface.

## Roles and permissions in shared profiles

| Action in a shared profile | Administrator | Member |
| --- | --- | --- |
| View the four modules and their movements | Yes | Yes |
| Invite, add, or remove members | Yes | No |
| Create, edit, or delete budgets | Yes | No |
| Create, edit, archive, or delete savings goals | Yes | No |
| Create, edit, archive, or delete investments | Yes | No |
| Update an investment's manual value | Yes | No |
| Create, edit, archive, or delete debts and record balance adjustments | Yes | No |
| Create or invalidate the invitation code | Yes | No |
| Record expenses and assign their initial category | Yes | Yes |
| Record savings or investment contributions and withdrawals | Yes | Yes |
| Record debt payments | Yes | Yes |
| Edit or delete own movements | Yes | Yes |
| Edit or delete another member's movements | Yes | No |
| Change authorship or raise own role | No | No |

The individual profile is accessible only by its owner, who manages all four modules. Author identity is the authenticated account and cannot be replaced arbitrarily.

## Calculation rules

All calculations are manual tracking of entered data. Validate them against independent cases: zero movements, multiple movements, corrections, and limits.

- **Budget:** used = sum of confirmed expenses for the category, period, and currency; available = limit − used. A negative result means the limit was exceeded.
- **Savings:** balance = contributions − withdrawals; progress = balance / target × 100, with a positive target.
- **Investments:** keep initial value, contributions, withdrawals, and value updates in time order. The recorded value starts from the latest manual valuation and is adjusted by later contributions and withdrawals; a new valuation replaces the previous one as the reference. This is not realized return or a market quote.
- **Debts:** outstanding balance = recorded initial balance + balance adjustments − valid payments. Installment and due date are informational. Adjustments are separate records, never counted as payments.

## Currency and financial integrity

Amounts are recorded in CRC or USD and viewed per currency. No automatic conversion, no cross-currency totals, no additional currencies.

CRC and USD both use **two decimal places**. Store amounts with an explicit currency and a fixed-precision representation, validated before saving. Tests verify rounding and totals.

## Technical architecture

- React Native, Expo, TypeScript, and the existing Expo Router navigation.
- expo-sqlite for the individual profile.
- Firebase Authentication and Cloud Firestore through the **Firebase JavaScript SDK** for shared profiles.
- No custom Swift code and no custom native modules.

### Storage boundaries

The selected profile routes each operation: SQLite for individual information; Firestore, online, for shared information. Firebase Authentication identifies members; Firestore rules verify membership, role, and authorship.

Individual financial data stays in SQLite on the device and **is never synchronized with or copied to Firebase**. SQLite remains the relational database for the individual profile and for the academic database evidence.

Main entities: profiles, members, categories, budgets, expenses, savings goals, savings movements, investments, investment movements, debts, and debt payments. Keep these concepts coherent across SQLite and Firestore.

### Connectivity

- The individual profile works locally without internet.
- Shared profiles check connectivity and report operations in progress, errors, and save confirmation.
- Shared offline operation, Firestore offline persistence, and a pending-sync queue are **not committed**.
- Retries use stable identifiers or equivalent controls so movements are not duplicated.
- Operations that update a movement and its balance stay consistent through transactions or by deriving balances from confirmed records.
- Test interruptions during an operation, failed responses, retries, and changes from two accounts.

### Execution and distribution

Validation runs on real iPhones through Expo Go with a version compatible with the project's Expo SDK. Before recruiting participants, verify on an iPhone: app start, one SQLite operation, authentication, and one shared read and write with two accounts. For the extended test, also verify access from an iPhone other than the owner's and the availability of the Expo server or link; independent installation is not promised.

App Store publication, a broad TestFlight pilot, installation from Xcode, and an Apple Developer membership are not part of the academic delivery. Firebase starts within its free-plan limits; any additional service or paid plan is evaluated for necessity and cost before use.

## Security and database evidence

- Translate the permission matrix into Firestore rules and verify them with two members of different roles and one unrelated account.
- A member cannot make themselves administrator. A removed member's access is rejected.
- Store only necessary shared information. Use Firebase's transport, authentication, and access protections; no custom end-to-end encryption.
- Use fictional data in tests. Never commit credentials or secrets.
- Document SQLite's relational model, physical design, relationships, constraints, queries, and transactions. Verify valid amounts, references between records, and persistence after closing and reopening the app.
- Perform and document a technical SQLite backup-and-restore exercise with fictional data. This is academic evidence, not a user-facing backup feature.
- Document Firestore collections, documents, logical relationships, required indexes, and access rules.

## Requirement catalog

Committed verifiable actions. Use these IDs in tasks, tests, and results. "Owner" is the individual-profile user; "member" includes administrator and member roles.

| ID | Module | Verifiable action | Who | Profile |
| --- | --- | --- | --- | --- |
| RF01 | Profiles | Open the private individual profile | Owner | Individual |
| RF02 | Access | Create an account, sign in, and sign out for shared profiles | User | Shared |
| RF03 | Profiles | Create a shared profile and assign its administrator | User | Shared |
| RF04 | Members | Invite and add a person to the profile | Administrator and invitee | Shared |
| RF05 | Members | Remove a member | Administrator | Shared |
| RF06 | Profiles | Switch profile and view only its information | Owner or member | Both |
| RF07 | Budget | Create a limit by category, period, and currency | Owner or administrator | Both |
| RF08 | Budget | View, edit, or delete a budget | By action and role | Both |
| RF09 | Expenses | Record an expense with date, amount, currency, and category | Owner or member | Both |
| RF10 | Expenses | View expenses; edit or delete by authorship | By role and authorship | Both |
| RF11 | Budget | View used, available, or exceeded amount | Owner or member | Both |
| RF12 | Savings | Create a goal with target amount and currency | Owner or administrator | Both |
| RF13 | Savings | View, edit, or delete a goal | By action and role | Both |
| RF14 | Savings | Record a contribution | Owner or member | Both |
| RF15 | Savings | Record a withdrawal within the available balance | Owner or member | Both |
| RF16 | Savings | View balance, movements, and progress | Owner or member | Both |
| RF17 | Investments | Create a record with currency and initial value | Owner or administrator | Both |
| RF18 | Investments | View, edit, or delete an investment | By action and role | Both |
| RF19 | Investments | Record a contribution | Owner or member | Both |
| RF20 | Investments | Record a valid withdrawal | Owner or member | Both |
| RF21 | Investments | Manually update an investment's value | Owner or administrator | Both |
| RF22 | Investments | View history and recorded value | Owner or member | Both |
| RF23 | Debts | Create a debt with balance, currency, installment, and due date | Owner or administrator | Both |
| RF24 | Debts | View, edit, or delete a debt | By action and role | Both |
| RF25 | Debts | Record a payment not exceeding the outstanding balance | Owner or member | Both |
| RF26 | Debts | View outstanding balance and payment history | Owner or member | Both |
| RF27 | Records | Edit or delete a permitted contribution, withdrawal, or payment and recalculate affected balances | By role and authorship | Both |
| RF28 | Connectivity | Save a shared operation and view it, confirmed, from another authorized account | Member | Shared |

Cross-cutting criteria: valid data, separate currencies, individual privacy, rejection of unrelated accounts, verified authorship and role, individual persistence after restart, balance integrity, and no duplicates from retries. Grouped CRUD actions are tested operation by operation, including effects on dependent records. Per the owner-confirmed decisions below, "delete" in RF13, RF18, and RF24 means archive when the record has movements, and RF04 uses an invitation code.

## Owner-confirmed decisions

The anteproyecto left these details open. Natalia, as project owner, confirmed them on 2026-11-03. They refine the updated scope; they are not a record of academic approval.

- **Invitations:** a signed-in user joins a shared profile by entering an invitation code. The administrator can invalidate the code.
- **Member removal:** revokes access; the member's records and authorship are preserved.
- **Budget deletion:** preserves the budget's expenses.
- **Archiving:** savings goals, investments, and debts with movements are archived to preserve history; empty ones can be deleted. Archived records remain viewable and accept no new movements.
- **Precision:** CRC and USD both use two decimal places and remain separate.
- **Budget periods:** cannot overlap for the same category and currency within the same profile.
- **Investment withdrawals:** must be positive and cannot exceed the recorded value.
- **Debt balance adjustments:** separate records with date, reason, author, and amount of change; they are not payments.
- **Navigation:** five tabs (Inicio, Presupuesto, Ahorros, Inversiones, Deudas) with an individual/shared profile selector; see design.md.

Still open (ask before implementing): invitation code format, expiry, or reuse beyond invalidation; and whether the administrator can leave or be replaced, since automatic administration transfer is excluded.

## Desirable features: conditional, not committed

Consider only after the committed core is integrated and verified, with enough time remaining and Natalia's authorization. They have no mandatory development time; if not completed and tested, they move to a later stage. New desirable work is frozen from week 15 to protect validation.

- **Extended budget and movements:** income records, pay-date configuration, custom categories and subcategories, payment method, shared-expense splitting (equal parts, percentages, or amounts), recurring expenses, and reserves for non-monthly expenses.
- **Views and alerts:** calendar, additional summaries and charts, due-date reminders, extended visual alerts, and local notifications. Basic balance, history, and progress views for the four modules remain committed.
- **Advanced planning:** emergency fund with specific rules, differentiated investment gains and performance analysis, debt interest and amortization, and extra-payment simulation. If simulation or amortization is authorized, define and test a single scheme first (for example, fixed installment with interest on balances and a documented nominal-annual-to-monthly rate conversion).
- **Shared offline operation:** offline recording, pending states, and later synchronization. Adding it requires reviewing technical compatibility with the JavaScript SDK and resolving conflicts and duplicates.

## Future work and exclusions

Outside the academic delivery:

- Apple Wallet, Shortcuts, and Apple Pay purchase automation (no mandatory early experiment).
- App Store publication and a broad TestFlight pilot.
- Android, additional languages and currencies, and home-screen widgets.
- Bank connections, payments from the app, automatic investment data, and reimbursement settlement between members.
- Spreadsheet import/export, Excel or Google Sheets sync, and full cloud backup. Online shared profiles are not a backup of individual data.
- Billing and premium-feature enforcement. A freemium model is a business hypothesis; the free/paid split is not decided, and the committed/desirable classification does not determine it.

## Delivery plan

The project spans 20 weeks and 400 hours: 50 for the anteproyecto (weeks 1–3) and 350 for execution (weeks 4–20). Build the real application incrementally, without a separate prototype. Progress is measured by verified functions, not screens created.

Implementation order:

1. Weeks 4–5: Windows and repository setup; architecture, navigation, and data models; early Expo Go on iPhone and SQLite check; actions and permissions; individual profile navigation.
2. Week 6: individual budget and expense CRUD.
3. Week 7: early Firebase JS test (authentication, two accounts, rules); create and join shared profiles; member management.
4. Weeks 8–9: basic online shared budget; stabilize permissions, connection errors, retries, and unrelated-account checks. Partial technical demonstration to the academic advisor in week 8.
5. Week 10: integration fixes; intermediate validation and first report milestone.
6. Weeks 11–12: savings, individual then shared.
7. Week 13: manual investment tracking in both profiles.
8. Week 14: debts and payments in both profiles.
9. Week 15: integrate the four modules; second milestone; freeze desirable additions.
10. Weeks 16–17: integration, rules, two-account, connectivity, calculation, and persistence tests; SQLite backup/restore exercise; access rehearsal on another iPhone; optional extended test starts.
11. Week 18: combined demonstration and guided test.
12. Week 19: priority corrections, evidence, final report, documentation, and Spanish user guide.
13. Week 20: defense preparation; minor fixes only.

Unit tests for rules, persistence checks, and documentation accompany each module. A delay does not automatically turn a committed function into a desirable one.

## User validation

Evaluate the functional application with volunteers, fictional data, and the same script for everyone. This replaces the earlier plan of 15 participants.

**Combined demonstration and guided test (week 18):** 4–6 people from the target segment, aiming for at least 5, including a couple or group that shares expenses when possible. A short demonstration is followed by individual tasks and a guided conversation about usefulness, clarity, and what is missing or unnecessary. Script: select the individual profile; create a budget and record and correct an expense; create a goal and record a contribution and a withdrawal; record an investment and view its history; record a debt and a payment; then do shared tasks for the participant's role and view a change made from another account. Prepare amounts and expected results for each action. Two iPhones with different accounts are needed to observe shared behavior.

**Extended test (weeks 17–18, conditional):** 2–3 people close to the owner with iPhones, preferably from one household or group, use the available features for 5–7 days through Expo Go. It runs only if access from another iPhone and shared use with two accounts are verified first and participants are available; otherwise document the limitation and keep the guided test. Participants may overlap with the guided test; report results by activity and do not count them twice as unique participants.

**Success criteria, evaluated separately:**

- **Usability:** at least 80 % of those who complete the guided test perform all main tasks applicable to their role with little or no help. Little help means at most one general hint per task, without pointing out controls step by step or doing the task for them. Record task, result, and help needed.
- **Perceived usefulness:** at least 80 % of combined-session participants identify a personal situation in which they would use ClearPath. Report count and proportion separately from usability.
- Threshold = ceiling of 0.80 × number evaluated: 4 of 4, 4 of 5, or 5 of 6. Report the real denominator, absences, and tasks not evaluated. Extended-test results are complementary and are not merged into the guided-test denominator.

Keep invitations and consent, the common script linked to requirement IDs, fictional data, task and help logs, issues, comments, and applied changes. Screenshots contain no real financial data or unnecessary personal information. The advisor's technical demonstration and technical reviews by the iOS technical advisor are separate from user evaluation. Small-group results are not evidence of demand, profitability, or market representativeness.

Expected delivery: a functional version of the committed core, English technical documentation, a Spanish user guide, and verifiable test evidence.

## Implementation status

As of this update, no financial module is implemented. The repository contains the Expo Router starter structure with an initial screen; expo-sqlite and Firebase are not yet project dependencies. Week-4 evidence (earlier design proposals and a web screenshot of the initial screen) predates the scope update and does not demonstrate implemented financial functions or iPhone execution.

## Repository workflow

- master: verified versions.
- develop: integration.
- Dedicated task branches: focused changes reviewed through pull requests into develop.
- GitHub for version control; Trello for task tracking.

Preserve existing user work and repository policies. Keep planned scope separate from verified implementation status.

## Running the project

Use npm and the existing package-lock.json. Inspect package.json for current scripts and versions.

Install the project dependencies:

    npm install

Start the development server, then open the project in Expo Go on an iPhone:

    npx expo start

Consult AGENTS.md before changing dependencies or configuration.
