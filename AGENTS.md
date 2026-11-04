# Agent Instructions — ClearPath

## Read first

Read README.md for the self-contained summary of the current project scope (based on the updated anteproyecto), then design.md for visual and interaction guidance. The agent may not have access to the anteproyecto.

README.md is the functional scope authority; design.md must not expand it. This file governs working instructions and technical boundaries. Inspect existing code and configuration before assuming anything is implemented.

An earlier proposal was approved with a broader scope. Do not describe the updated scope as academically approved unless README.md records that approval.

If documents conflict, identify the conflict and ask a focused question rather than selecting an interpretation silently. Follow explicit current owner instructions and applicable repository policies.

## Communication and language

Communicate with Natalia in English, including explanations, clarification questions, and descriptions of design decisions. Use clear beginner-friendly explanations and ordered commands.

Write code, identifiers, technical comments, technical documentation, commits, and pull request titles/descriptions in English. User-facing application text and the participant guide must be Spanish for Costa Rica.

Explain material choices. Follow the owner-confirmed decisions in README.md (owner decisions, not academic approval). Ask about anything still open that materially affects implementation; do not invent authentication, invitation, permission, deletion, precision, or interest rules.

## Platform and environment

The owner develops on a Lenovo running Windows with VS Code and PowerShell. Target iOS/iPhone only; Windows is the development OS, not the product platform.

Do not assume access to a Mac, Xcode, an iPhone simulator, or an Apple Developer Program membership. Do not add Android work.

On-device execution uses Expo Go on real iPhones. Web preview is useful for interface review but is not an iOS test.

## Scope and priorities

Committed: individual and shared financial profiles with administrator/member roles; budget (limits by period, category, and currency; expense CRUD; used and available amounts); savings (goals, contributions, withdrawals, balance, progress); manual investment tracking (records, contributions, withdrawals, value updates, history); basic debt tracking (balance, installment, due date, payments, outstanding balance). All four modules exist in both profile types.

Desirable: income, custom categories/subcategories, payment method, expense splitting, recurring and non-monthly expenses, calendar, additional summaries/charts, reminders and local notifications, emergency-fund rules, investment performance analysis, debt interest/amortization/simulation, and shared offline operation. Start a desirable feature only when the committed core is integrated and verified, enough time remains to implement and test it without compromising corrections and documentation, and Natalia has explicitly authorized it. Do not add desirable work from week 15 onward.

Future work, outside the academic delivery: Wallet, Shortcuts, Apple Pay automation (no early experiment), App Store publication, broad TestFlight pilot, Android, additional languages/currencies, widgets, bank connections, in-app payments, automatic investment data, reimbursement settlement, spreadsheets, full cloud backup, and billing/premium enforcement.

Use README's delivery plan to select work. Protect integration, testing, correction, and documentation time. Build functional increments, not a separate prototype. A delay does not reclassify committed work as desirable; that change requires Natalia and the academic advisor.

## Technologies and API verification

Use React Native, Expo, TypeScript, Expo Router, expo-sqlite, and Firebase Authentication and Cloud Firestore through the **Firebase JavaScript SDK**. Only use libraries that run in Expo Go for the project's SDK. Do not write custom Swift code or custom native modules, and do not add React Native Firebase or other native-only libraries without a discussed decision.

Cloud Functions and paid Firebase plans are not planned. Evaluate necessity and cost, and ask Natalia, before adding any Firebase service beyond Authentication and Firestore on the free plan.

Inspect package.json and package-lock.json before changing APIs or dependencies. Consult official documentation matching installed versions for Expo, React Native, and the Firebase JavaScript SDK, including how the JS SDK handles auth session persistence in React Native. Do not rely on remembered APIs or assume latest docs match the project.

Expo Go supports a limited set of Expo SDK versions. Confirm early that the Expo Go available on the test iPhones runs the project's SDK, and avoid SDK changes that break that.

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

Keep route files under src/app/ using Expo Router; non-route reusable code stays outside it. Preserve existing conventions. Separate screens from module rules and data access.

The selected profile routes each operation: individual data to SQLite, shared data to Firestore. Individual financial data stays in SQLite and must never be read into, copied to, or synchronized with Firebase. Keep domain concepts coherent across both stores.

The individual profile must work without an account or internet. Shared profiles require authentication and internet for creating, viewing, and modifying data.

