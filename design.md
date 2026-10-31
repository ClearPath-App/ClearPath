# ClearPath — Product Design Specification

**Platform:** iPhone/iOS only  
**Status:** Proposed visual direction, subject to validation  
**UI:** Spanish for Costa Rica  
**Implementation:** React Native, Expo, TypeScript, existing Expo Router  
**Functional authority:** README.md

## 1. Product and commercial intent

ClearPath helps individuals, couples, families, and groups organize expenses, budgets, and debts. The experience should make manual financial organization understandable and approachable.

Brand direction: calm, clear, competent, warm, and transparent. Proposed promise: "Tus finanzas, con un camino más claro."

Commercial differentiation should come from an understandable integration of personal and shared finance, quick expense entry, and honest offline behavior. Do not imitate a bank or trading platform, imply bank connectivity, or present profitability as proven.

Color associations are contextual design hypotheses, not universal psychological effects or evidence that a palette increases sales. Validate clarity, recognition, and perceived trust with users of the working application.

## 2. Scope constraints

Only expose committed modules: spaces, expenses, optional income, budgets, calendar, summaries, debts, extra-payment simulation, local notifications, and offline/shared sync states.

Savings goals, emergency fund, recurring/less frequent expenses, investments, and Wallet/Shortcuts remain desirable. Do not show them as available, required screens, or empty premium teasers before authorized implementation.

Never display settlement balances, "who owes whom," reimbursements, or settlement actions. Shared allocation and recorded debt payments are different concepts.

The visual system does not authorize new product requirements. Privacy masking, custom onboarding, dark mode, extra charts, and haptics are design options, not committed features.

## 3. Palette and rationale

Navy anchors the brand; deep green distinguishes primary financial actions. Neutral surfaces keep forms and amounts readable. Amber highlights attention without treating every pending item as a failure. Red is reserved for errors and destructive operations.

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
| state.success | #166647 | Confirmed outcomes |
| state.successSoft | #E5F3EB | Success background |
| state.warning | #825000 | Pending sync and upcoming due dates |
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

Use navy and green with distinct roles; do not make every card dark or every amount green. Expenses are neutral labeled amounts, not automatically red. Do not use green to imply profit or available money when only expense data exists.

Use a light theme as the proposed initial design direction. Dark mode is not a committed requirement of the approved proposal; adding it requires separate scope consideration and contrast verification. The template's existing dark-theme code may remain in the codebase; its presence does not require implementing dark mode.

## 4. Typography

Proposed family: **Inter**, with the native iOS system font as fallback. Use Regular 400, Medium 500, SemiBold 600, and Bold 700.

Inter is proposed for a consistent contemporary identity and readable interfaces. This is a design choice, not a claim that the font improves commercial conversion. Check current font-loading setup before introducing assets or dependencies.

| Style | Size | Weight | Line height | Use |
| --- | --- | --- | --- | --- |
| Metric | 32 | 700 | 40 | Main expense total |
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

Proposed four bottom tabs:
1. "Inicio"
2. "Movimientos"
3. "Planificar"
4. "Compartidos"

Use a visible "Registrar gasto" action on the "Inicio" and "Movimientos" tabs. "Planificar" provides budgets, calendar, and debts. Settings are reached through a labeled profile/settings control.

Show a persistent space selector where financial information is displayed. Examples: "Personal" and "Gastos del hogar". Pair a shared-space name with an icon and label so context does not depend on color.

Consult one selected space at a time. Never mix currencies into a total.

Personal financial data is local. Do not impose an online login wall on personal use based solely on this design; the authentication flow needs an explicit product decision.

Avoid adding a multi-screen onboarding sequence before core functionality. If explanations are needed, use short contextual copy.

## 7. "Inicio" tab

Order:
1. Greeting or neutral welcome and settings access.
2. Selected financial space.
3. Navy hero card: "Gastos del mes", with separate CRC/USD amounts when present.
4. Primary "Registrar gasto" and secondary "Ver movimientos".
5. Category budget progress.
6. Basic category/subcategory spending summary.
7. Upcoming debt payments, if any.

