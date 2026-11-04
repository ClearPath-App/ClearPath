# ClearPath — Product Design Specification

**Platform:** iPhone/iOS only  
**Status:** Proposed visual direction, subject to validation; aligned with the updated scope in README.md  
**UI:** Spanish for Costa Rica  
**Implementation:** React Native, Expo, TypeScript, existing Expo Router  
**Functional authority:** README.md

## 1. Product and commercial intent

ClearPath helps individuals, couples, families, and groups organize their budget, savings, manual investment tracking, and debts, in individual and shared financial profiles. The experience should make manual financial organization understandable and approachable.

Brand direction: calm, clear, competent, warm, and transparent. Proposed promise: "Tus finanzas, con un camino más claro."

Commercial differentiation should come from an understandable integration of the four modules across individual and shared profiles, quick recording, and honest connectivity states. Do not imitate a bank or trading platform, imply bank connectivity, or present profitability as proven.

Color associations are contextual design hypotheses, not universal psychological effects or evidence that a palette increases sales. Validate clarity, recognition, and perceived trust with users of the working application.

## 2. Scope constraints

Only expose committed functionality: individual and shared profiles, members and roles, budget, savings, investments, and debts.

Desirable features stay hidden until authorized and implemented: income, subcategories, custom categories, payment method, expense splitting, recurring expenses, calendar, additional charts, notifications, simulations, amortization, and shared offline mode. Do not show them as available, required screens, or empty premium teasers. Wallet and Shortcuts are future work.

Never display settlement balances, "who owes whom," reimbursements, or settlement actions.

The visual system does not authorize new product requirements. Privacy masking, custom onboarding, dark mode, extra charts, and haptics are design options, not committed features.

## 3. Palette and rationale

Navy anchors the brand; deep green distinguishes primary actions. Neutral surfaces keep forms and amounts readable. Amber highlights attention, such as an exceeded limit or an upcoming due date, without treating it as a failure. Red is reserved for errors and destructive operations.

| Token | Hex | Intended role |
| --- | --- | --- |
| brand.navy | #142D4E | Header accents, hero surface, brand identity |
| brand.navyDeep | #0D1E35 | Strong dark brand surface |
| brand.primary | #116B5A | Primary buttons, active navigation, selected controls |
| brand.primaryPressed | #0A4C43 | Pressed primary action |
| brand.primarySoft | #E1F4EC | Selected backgrounds and subtle highlights |
| brand.mint | #BDEDD8 | Decorative accents; not small text |
| background.page | #F5F7FA | Main canvas |
| background.surface | #FFFFFF | Cards, sheets, fields |
| background.subtle | #EDF2F7 | Secondary surfaces |
| text.primary | #172B42 | Headings, monetary amounts, body text |
| text.secondary | #526277 | Supporting text |
| text.inverse | #FFFFFF | Text on navy or deep green |
| border.subtle | #DCE3EC | Decorative separators |
| border.control | #718096 | Input boundaries when needed for visibility |
| state.success | #166647 | Confirmed outcomes, such as a confirmed shared save |
| state.successSoft | #E5F3EB | Success background |
| state.warning | #825000 | Operation in progress, exceeded limit, upcoming due date |
| state.warningSoft | #FFF3D9 | Warning background |
| state.danger | #B42332 | Errors and destructive actions |
| state.dangerSoft | #FCEBED | Error background |
| state.info | #245B9B | Informational content |
| state.infoSoft | #EAF1FB | Information background |
| chart.navy | #142D4E | Category series |
| chart.green | #116B5A | Category series |
| chart.blue | #3973AE | Category series |
| chart.purple | #7955A0 | Category series |
| chart.amber | #A56A16 | Category series |

Only the state.success and state.warning role descriptions changed with the scope update; hex values are unchanged. "Pending sync" was removed because shared offline mode is no longer committed.

Use navy and green with distinct roles; do not make every card dark or every amount green. Expenses, withdrawals, and payments are neutral labeled amounts, not automatically red. Do not use green to imply profit or investment gains; recorded investment value is not a return.

Use a light theme as the proposed initial design direction. Dark mode is not committed; adding it requires separate scope consideration and contrast verification. The template's existing dark-theme code may remain in the codebase; its presence does not require implementing dark mode.

## 4. Typography

Proposed family: **Inter**, with the native iOS system font as fallback. Use Regular 400, Medium 500, SemiBold 600, and Bold 700.

Inter is proposed for a consistent contemporary identity and readable interfaces. This is a design choice, not a claim that the font improves commercial conversion. Check current font-loading setup before introducing assets or dependencies; any font loading must work in Expo Go.

