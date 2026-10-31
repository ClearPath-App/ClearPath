# ClearPath

An iPhone application under development for personal and shared financial management in Costa Rica.

**Owner:** Natalia Martínez Villegas  
**Context:** TPA-5001 Proyecto Empresarial, Software Application Development technical program.  
**Status:** Approved proposal; incremental implementation. This document defines planned requirements, not completed functionality.

## Authoritative project context

This README is the repository's self-contained summary of the approved project proposal. An implementation agent may not have access to the original proposal: use the requirements and boundaries below without inventing missing decisions. AGENTS.md defines working instructions; design.md defines the proposed visual system. Design suggestions must not expand functional scope.

The product serves individuals, couples, families, and groups who manually record financial information. It brings expenses, budgets, upcoming payments, and debt tracking into one application. It is an independent entrepreneurship project, not software commissioned by an existing business.

The first release targets **iOS/iPhone only**, including every participant in shared spaces. Android development and testing are deferred.

## Languages and development environment

- Code, identifiers, technical comments, README, and technical documentation: English.
- Application interface, validation messages, notifications, and participant user guide: Spanish for Costa Rica.
- Current development computer: Lenovo running Windows, with VS Code and PowerShell. Do not assume local Xcode or an iOS simulator.

## Committed first-version requirements

### 1. Financial spaces and permissions

Each user can use a private personal space and create or join shared spaces with two or more members. Shared access and modifications depend on identity, membership, and permissions.

Account creation and initial entry into shared spaces require internet. The precise authentication flow, invitation mechanism, and permission matrix must be defined before implementation; they are not specified by this summary. Do not assume that personal-only use requires an online account.

### 2. Expenses and classification

Create, view, edit, and delete expenses with date, amount, currency, category, subcategory, and payment method. Provide initial categories and support customization of categories and subcategories.

Shared expenses can be allocated equally, by percentages, or by different fixed amounts. Support common group expenses. Validate allocation totals and define rounding rules.

Allocation records each member's share; **calculating or settling reimbursements between members is excluded**. Do not introduce settlement balances, transfers, or "who owes whom" workflows.

### 3. Optional income and budgets

Income entry is optional. Expense management must remain usable without income records. Users recording income can indicate monthly or twice-monthly pay and configure payment dates.

Support monthly or twice-monthly budgets, editable spending limits by category, and progress tracking. Twice-monthly means two periods per month, not every two weeks; exact period boundaries remain a product decision.

When a category limit is exceeded, show a visual warning and send one local notification for that limit in the corresponding period, subject to notification permission. Avoid repeated alerts for the same limit and period.

### 4. Calendar and basic summaries

For the selected space, show daily expense totals and distinguish upcoming debt due dates. Selecting a date reveals its movements.

Summaries show expenses by category and subcategory, with separate totals per currency. Initially display one space at a time; do not consolidate multiple spaces into a single financial overview.

### 5. Debts and additional-payment simulation

Record personal or shared debts with outstanding balance, interest rate, installment amount, payment date, and payments toward the debt.

Simulate one additional payment amount at a time to estimate its effect on the payoff date. Define and document the calculation assumptions before implementation, including interest conventions and payment treatment. Validate results against independently calculated examples.

Label results as estimates based on entered data and documented assumptions, subject to the actual debt terms. This is not a lender connection or a guaranteed payoff calculation.

### 6. Local reminders

With device notification permission, allow a debt reminder either one day or one week before its due date. Handle scheduling, changes, and cancellation.

Recurring-expense reminders are conditional on implementing that desirable feature; they are not an independent committed module.

### 7. Offline expenses and shared synchronization

Personal expenses can be recorded and viewed offline and persist locally across application restarts.

Previously accessed shared spaces can expose locally available information and accept new expenses offline when the necessary space data is present. Clearly distinguish pending transactions from transactions confirmed in the cloud.

When connectivity returns, Firestore attempts to send pending changes and receive updates. Other members see an offline-created expense after successful synchronization, not immediately upon its local creation.

Test connectivity loss, recovery, write failures, and simultaneous edits. Define unresolved conflict behavior rather than claiming conflict prevention without evidence.

## Currency and financial integrity

Support CRC and USD with separate amounts and totals. No automatic conversion, cross-currency total, or additional currencies.

Financial information is entered manually. Optional income must not be treated as a required input or an automatically verified bank balance.

Document money representation, precision, allocation rounding, budget boundaries, and debt assumptions. Do not invent these rules silently.

## Approved technical architecture

- React Native, Expo, TypeScript, and the existing Expo Router navigation.
- SQLite for personal financial information.
- Firebase Authentication for users accessing shared spaces.
- Cloud Firestore with native iOS SDK local persistence for shared financial information.
- Firebase Cloud Functions only if additional server-side logic is necessary, after evaluating requirements and costs.

