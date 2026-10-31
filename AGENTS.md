# Agent Instructions — ClearPath

## Read first

Read README.md for the complete available summary of the approved project proposal, then design.md for visual guidance. The agent may not have access to the original proposal.

README.md is the functional scope authority; design.md must not expand it. This file governs implementation workflow. Inspect existing code and configuration before assuming anything is implemented.

If documents conflict, identify the conflict and ask a focused question rather than selecting an interpretation silently. Follow explicit current owner instructions and applicable repository policies.

## Communication and language

Communicate with Natalia in English, including explanations, clarification questions, and descriptions of design decisions. Use clear beginner-friendly explanations and ordered commands.

Write code, identifiers, technical comments, technical documentation, commits, and pull request titles/descriptions in English. User-facing application text and the participant guide must be Spanish for Costa Rica.

Explain material choices. Ask about unresolved requirements that materially affect implementation; do not invent authentication, permission, interest, or rounding rules.

## Platform and environment

The owner develops on a Lenovo running Windows with VS Code and PowerShell. Target iOS/iPhone only; Windows is the development OS, not the product platform.

Do not assume Xcode or a local iOS simulator. Do not add Android work.

## Scope and priorities

Committed features: personal/shared spaces and permissions; expense CRUD/classification/allocation; optional income; monthly/twice-monthly budgets; calendar/basic summaries; debts and one additional-payment simulation; local notifications; offline expenses and shared synchronization.

Desirable features: recurring/less frequent expenses, savings/emergency fund, manual investments, and experimental Wallet/Shortcuts. Implement a desirable feature only when committed functionality is complete, integrated, and verified, and sufficient time remains to implement and test it without compromising corrections and documentation. Natalia's explicit authorization is also required before starting any desirable feature; this is a working agreement, not an additional academic requirement. Exception: the early, bounded Wallet/Shortcuts feasibility experiment described in README.md is a planned feasibility test in the approved proposal, separate from full implementation of that desirable feature. It may take place before all committed functionality is complete, when the appropriate task is reached, but Natalia's explicit approval is required before starting it (also a working agreement, not an additional academic requirement). The experiment does not commit the project to the complete integration.

Deferred: bank integrations, app payments, reimbursements/settlement, automatic conversion, Android, additional currencies/languages, spreadsheets, full cloud backup, widgets, billing/premium enforcement.

Use README's delivery priorities to select work. Protect integration, testing, correction, and documentation time. An early feasibility experiment does not authorize full optional implementation. Build functional increments, not a separate prototype.

Do not require access to the original proposal to understand already documented requirements. Request missing decisions only when necessary.

## Technologies and API verification

Preserve React Native, Expo, TypeScript, Expo Router, SQLite, Firebase Authentication, and native-persistent Cloud Firestore. Cloud Functions are conditional; evaluate necessity and costs first.

Inspect package.json and package-lock.json before changing APIs or dependencies. Consult official documentation matching installed versions for Expo, React Native, Firebase, and EAS. Do not rely on remembered APIs or assume latest docs match the project.

Expo documentation:
https://docs.expo.dev/versions/
https://docs.expo.dev/llms.txt

Use npm and preserve package-lock.json; no yarn, pnpm, or bun. When adding Expo or React Native dependencies, use npx expo install so SDK-compatible versions are resolved. npm install remains valid for installing the project's existing dependencies. Do not install packages or change dependencies unless the current task explicitly calls for it. Use existing scripts and avoid unrelated SDK upgrades.

Typical commands, subject to repository configuration:

    npm install
    npx expo start
    npx expo lint
    npx tsc --noEmit
    npx expo-doctor

On PowerShell, npm.cmd/npx.cmd can avoid blocked .ps1 launchers without changing machine-wide execution policy.

Diagnose dependency incompatibilities before running broad fixes such as npx expo install --fix.

## Architecture and data