No savings preview in the committed layout. No "Disponible", "Patrimonio", or "Saldo bancario" metric without an approved, documented formula and the necessary inputs. Optional income does not establish a verified balance.

Use fictional examples explicitly as development fixtures, not hardcoded production totals.

## 8. Expense entry and shared information

Keep the form focused on approved fields: amount, currency, date, category, subcategory, payment method, and selected space.

For shared expenses, show participants and allocation method:
- "Partes iguales"
- "Por porcentaje"
- "Por monto"

Make the allocated total and any difference visible, with understandable validation. Define remainder/rounding rules before implementing.

Do not add unapproved mandatory fields such as payer identity or notes without a product decision. Do not present allocated shares as reimbursement debts.

Show permissions honestly, including denied edits and unavailable data. Confirm deletion using the record context.

## 9. Budgets, calendar, and debts

Budgets: show period, category limit, recorded expenses, and remaining or exceeded amount, separated by currency. Label progress numerically and with text; do not rely on bars alone.

Calendar: show recorded daily expenses and debt due dates. Selecting a date opens relevant movements. Recurring bill scheduling is conditional on optional implementation.

Debts: show entered balance, interest rate, installment, due date, and recorded payments. The simulation accepts one additional amount at a time and explains assumptions and estimated payoff results.

Do not show decorative precision or a payoff date when the entered terms do not support a valid calculation.

## 10. Components

Primary button: green fill, white text, minimum 52 height when text scaling permits, radius 14. Secondary button: white/subtle surface with visible boundary and navy text. Destructive action: danger text and clear confirmation where appropriate.

Cards: white surfaces with clear headings, relevant amounts, and restrained metadata. Prefer one primary purpose per card.

Fields: persistent labels, helper/error text, visible focus, appropriate keyboard. Place currency near amount input. Accept input according to a documented locale/precision policy.

Charts: basic summaries only. Prefer accessible horizontal bars and a labeled list before introducing complex charts. Include period, currency, category, and exact values. Do not require a new chart dependency for decorative purposes.

Icons: use one existing icon family with consistent stroke and size. Icons supplement text for important actions.

## 11. Offline and feedback copy

Personal space:
"Tus finanzas personales se guardan en este dispositivo."

Pending shared expense:
"Guardado en este dispositivo. Pendiente de sincronizar."

Offline shared information:
"Sin conexión. Se muestra la información disponible en este dispositivo."

Confirmed shared expense:
"Sincronizado."

Synchronization error:
"No se pudo sincronizar. Revisa tu conexión."

Initial shared access:
"Necesitas conexión para crear o unirte a un espacio compartido."

Empty expenses:
"Aquí aparecerán tus movimientos. Registra tu primer gasto."

Budget exceeded:
"Superaste el límite de esta categoría para este período."

Notifications denied:
"Puedes activar las notificaciones en los ajustes del iPhone."

A local save is not cloud confirmation. Do not hide cached content behind a blocking network loader. Avoid promises of guaranteed background delivery or instant visibility to other members.

## 12. Privacy and tone

Use respectful, nonjudgmental language. Prefer "Superaste el límite registrado" over blame or shame.

Do not expose sensitive amounts unnecessarily in reminder copy. Keep notifications concise; exact wording is a design decision to validate.

Never imply personal cloud backup. Explain the difference between local personal storage and shared synchronization.

## 13. Implementation and review

Define reusable color, typography, spacing, and radius tokens outside route files. Reuse existing project conventions rather than restructuring unrelated code.

Before changing visual tokens, explain the exact values, their roles, and the reason in English. Significant changes to this proposal should be discussed, not silently introduced.

Verify:
- Selected space is obvious.
- CRC/USD remain separate.
- Income is optional.
- Only authorized implemented modules appear.
- Shared allocation contains no settlement workflow.
- Pending and confirmed writes are distinct.
- All amounts derive from real records or explicitly fictional fixtures.
- Large text, keyboard, VoiceOver, contrast, and safe areas are considered.
- Commercial appeal remains a hypothesis to evaluate with users.
