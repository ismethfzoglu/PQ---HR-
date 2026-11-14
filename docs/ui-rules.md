# UI and Color Rules

Rules for every screen of the attendance platform frontend: **React + TypeScript + Vite + Tailwind CSS v4**, palette **Warm Modern**.

## How to use this file

- Follow every rule here when writing or changing frontend code. **MUST** and **NEVER** are hard rules; anything else is the default unless the user says otherwise.
- If a request conflicts with a rule, say so and ask before breaking it.
- Before finishing any UI task, run the **Ship checklist** at the end of this file and fix what fails.
- Never invent a size, color, spacing, radius or z-index value that isn't listed here.

---

## 1. Color palette (60 / 30 / 10)

| Role | Share | Main color | Used for |
| --- | --- | --- | --- |
| Base | 60% | Cream `#FAF7F2` | page background, cards, tables |
| Brand | 30% | Slate `#2D3142` | sidebar, top bar, headings, primary text |
| Accent | 10% | Terracotta `#E07A5F` | the one main action per screen, highlights, active states |

### Scales

| Token | Hex | Use |
| --- | --- | --- |
| cream-50 | `#FFFDFA` | cards and panels |
| cream-100 | `#FAF7F2` | page background |
| cream-200 | `#F1ECE3` | hover rows, subtle fills, table header |
| cream-300 | `#E5DED2` | borders and dividers |
| slate-900 | `#1B1E29` | sidebar in dark mode, pressed states |
| slate-800 | `#2D3142` | sidebar, top bar, headings, primary text |
| slate-600 | `#5C6070` | secondary text |
| slate-400 | `#8A8FA3` | muted text, placeholders, icons |
| slate-200 | `#C9CCD6` | disabled borders |
| slate-100 | `#E6E7EC` | slate-tinted chips |
| terra-700 | `#A8472F` | button pressed, links, accent text on cream |
| terra-600 | `#C4583D` | primary button background (white text) |
| terra-500 | `#E07A5F` | highlights, active tab line, focus ring, icons, chart highlight |
| terra-300 | `#EEA894` | dark-mode accent text |
| terra-100 | `#FBE7E0` | tint behind selected items |

### Attendance status colors