### Storage boundaries

Personal-space financial data remains in SQLite on the device and **is not synchronized with Firebase**.

Shared-space information is stored in Firestore, with native persistence supporting previously loaded data and pending offline writes. Do not replace this architecture with a different backend or automatically replicate personal records into shared storage.

Maintain coherent domain concepts for spaces, movements, categories, budgets, and debts while adapting physical structures to SQLite and Firestore.

Shared synchronization is distinct from full cloud backup and recovery, which is deferred.

### Builds and test distribution

Prepare an Expo development build for the native Firebase integration and real-iPhone testing. Expo Go alone is not the planned environment for the complete stack.

Verify installation, persistence, synchronization between accounts, and notifications on real devices. Determine the test-distribution mechanism and applicable signing requirements; Apple Developer Program membership is contemplated when needed.

App Store publication is not established as a required first-version deliverable. Do not assume that a local iOS build is available on Windows or that all testers must share one Wi-Fi network.

## Security and database evidence

Use Firebase Authentication and Firestore security rules to enforce identity, membership, and permissions. Client-side visibility controls alone are insufficient.

Store only necessary shared information. Use Firebase's provided protection mechanisms; custom end-to-end encryption is not part of the proposal.

Document SQLite's logical and physical design, relationships, integrity constraints, queries, and transactions. Document Firestore collections, document relationships, and access rules.

Test permitted access and rejected access to unrelated spaces with multiple accounts.

Perform and document a technical SQLite backup-and-restoration exercise with fictional data and verify recovered records. This is academic database evidence, not a user-facing cloud backup feature.

## Desirable features: conditional, not committed

Only consider implementation after committed functionality is integrated and verified and sufficient time remains for testing, corrections, and documentation:

- Recurring expenses.
- Less frequent expenses such as annual vehicle registration: suggested reserves per period and records of amounts actually set aside.
- Savings goals and emergency fund: contributions, withdrawals, optional targets, and progress.
- Manual investments: contributions, withdrawals, received gains, manually updated values, and history; no external market-data retrieval.
- Experimental Apple Wallet and Shortcuts automation to help register in-person Apple Pay purchases.

The Wallet/Shortcuts option requires a bounded feasibility test covering activation, available data, and transfer into ClearPath, including execution compatibility. A successful experiment does not automatically authorize full integration. Manual expense entry remains available.

If a desirable feature cannot be completed and tested without compromising committed delivery, defer it. These development priorities do not determine free versus paid features.

## Deferred or excluded from the first version

- Android support and testing.
- Bank connections and external financial-institution integrations.
- Automatic external investment data.
- Payments from within the application.
- Reimbursement calculation and settlement.
- Automatic exchange conversion and additional currencies.
- Internationalization and additional interface languages.
- Budget import/export to Excel or other formats.
- Live Excel or Google Sheets synchronization.
- Full cloud backup and recovery of financial information.
- Home-screen widgets.
- Billing, subscriptions, and premium-feature enforcement.

A freemium model is a future business hypothesis. The division of free and paid features is not finalized, and no payment integration is required now.

## Delivery priorities and validation

Develop the actual application incrementally, without a separate prototype.

Implementation order: environment/repository and architecture/data models; real-iPhone execution, basic SQLite and Firebase checks, and distribution feasibility; access/navigation and personal expenses; shared spaces and permissions; allocation and offline synchronization; income/budgets; calendar/summaries; debts/simulation; local notifications; integration and corrections; usability evaluation and final evidence.

Keep documentation current throughout development. Optional work must not displace verification, corrections, or delivery preparation. The Wallet/Shortcuts feasibility experiment can occur early, without committing to full implementation.

The advisor's technical demonstration is separate from usability evaluation.

Evaluate the functional application with **15 volunteer iPhone users**, using fictional financial data. Include expense entry, switching between personal/shared spaces, budget consultation, debt simulation, and shared workflows in pairs or small groups. Check understanding of pending versus synchronized information.

Record task completion, help required, difficulties, and comments. Initial usefulness is supported if at least 12 of 15 participants identify a personal use case and complete the main tasks with little or no help. Little help means a general hint, not completing the task for them.

Use findings to prioritize corrections and verify fixes. This small evaluation is not representative market research or proof of profitability.

Expected delivery includes the functional committed scope, English technical documentation, a Spanish user guide, and test evidence.

## Repository workflow

- master: verified versions.
- develop: integration.
- Dedicated task branches: focused changes reviewed through pull requests into develop.

Preserve existing user work and repository policies. Keep planned scope separate from verified implementation status.

## Running the project

Use npm and the existing package-lock.json. Inspect package.json for current scripts and versions.

Install the project dependencies:

    npm install

Start the development server:

    npx expo start

Native functionality requires the configured development build. Consult AGENTS.md before changing dependencies or build configuration.