Keep route files under src/app/ using Expo Router; non-route reusable code stays outside it. Preserve existing conventions. Separate screens from financial rules, storage, and synchronization.

Personal-space financial information stays in SQLite and must not be uploaded to Firebase. Shared information uses Firestore with native local persistence. Keep domain concepts coherent across storage mechanisms.

Do not add a parallel custom synchronization queue without a demonstrated requirement and discussed design.

Shared offline access requires prior access and available local data. Account creation and initial shared access require internet. Do not impose mandatory online authentication for personal-only use without an explicit decision.

Distinguish pending local writes from cloud-confirmed records using supported metadata. Do not claim that other members received a local write before synchronization. Document conflict behavior.

## Financial and notification rules

Keep CRC and USD totals separate; never convert or sum them together. Display one space at a time.

Income entry is optional. Do not invent available/bank balance formulas.

Allocation is not reimbursement settlement. Validate allocations and document monetary precision/remainder rules.

Twice-monthly is not every two weeks. Define period boundaries explicitly.

Document interest and payment assumptions before debt calculations. Show payoff results as estimates and verify independent examples.

Use permission-based local notifications. Debt reminders are one day or one week before due dates. Budget alerts are at most one local notification per exceeded category limit per period. Update/cancel reminders when records change.

## Security and privacy

Enforce identity, membership, and permissions through Firebase Authentication and Firestore rules, not UI alone. Do not invent member roles without a decision.

Commit no secrets, private signing material, service accounts, or real financial data. Follow existing environment/ignore conventions. Use fictional data in tests and demos.

Do not implement custom end-to-end encryption or user-facing cloud backup.

Document SQLite integrity/transactions and Firestore collections/rules. The technical SQLite backup/restore exercise is academic validation, not a product cloud feature.

## Builds and distribution

The full native Firebase stack requires an Expo development build. Do not assume Expo Go runs the complete product.

Inspect app configuration and eas.json before build changes. On Windows, use a configured cloud iOS build workflow where appropriate; do not prescribe local Xcode commands.

Run EAS CLI as:

    npx eas-cli@latest <command>

For Continuous Native Generation, use app configuration and plugins instead of manually creating/editing generated native directories.

Test distribution and Apple signing requirements remain to be verified. App Store publication is not an assumed deliverable. Do not start paid services, publish updates, submit builds, or change signing/distribution settings without an explicit request.

## Design and business intent

Follow design.md tokens, proposed typography, accessible layouts, and Spanish copy. Explain exact color/font choices and roles in English when changing them.

Navy preference is one input; product clarity, accessibility, consistency, and audience feedback guide decisions. Treat color psychology and commercial appeal as hypotheses, not guarantees of trust or sales.

Do not expose optional features as available, add settlement dashboards, or imply bank access. Design must reflect actual implemented functionality.

## Git workflow

Preserve master as the main branch; never rename it to main.

Use develop for integration and dedicated branches for tasks. Inspect branch and working tree first. Start new work from an updated develop when available; preserve uncommitted work and do not reuse merged feature branches for new tasks.

Stage intended files explicitly. Keep commits focused and English. Follow configured branch policies and pull request review requirements.

Do not discard others' changes, force-push, rewrite shared history, delete branches, or merge pull requests without an explicit request. Task PRs normally target develop; do not merge into master incidentally.

## Verification and reporting

For code/configuration changes, run relevant lint/type checks and focused verification. For documentation-only changes, check Markdown and consistency; app tests are unnecessary.

Test affected financial calculations, persistence, permissions, and sync behavior meaningfully. Validate real-device behavior on an iPhone when required; static checks and web previews are not iOS tests.

Support the README's 15-person usability procedure with fictional data. Preserve its success criterion and limitations without claiming market validation.

Report what changed, why, checks/results, unverified behavior, and unresolved decisions. Never claim checks passed when not run.

Keep documentation synchronized with approved decisions and distinguish planned from implemented features.