Status colors mean the same thing on every screen. **NEVER** use them for branding or decoration, and **NEVER** use terracotta to signal a status (it's too close to absent red).

| Status | Strong (dots, icons, charts) | Tint (chip/cell background) | Text on tint |
| --- | --- | --- | --- |
| Present | `#16A34A` | `#DCFCE7` | `#14532D` |
| Late | `#F59E0B` | `#FEF3C7` | `#78350F` |
| Absent | `#DC2626` | `#FEE2E2` | `#7F1D1D` |
| On leave | `#3B82F6` | `#DBEAFE` | `#1E3A8A` |
| Holiday | `#8B5CF6` | `#EDE9FE` | `#4C1D95` |
| Day off | `#8A8FA3` | `#E6E7EC` | `#2D3142` |

- Tables and the monthly sheet use the **tint + text** pair. The **strong** color is only for small marks: a dot, an icon, a chart bar.
- Status is **NEVER** shown by color alone: always include the word or an icon (`StatusBadge` does this).

### Where each color goes

| Element | Light mode | Tailwind |
| --- | --- | --- |
| Page background | cream-100 | `bg-bg` |
| Cards, panels, modals | cream-50 with cream-300 border | `bg-surface border border-border` |
| Table header, hover row | cream-200 | `bg-surface-hover` |
| Sidebar and top bar | slate-800 bg, cream-100 text | `bg-brand text-on-brand` |
| Headings and body text | slate-800 | `text-text` |
| Secondary text, labels | slate-600 | `text-text-secondary` |
| Placeholders, muted icons | slate-400 | `text-text-muted` |
| Primary button (max one per screen) | terra-600, white text | `bg-primary text-on-primary` |
| Active nav item, active tab line | terra-500 | `bg-accent` / `border-accent` |
| Selected row or card | terra-100 | `bg-accent-tint` |
| Links, accent text | terra-700 | `text-accent-text` |
| Dashboard numbers | slate-800; terra-600 only for the one headline number | `text-text` |
| Charts | slate-800 main series, terra-500 highlight, slate-400 others | |
| Form errors | absent red | `text-status-absent` |

### Contrast

Body text **MUST** reach 4.5:1 (WCAG AA); large text (18px+, or 14px+ bold) and icons 3:1.

| Text | On | Approx. | Allowed for |
| --- | --- | --- | --- |
| slate-800 | cream-100 | ~12:1 | all text |
| slate-600 | cream-100 | ~6:1 | secondary text |
| slate-400 | cream-100 | ~3:1 | placeholders, icons, large text only |
| cream-100 | slate-800 | ~12:1 | sidebar and top bar text |
| white | terra-600 | ~4.5:1 | button labels |
| white | terra-500 | ~3:1 | large/bold text only, **NEVER** small labels |
| terra-700 | cream-100 | ~6:1 | links, accent text |

**NEVER** use terra-500 for body text on cream. Verify final pairs with a contrast checker before launch.

### Dark mode

Same 60/30/10 split: dark slate replaces cream, and the accent gets lighter.

| Token | Light | Dark |
| --- | --- | --- |
| Page background | `#FAF7F2` | `#16181F` |
| Card / panel | `#FFFDFA` | `#1F222C` |
| Hover row, table header | `#F1ECE3` | `#272A36` |
| Border | `#E5DED2` | `#2E3240` |
| Sidebar / top bar | `#2D3142` | `#1B1E29` |
| Primary text | `#2D3142` | `#F1ECE3` |
| Secondary text | `#5C6070` | `#B6B9C4` |
| Muted text | `#8A8FA3` | `#7D8193` |
| Primary button | `#C4583D` | `#E07A5F` (slate-900 text) |
| Accent text, links | `#A8472F` | `#EEA894` |
| Selected background | `#FBE7E0` | `#3A2620` |

Status tints in dark mode: the strong color at ~18% opacity over the card, with the strong color as the text.

---

## 2. Tokens in code (single source of truth)

Components use **semantic Tailwind classes** (`bg-surface`, `text-text-secondary`, `bg-primary`), which read CSS variables, so dark mode is one switch on `<html data-theme="dark">`.

- **NEVER** write raw hex values, `rgb()` or Tailwind palette colors (`bg-orange-500`, `text-gray-700`) in components.
- **NEVER** use arbitrary values (`p-[13px]`, `z-[999]`, `bg-[#fff]`, `text-[17px]`).

### `src/styles/app.css`

Raw values use the `--app-*` prefix; Tailwind's `@theme inline` maps them to `--color-*` utilities. (Mapping `--color-bg: var(--color-bg)` would be circular, so the prefixes must differ.)

```css
@import "tailwindcss";
@custom-variant dark (&:where([data-theme="dark"], [data-theme="dark"] *));

:root {
  /* 60% base */
  --app-bg: #FAF7F2;
  --app-surface: #FFFDFA;
  --app-surface-hover: #F1ECE3;
  --app-border: #E5DED2;
  /* 30% brand */
  --app-brand: #2D3142;
  --app-text: #2D3142;
  --app-text-secondary: #5C6070;
  --app-text-muted: #8A8FA3;
  --app-on-brand: #FAF7F2;
  /* 10% accent */
  --app-primary: #C4583D;
  --app-primary-hover: #A8472F;
  --app-accent: #E07A5F;
  --app-accent-text: #A8472F;
  --app-accent-tint: #FBE7E0;
  --app-on-primary: #FFFFFF;
  /* status */
  --app-present: #16A34A;  --app-present-tint: #DCFCE7;  --app-present-text: #14532D;
  --app-late: #F59E0B;     --app-late-tint: #FEF3C7;     --app-late-text: #78350F;
  --app-absent: #DC2626;   --app-absent-tint: #FEE2E2;   --app-absent-text: #7F1D1D;
  --app-leave: #3B82F6;    --app-leave-tint: #DBEAFE;    --app-leave-text: #1E3A8A;
  --app-holiday: #8B5CF6;  --app-holiday-tint: #EDE9FE;  --app-holiday-text: #4C1D95;
  --app-off: #8A8FA3;      --app-off-tint: #E6E7EC;      --app-off-text: #2D3142;
}

[data-theme="dark"] {
  --app-bg: #16181F;
  --app-surface: #1F222C;
  --app-surface-hover: #272A36;
  --app-border: #2E3240;
  --app-brand: #1B1E29;
  --app-text: #F1ECE3;
  --app-text-secondary: #B6B9C4;
  --app-text-muted: #7D8193;
  --app-on-brand: #F1ECE3;
  --app-primary: #E07A5F;
  --app-primary-hover: #EEA894;
  --app-accent-text: #EEA894;
  --app-accent-tint: #3A2620;
  --app-on-primary: #1B1E29;
}

@theme inline {
  --color-bg: var(--app-bg);
  --color-surface: var(--app-surface);
  --color-surface-hover: var(--app-surface-hover);
  --color-border: var(--app-border);
  --color-brand: var(--app-brand);
  --color-text: var(--app-text);
  --color-text-secondary: var(--app-text-secondary);
  --color-text-muted: var(--app-text-muted);
  --color-on-brand: var(--app-on-brand);
  --color-primary: var(--app-primary);
  --color-primary-hover: var(--app-primary-hover);
  --color-accent: var(--app-accent);
  --color-accent-text: var(--app-accent-text);
  --color-accent-tint: var(--app-accent-tint);
  --color-on-primary: var(--app-on-primary);
  --color-status-present: var(--app-present);
  --color-status-present-tint: var(--app-present-tint);
  --color-status-present-text: var(--app-present-text);
  --color-status-late: var(--app-late);
  --color-status-late-tint: var(--app-late-tint);
  --color-status-late-text: var(--app-late-text);
  --color-status-absent: var(--app-absent);
  --color-status-absent-tint: var(--app-absent-tint);
  --color-status-absent-text: var(--app-absent-text);
  --color-status-leave: var(--app-leave);
  --color-status-leave-tint: var(--app-leave-tint);
  --color-status-leave-text: var(--app-leave-text);
  --color-status-holiday: var(--app-holiday);
  --color-status-holiday-tint: var(--app-holiday-tint);
  --color-status-holiday-text: var(--app-holiday-text);
  --color-status-off: var(--app-off);
  --color-status-off-tint: var(--app-off-tint);
  --color-status-off-text: var(--app-off-text);
}

@theme {
  --font-sans: "Inter", "IBM Plex Sans Arabic", system-ui, sans-serif;
  --text-base: 15px;  --text-base--line-height: 24px;
  --text-xl: 22px;    --text-xl--line-height: 30px;
  --text-2xl: 28px;   --text-2xl--line-height: 36px;
}
```

Everything else (spacing, `text-xs`, `text-sm`, `text-lg`, radius, z-index, breakpoints) uses Tailwind's defaults.

---

## 3. Foundations

### Control heights

Every control (input, select, combobox, date picker, search, button) shares the same height per size, so they line up in a row. **NEVER** set a custom control height.

| Size | Height | Tailwind | Font | Icon | Use |
| --- | --- | --- | --- | --- | --- |
| `sm` | 32px | `h-8 px-3 text-sm` | 14px | 16px | table toolbars, filters, dense rows |
| `md` (default) | 40px | `h-10 px-4 text-base` | 15px | 20px | all forms and normal buttons |
| `lg` | 48px | `h-12 px-5 text-base` | 15px | 20px | login, mobile primary actions |

On phones (below `md`, 768px), `md` controls grow to 44px: `h-11 md:h-10`.

### Spacing (4px grid, Tailwind defaults)

| Class | Value | Use |
| --- | --- | --- |
| `1` | 4px | icon to text |
| `2` | 8px | label to input, items in a row |
| `3` | 12px | between related fields |
| `4` | 16px | card padding (mobile), between form fields |
| `6` | 24px | card padding (desktop), between groups in a card |
| `8` | 32px | between page sections |
| `12` | 48px | page top padding |

### Radius

| Class | Value | Use |
| --- | --- | --- |
| `rounded-lg` | 8px | inputs, buttons, chips, badges |
| `rounded-xl` | 12px | cards, modals, dropdown menus, toasts |
| `rounded-full` | — | avatars, status dots, pill badges |

**NEVER** use `rounded-md`, `rounded-2xl` or other radii.

### Typography

| Class | Size / line height | Weight | Use |
| --- | --- | --- | --- |
| `text-xs` | 12 / 16px | 500 | badges, table meta |
| `text-sm` | 14 / 20px | 400 | table cells, hints, helper text |
| `text-base` | 15 / 24px | 400 | body text, input text |
| `text-lg` | 18 / 28px | 600 | card titles |
| `text-xl` | 22 / 30px | 600 | page titles |
| `text-2xl` | 28 / 36px | 600 | dashboard headline numbers |

Weights: 400 text, 500 badges, 600 labels, titles and numbers. Fonts: Inter (Latin) and IBM Plex Sans Arabic (Arabic, Kurdish), self-hosted.

### Z-index

| Layer | Class |
| --- | --- |
| Sticky table header | `z-10` |
| Sidebar, top bar | `z-20` |
| Dropdown, popover, tooltip | `z-30` |
| Modal backdrop and modal | `z-40` |
| Toast | `z-50` |

**NEVER** use any other z-index.

### Focus

One focus style for every focusable element, keyboard focus only. **NEVER** remove it without this replacement.

```text
focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-accent
focus-visible:ring-offset-2 focus-visible:ring-offset-bg
```

Inputs also use `focus:border-accent`.

---

## 4. Component library

Pages **only** compose components from `src/components/ui`. A page **NEVER** styles a raw `<input>`, `<button>`, `<select>` or `<table>` itself.

| Component | Built on | Notes |
| --- | --- | --- |
| `Button`, `IconButton` | native `<button>` | variants and sizes in section 6 |
| `Input`, `Textarea`, `PasswordInput`, `SearchInput` | native elements | wrapped in `Field` |
| `Select`, `Combobox` | Radix UI | keyboard and RTL support |
| `Checkbox`, `Radio`, `Switch` | Radix UI | |
| `DatePicker`, `DateRangePicker`, `TimePicker` | react-day-picker | company timezone |
| `Field` | — | label, hint, error, required mark |
| `Form` | React Hook Form + Zod | section 7 |
| `DataTable` | TanStack Table | section 8 |
| `Modal`, `ConfirmDialog`, `Drawer` | Radix Dialog | focus trap, Esc to close |
| `Toast` | Sonner | |
| `Badge`, `StatusBadge` | — | `StatusBadge status="present" \| "late" \| "absent" \| "leave" \| "holiday" \| "off"` |
| `Card`, `StatCard` | — | dashboard tiles |
| `Tabs`, `DropdownMenu`, `Tooltip` | Radix UI | |
| `EmptyState`, `ErrorState`, `Skeleton`, `Spinner` | — | section 9 |
| `Avatar`, `PageHeader`, `Pagination` | — | |

shadcn/ui is allowed as a starting point, restyled with the tokens above.

### Folder structure

```text
src/
  components/
    ui/            # reusable building blocks only
    layout/        # Sidebar, TopBar, PageShell
  features/
    attendance/    # screens, hooks and API calls per feature
    employees/
    payroll/
    reports/
  lib/
    api.ts         # the one fetch client
    format.ts      # dates, numbers, currency
    i18n/
  styles/
    app.css
```

### Component rules

1. One component per file, named export, file named after the component.
2. Every component accepts `className` and merges it with `cn()` (clsx + tailwind-merge).
3. Variants are defined with `cva`, **NEVER** with ad-hoc `if` statements building class strings.
4. Form controls use `forwardRef` so React Hook Form can focus them.
5. Each `ui` component has a Storybook story with every size, variant and state, in light, dark and RTL.

---

## 5. Inputs

One `Input` API for every text field; label, hint and error always sit in the same place.

**Anatomy (top to bottom):**
1. **Label**: `text-sm font-semibold text-text mb-2`; red `*` when required.
2. **Control**: height from its size, `border border-border rounded-lg bg-surface`, optional left icon or right slot (unit, clear button, eye icon).
3. **Hint or error**: `text-sm mt-1`; the error replaces the hint, never stacks with it.

| State | Border | Background | Other |
| --- | --- | --- | --- |
| Default | `border-border` | `bg-surface` | `text-text`, placeholder `text-text-muted` |
| Hover | slate-400 | `bg-surface` | |
| Focus | `border-accent` | `bg-surface` | standard focus ring |
| Error | `border-status-absent` | `bg-surface` | error text `text-status-absent`, `aria-invalid="true"` |
| Disabled | `border-border` | `bg-surface-hover` | `text-text-muted cursor-not-allowed` |
| Read-only | `border-border` | `bg-surface-hover` | `text-text`, selectable |

```tsx
type InputProps = Omit<React.InputHTMLAttributes<HTMLInputElement>, "size"> & {
  label?: string;
  hint?: string;
  error?: string;
  size?: "sm" | "md" | "lg";      // default "md"
  leftIcon?: React.ReactNode;
  rightSlot?: React.ReactNode;     // "IQD", clear button, eye icon
};

<Input label={t("employees.phone")} hint={t("employees.phoneHint")}
  error={errors.phone?.message} required {...register("phone")} />
```

1. The label is always visible; **NEVER** use the placeholder as the label.
2. `id` comes from `useId()`; `htmlFor` and `aria-describedby` are wired automatically.
3. Correct `type`/`inputMode`: `tel` for phones, `inputMode="numeric"` for IDs and amounts, `email` for email.
4. Money fields show `IQD` as a right slot, accept digits only, display with thousands separators.
5. Width comes from the layout (`w-full`); inputs **NEVER** set a fixed width.
6. Trim whitespace on submit, not while typing.
7. Select, Combobox and date pickers use the same heights, border, states and `Field` wrapper as `Input`.

---

## 6. Buttons

Four variants, three sizes (`sm` / `md` / `lg` = 32 / 40 / 48px), **at most one primary button per screen**.

| Variant | Classes | Use |
| --- | --- | --- |
| `primary` | `bg-primary hover:bg-primary-hover text-on-primary` | the one main action |
| `secondary` (default) | `bg-transparent text-text border border-border hover:bg-surface-hover` | Cancel, Filter, Back |
| `ghost` | `text-text hover:bg-surface-hover` | toolbar and row actions |
| `danger` | `bg-status-absent text-white` | Delete, Deactivate, always behind `ConfirmDialog` |

```tsx
<Button variant="primary" loading={isSubmitting} type="submit">{t("employees.save")}</Button>
<Button variant="secondary" type="button" onClick={onCancel}>{t("common.cancel")}</Button>
<IconButton aria-label={t("employees.edit")} icon={<Pencil />} variant="ghost" />
```

1. Always set `type`: `submit` in forms, `button` everywhere else.
2. `loading` shows a spinner, keeps the width and blocks clicks (prevents double submits).
3. Labels: verb first, sentence case, 1–3 words ("Add employee", not "OK"/"Submit").
4. Icon-only buttons need `aria-label` and a tooltip.
5. Order: primary on the end side (right in LTR, left in RTL), cancel before it.
6. Avoid disabled buttons; keep Save enabled and show validation errors on click.

---

## 7. Forms

Every form uses React Hook Form + a Zod schema; types come from the schema.

```tsx
const employeeSchema = z.object({
  fullName: z.string().trim().min(2, t("errors.nameTooShort")),
  phone: z.string().regex(/^07\d{9}$/, t("errors.phoneInvalid")),
  deviceUserId: z.string().min(1, t("errors.required")),
  salaryAmount: z.coerce.number().int().positive(),
});
type EmployeeForm = z.infer<typeof employeeSchema>;
const form = useForm<EmployeeForm>({ resolver: zodResolver(employeeSchema) });
```

1. Validate on submit first, then on change for fields already showing an error.
2. Focus the first invalid field on submit.
3. Server errors `{ field: ["message"] }` go under their field via `setError`; errors with no field go in a banner at the top.
4. Submit button uses `loading={isSubmitting}`.
5. Warn before leaving a form with unsaved changes (`isDirty`).
6. Layout: one column on mobile, two on desktop for short fields; `gap-4` between fields, `gap-6` between groups, actions at the bottom.
7. Django serializers repeat the validation; the frontend check is for speed only.
8. On success: close or navigate, then a toast ("Employee added").

---

## 8. Tables and lists

Every table is a `DataTable`, paginated on the server.

| Part | Rule |
| --- | --- |
| Header | `text-sm font-semibold text-text-secondary bg-surface-hover`, sticky `z-10` |
| Row height | 48px default, 40px compact (monthly sheet) |
| Row hover / selected | `hover:bg-surface-hover` / `bg-accent-tint` |
| Cell text | `text-sm`, truncate with tooltip |
| Numbers and money | end-aligned, `tabular-nums`, thousands separators, `IQD` in the header only |
| Status | `StatusBadge`, **NEVER** a colored cell |
| Row actions | `ghost` `sm` `IconButton`s or one `DropdownMenu` at the row end |
| Borders | `border-b border-border` between rows; no vertical lines |

1. Server-side pagination, 25 rows default (25 / 50); **NEVER** fetch all rows.
2. Sorting and filters live in the URL query string.
3. Search debounced 300ms, resets to page 1.
4. Virtualize (TanStack Virtual) views over 100 rows, such as the monthly sheet.
5. Loading = skeleton rows at real row height.
6. Empty = `EmptyState` with a next action; filtered empty offers "Clear filters".
7. On phones, tables with more than 4 columns become card lists.
8. Exports start a backend job and show a toast; **NEVER** build files in the browser.

---

## 9. Loading, empty, error states; modals; toasts

Every data screen handles loading, empty, error and success.

| State | Component | Rule |
| --- | --- | --- |
| Loading | `Skeleton` | same layout as the content; no layout jump |
| Empty | `EmptyState` | headline, one line, one action |
| Error | `ErrorState` | plain message + "Try again"; **NEVER** raw error text |
| Background refresh | small `Spinner` in the header, or nothing | **NEVER** blank the screen |

**Modals**
1. `Modal` for short tasks (under ~6 fields); longer tasks get a page or `Drawer`.
2. Widths: `max-w-sm` (confirm), `max-w-xl` (forms), `max-w-3xl` (details); full screen on phones.
3. Close with Esc, close button or backdrop, except forms with unsaved changes (ask first).
4. Focus moves in on open and back to the trigger on close.
5. Destructive actions use `ConfirmDialog` naming the item ("Deactivate Ahmed Karim?").
6. **NEVER** open a modal from inside a modal.

**Toasts**
1. Success: short past tense, no "successfully", 4 seconds.
2. Errors stay until dismissed and say what to do next.
3. Bottom end corner on desktop, top on phones, `z-50`, max 3.
4. **NEVER** use a toast for a field error.

---

## 10. RTL, languages, dates and numbers

Arabic, Kurdish (Sorani), Turkmen and English from day one; Arabic and Kurdish are RTL.

1. Set `<html lang dir>` from the user/company language; switch without reload.
2. **ONLY** logical classes: `ms-* me-* ps-* pe-* start-* end-* text-start text-end`. **NEVER** `ml-* mr-* pl-* pr-* left-* right-* text-left text-right`.
3. Directional icons flip with `rtl:rotate-180`; clock, user, check icons never flip.
4. Phone numbers, IDs, times and amounts get `dir="ltr"` inside RTL text.
5. **NEVER** hardcode user-facing strings: everything goes through i18next (`t("employees.add")`).
6. Leave room for ~30% longer labels; **NEVER** fix button widths.
7. Times arrive from the API in UTC (ISO 8601) and display in the company timezone (`Asia/Baghdad`) via date-fns-tz; **NEVER** the browser timezone.
8. All formatting goes through `lib/format.ts`: date `11/11/2026`, time `08:15` (24h), Western digits 0–9 in every language.
9. Money is whole IQD with separators (`500,000 IQD`), via `Intl.NumberFormat`; **NEVER** decimals, `toFixed` or string concatenation.
10. Durations display as `1h 25m`, not `85`.
11. Week starts on Saturday or the company's first working day.

---

## 11. Accessibility and responsive layout

| Breakpoint | Width | Layout |
| --- | --- | --- |
| default | < 640px | one column, sidebar becomes a sheet menu, tables become cards |
| `sm` | 640px+ | one column, wider cards |
| `md` | 768px+ | two-column forms, `md` controls back to 40px |
| `lg` | 1024px+ | fixed sidebar `w-64`, full tables |
| `xl` | 1280px+ | content `max-w-7xl mx-auto` |

Page padding `px-4 md:px-6 lg:px-8`; card padding `p-4 md:p-6`; sections `gap-8`.

1. Clickable things are `<button>` or `<a>`, **NEVER** `<div onClick>`.
2. Tab order follows visual order; modals trap focus.
3. Every input has a visible label; every icon-only button has `aria-label`.
4. Touch targets at least 44×44px on phones.
5. Respect `prefers-reduced-motion`; animations max 200ms.
6. Charts have a text or table alternative.

---

## 12. Bug prevention

**TypeScript and tooling**
1. `"strict": true` and `"noUncheckedIndexedAccess": true`; **NEVER** `any` or `@ts-ignore` (`@ts-expect-error` with a reason only if unavoidable).
2. ESLint (typescript-eslint, react-hooks, jsx-a11y) + Prettier; CI fails on warnings.
3. Lint rules block `ml-*`/`mr-*`, arbitrary values and raw hex colors.
4. No `console.log` in committed code; errors go to Sentry.
5. `VITE_` env vars never contain secrets.

**API and data**
1. Types are generated from Django's OpenAPI schema (drf-spectacular + openapi-typescript).
2. One client in `lib/api.ts` (CSRF, errors, JSON); components **NEVER** call `fetch` directly.
3. All server data goes through TanStack Query; **NEVER** `useEffect` + `fetch`. Mutations invalidate related queries.
4. Query keys include the company id: `["attendance", companyId, date]`.

| Status | UI behaviour |
| --- | --- |
| 400 | field errors under inputs |
| 401 | clear cache, go to login, return afterwards |
| 402 / subscription expired | renew banner; attendance keeps saving |
| 403 | "You don't have access to this" `ErrorState` |
| 404 | not-found page with a way back |
| 500+ / network | `ErrorState` + "Try again"; one automatic retry |

**Security and runtime**
1. Django session cookie (`HttpOnly`, `Secure`, `SameSite=Lax`) + CSRF; **NEVER** tokens in `localStorage`.
2. Hide what a role can't do; the backend enforces it.
3. **NEVER** `dangerouslySetInnerHTML` with user text.
4. An `ErrorBoundary` wraps every route.
5. List keys are record ids, **NEVER** array indexes.
6. Payroll, social security, tax and money are calculated on the backend only; the frontend **NEVER** recalculates them.

---

## 13. Testing

| Level | Tool | Covers |
| --- | --- | --- |
| Component | Storybook | every size, variant, state; light, dark, RTL |
| Unit | Vitest + Testing Library | `lib/format.ts`, Zod schemas, components with logic |
| End to end | Playwright | login, add employee, today screen, monthly report, payroll, export, subscription expiry |
| Accessibility | Playwright + axe | no violations on key screens |
| Performance | Lighthouse in CI | first load < 2s on 4G, initial JS < ~200 KB gzipped |

---

## Ship checklist (run before finishing any UI task)

- [ ] Built only from `src/components/ui`; no raw styled `<input>`, `<button>`, `<select>`, `<table>`
- [ ] Controls use `sm` / `md` / `lg`; no custom heights
- [ ] Only semantic color classes and Tailwind default spacing/radius/z-index; no hex, palette colors or arbitrary values
- [ ] At most one primary button on the screen
- [ ] Status shown with `StatusBadge` (tint + word), never color alone; terracotta never used for status
- [ ] Loading, empty and error states handled
- [ ] Zod validation; server field errors under the fields; double submit blocked
- [ ] Logical (RTL-safe) classes only; works in Arabic and on a 360px phone
- [ ] Works in dark mode
- [ ] Keyboard only: every action reachable, focus ring visible
- [ ] All text through i18n; dates in company timezone; money as whole IQD
- [ ] Lists paginated or virtualized; search debounced
- [ ] TypeScript, ESLint and tests pass