| Style | Size | Weight | Line height | Use |
| --- | --- | --- | --- | --- |
| Metric | 32 | 700 | 40 | Main module amount |
| Screen title | 26 | 700 | 34 | Main screen heading |
| Section title | 21 | 600 | 28 | Major section |
| Card title | 18 | 600 | 25 | Card heading |
| Body | 16 | 400 | 24 | Content and fields |
| Body emphasis | 16 | 600 | 24 | Important labels |
| Supporting | 14 | 400 | 20 | Descriptions |
| Caption | 12 | 500 | 17 | Secondary metadata |
| Button | 16 | 600 | 22 | Actions |

Use tabular figures where supported for amounts and percentages. Support system text scaling and wrapping; fixed heights must not clip enlarged text. Never reduce text below 12 to make a layout fit.

## 5. Accessibility and layout

- Use an adaptive iPhone layout, not a fixed screenshot-sized canvas.
- Reference viewport: 393 x 852 logical units.
- Respect safe areas, keyboard, and small-screen scrolling.
- Minimum touch target: 44 x 44 logical units.
- Spacing: 4, 8, 12, 16, 20, 24, 32, 40.
- Side padding: 20; card padding: 18–20.
- Corners: fields 12, buttons 14, cards 20.
- Use subtle shadows only when they help hierarchy.
- Check actual text/background pairs: target 4.5:1 for normal text and 3:1 for large text; check essential control/state indicators separately.
- Do not assume every palette combination passes contrast.
- Provide text/icon alternatives to color, accessible labels, and meaningful reading order.
- Respect reduced-motion preferences and do not rely on animation to explain a result.

## 6. Navigation

Use the existing Expo Router structure; routes live under src/app/. Do not introduce an App.tsx/screens navigation architecture to implement this document.

Flow, following the anteproyecto's navigation map: start and profile selector → individual profile, or account access → create or join a shared profile → the selected profile's four modules → list and detail, with create and edit actions according to permissions. Shared profiles also lead to member management for the administrator.

Five bottom tabs, all scoped to the selected profile (owner-confirmed on 2026-11-03, replacing the earlier "Movimientos / Planificar / Compartidos" tabs):
1. "Inicio"
2. "Presupuesto"
3. "Ahorros"
4. "Inversiones"
5. "Deudas"

Show a persistent profile selector where financial information is displayed. Examples: "Personal" and "Gastos del hogar". Pair a shared profile's name with an icon and label so context does not depend on color. The selector also leads to creating a shared profile or joining one with an invitation code ("Unirme con un código"). Members ("Integrantes") and settings are reached through labeled controls, not tabs. In "Integrantes", the administrator sees the current code with an "Invalidar código" action and can remove members; removing someone keeps their records and author name visible.

Show one selected profile at a time. Never mix currencies into a total.

The individual profile is local and needs no account. Do not place a login wall before individual use; request sign-in only when the person opens or creates a shared profile.

Avoid adding a multi-screen onboarding sequence before core functionality. If explanations are needed, use short contextual copy.

## 7. "Inicio" tab

Order:
1. Greeting or neutral welcome and settings access.
2. Selected profile.
3. Navy hero card: "Gastos del período" for the budget, with separate CRC/USD amounts when present.
4. Primary "Registrar gasto".
5. Category budget progress: used, available, or exceeded.
6. Short summaries of savings goals, investments, and debts, each linking to its tab.
7. Upcoming debt due dates, if any, as recorded information (no reminders).

"Disponible" may appear only as a budget's limit minus used, labeled with its category, period, and currency. No "Patrimonio", net worth, "Saldo bancario", or total combining modules or currencies. No income-based balance.

Use fictional examples explicitly as development fixtures, not hardcoded production totals.

## 8. Recording movements

Expense form: amount, currency, date, and category, in the selected profile. Initial categories: "Alimentación", "Vivienda y servicios", "Transporte", "Salud", "Educación", "Entretenimiento", "Otros". No subcategory, payment method, splitting, or notes fields unless authorized.

Savings and investment forms: amount, currency (fixed by the goal or investment), and date for contributions and withdrawals; investments also have an "Actualizar valor" action for the administrator or owner. Debt payment form: amount and date.

Debt balance adjustment form (administrator or owner): date, reason, and amount of change. Label it "Ajuste de saldo" and keep it visually separate from "Registrar abono"; an adjustment is never a payment.

Amount fields accept two decimal places for both CRC and USD.

Validate before saving with understandable Spanish messages, including withdrawals above the recorded balance or value, payments above the outstanding balance, and overlapping budget periods. A withdrawal is a record, not a transfer; word actions as "Registrar retiro" and "Registrar abono".

In shared profiles, show each movement's author. Show permissions honestly: when a role cannot perform an action, hide it or disable it with a short explanation. Storage rules still enforce permissions.