Do not build shared offline support, rely on Firestore offline persistence, or add a pending-sync queue; these are desirable features. Show shared records as saved only after the server confirms the write, and use supported SDK metadata to avoid presenting unconfirmed local state as saved. Make retries idempotent with stable identifiers so movements are not duplicated. Keep a movement and its balance consistent through transactions or by deriving balances from confirmed records.

## Financial rules

Keep CRC and USD separate; never convert or sum them together. Display one profile at a time.

Follow README's calculation rules for budget, savings, investments, and debts, and verify them with independent examples (zero movements, multiple movements, corrections, and limits). Do not add formulas beyond them, such as bank balance, net worth, returns, or interest.

Use the period start and end dates entered for a budget; do not derive periods from income or pay dates. Reject budget periods that overlap for the same category, currency, and profile. Deleting a budget never deletes its expenses.

Withdrawals cannot exceed a goal's recorded balance; investment withdrawals must be positive and cannot exceed the recorded value; payments cannot exceed a debt's outstanding balance. Debt balance adjustments are separate records (date, reason, author, amount of change) and never count as payments. Recalculate affected balances when movements are edited or deleted.

Archive, rather than delete, savings goals, investments, and debts that have movements; only empty ones can be deleted. Archived records stay viewable and reject new movements, in the UI and in storage rules.

Both CRC and USD use two decimal places. Store amounts in a fixed-precision form (for example, integer hundredths) and avoid floating-point arithmetic for stored values or totals.

## Security and privacy

Enforce identity, membership, roles, and authorship through Firebase Authentication and Firestore rules, not UI alone. Use exactly the two roles and the permission matrix in README.md. Authorship is the authenticated account; members cannot change authorship or raise their own role. Joining requires a signed-in user with a valid invitation code; an invalidated code must be rejected by the rules. Removed members lose access, but their records and authorship are preserved. Verify rules with an administrator, a member, a removed member, and an unrelated account.

Commit no secrets, private signing material, service accounts, or real financial data. Follow existing environment/ignore conventions. Use fictional data in tests and demos.

Do not implement custom end-to-end encryption or user-facing cloud backup.

Document SQLite integrity/transactions and Firestore collections/indexes/rules. The technical SQLite backup/restore exercise is academic validation, not a product feature.

## Builds and distribution

Development builds, EAS Build, TestFlight, and App Store distribution are not part of the planned validation. Do not configure them, create eas.json, change signing, start paid services, publish updates, or submit builds without an explicit request.

For Continuous Native Generation, use app configuration and plugins instead of manually creating/editing generated native directories. Do not prescribe local Xcode commands.

If a build workflow is explicitly requested later, run EAS CLI as:

    npx eas-cli@latest <command>

## Design and business intent

Follow design.md tokens, proposed typography, accessible layouts, and Spanish copy. Explain exact color/font choices and roles in English when changing them.

Navy preference is one input; product clarity, accessibility, consistency, and audience feedback guide decisions. Treat color psychology and commercial appeal as hypotheses, not guarantees of trust or sales.

Earlier Stitch screens are design proposals made before the scope update, not implemented features or requirements. Do not regenerate them unless asked.

Do not expose desirable or future features as available, add settlement dashboards, or imply bank access. Design must reflect actual implemented functionality.

## Git workflow

Preserve master as the main branch; never rename it to main.

Use develop for integration and dedicated branches for tasks. Inspect branch and working tree first. Start new work from an updated develop when available; preserve uncommitted work and do not reuse merged feature branches for new tasks.

Stage intended files explicitly. Keep commits focused and English. Follow configured branch policies and pull request review requirements.

Do not discard others' changes, force-push, rewrite shared history, delete branches, or merge pull requests without an explicit request. Task PRs normally target develop; do not merge into master incidentally.

## Verification and reporting

For code/configuration changes, run relevant lint/type checks and focused verification. For documentation-only changes, check Markdown and consistency; app tests are unnecessary.

Test affected calculations, persistence, permissions, connectivity errors, and retries meaningfully. Validate real-device behavior on an iPhone through Expo Go when required; static checks and web previews are not iOS tests. Link tests and results to README's requirement IDs (RF01–RF28).

Support README's validation plan with fictional data: the combined demonstration and guided test with 4–6 participants (aiming for at least 5) and the conditional extended test with 2–3 people. Evaluate usability and perceived usefulness separately, preserve the success criteria and limitations, and do not claim market validation.

Report what changed, why, checks/results, unverified behavior, and unresolved decisions. Never claim checks passed when not run.

Keep documentation synchronized with agreed decisions and distinguish planned from implemented features.