Deletion and archiving:
- Confirm deletion using the record's context.
- Deleting a budget states that its expenses are kept.
- A savings goal, investment, or debt with movements offers "Archivar" instead of "Eliminar"; an empty one can be deleted.
- Archived records appear in a separate, clearly labeled "Archivados" list, remain viewable, and show no actions for new movements.

## 9. Module screens

**Presupuesto:** period start and end dates, category, currency, limit, used, and available or exceeded amount ("Excedido por ₡3.200"). Label progress numerically and with text; do not rely on bars alone. No notifications.

**Ahorros:** goal name, currency, target, balance, progress percentage, and contributions/withdrawals history.

**Inversiones:** name, currency, "Valor registrado", date of the latest manual valuation, and history of contributions, withdrawals, and value updates. Do not label values as "rendimiento" or "ganancia", and do not show market data.

**Deudas:** name, currency, initial balance, installment, due date, outstanding balance ("Saldo pendiente"), and a history that distinguishes payments ("Abono") from balance adjustments ("Ajuste de saldo", with reason and author). Installment and due date are informational. No interest rate, payoff date, amortization, or simulation.

## 10. Components

Primary button: green fill, white text, minimum 52 height when text scaling permits, radius 14. Secondary button: white/subtle surface with visible boundary and navy text. Destructive action: danger text and clear confirmation where appropriate.

Cards: white surfaces with clear headings, relevant amounts, and restrained metadata. Prefer one primary purpose per card.

Fields: persistent labels, helper/error text, visible focus, appropriate keyboard. Place currency near amount input. Accept input with up to two decimal places, as documented in README.md.

Progress: basic labeled bars for budgets and savings goals, with exact values, period, and currency. Additional charts are desirable. Do not require a new chart dependency for decorative purposes.

Icons: use one existing icon family with consistent stroke and size. Icons supplement text for important actions.

## 11. Connectivity and feedback copy

Individual profile:
"Tus finanzas personales se guardan en este dispositivo."

Shared profile without connection:
"Sin conexión. Los perfiles compartidos necesitan internet para consultar y registrar información."

Shared operation in progress:
"Guardando…"

Confirmed shared save:
"Guardado."

Shared save error:
"No se pudo guardar. Revisa tu conexión e inténtalo de nuevo."

Initial shared access:
"Necesitas conexión para crear o unirte a un perfil compartido."

Permission limit (example):
"Solo el administrador puede modificar este presupuesto."

Withdrawal above balance (savings):
"El retiro no puede superar el saldo registrado."

Withdrawal above value (investments):
"El retiro no puede superar el valor registrado."

Overlapping budget period:
"Ya existe un presupuesto para esta categoría y moneda en esas fechas."

Archived record:
"Archivado. Puedes consultarlo, pero no registrar movimientos nuevos."

Invalid invitation code:
"El código no es válido o ya no está disponible."

Payment above balance:
"El abono no puede superar el saldo pendiente."

Empty expenses:
"Aquí aparecerán tus gastos. Registra tu primer gasto."

Budget exceeded:
"Superaste el límite de esta categoría para este período."

Show a shared record as saved only after the service confirms it. Do not show offline-pending states or promise offline shared recording. On retry, avoid creating duplicates.

## 12. Privacy and tone

Use respectful, nonjudgmental language. Prefer "Superaste el límite registrado" over blame or shame.

Never imply personal cloud backup. Explain the difference between local individual storage and online shared profiles.

If notifications are authorized later, do not expose sensitive amounts in their copy.

## 13. Earlier design proposals

Stitch screens created in week 4 (versions 1–3, kept as evidence in the anteproyecto) are earlier design proposals. They predate the scope update and are not implemented features or requirements. They may show elements that are now desirable or replaced, such as the "Movimientos / Planificar / Compartidos" tabs, pending-sync labels, calendar planning, or a single expense-focused home.

Use them only as references for the established visual direction: navy hero card, green primary action, white cards, labeled budget progress, and the profile selector. Do not regenerate them unless asked; new screens follow this document and README.md.

## 14. Implementation and review

Define reusable color, typography, spacing, and radius tokens outside route files. Reuse existing project conventions rather than restructuring unrelated code.

Before changing visual tokens, explain the exact values, their roles, and the reason in English. Significant changes to this proposal should be discussed, not silently introduced.

Verify:
- Selected profile is obvious.
- CRC/USD remain separate.
- Only committed, implemented modules appear.
- Role-limited actions are hidden or explained, and rules enforce them.
- Archived records are viewable but accept no new movements.
- Debt adjustments are never shown as payments.
- Shared saves appear as saved only after confirmation.
- No settlement, bank balance, net worth, interest, or return figures.
- All amounts derive from real records or explicitly fictional fixtures.
- Large text, keyboard, VoiceOver, contrast, and safe areas are considered.
- Commercial appeal remains a hypothesis to evaluate with users.
