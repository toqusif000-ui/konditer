# Библиотека кондитера — how to build with this design system

An educational platform for beginner pastry chefs. Audience: women 16–25 and
mothers on parental leave. **90% of traffic is mobile — design mobile-first**,
base styles for phones, `sm:`/`md:`/`lg:` for desktop. Copy is Russian.

Mood: a warm home patisserie. Dusty rose on warm milk, bordeaux headings in an
antiqua serif, soft warm shadows, cards in a pink "mat" (passe-partout).

## 1. Every screen MUST start with a zone wrapper

This is the one mistake that ruins everything silently. Components do **not**
set heading fonts themselves. The serif arrives from an ancestor rule. Wrap all
content in one of two zone classes — this is exactly what the real app does:

```jsx
// Marketing / landing pages — full decor, h1–h3 in the serif
<div className="landing">…</div>

// App: login, LMS, cabinet — quieter, ONLY h1 in the serif
<div className="bk-app">…</div>
```

Without the wrapper you get white background, no milk, and every heading in the
sans stack. It renders, it looks plausible, and it is wrong. Nothing warns you.

`.landing` and `.bk-app` also carry the page background (`--bk-milk`) and the
default text colour, so give the wrapper the page's full height.

## 2. Styling idiom: Tailwind utilities with the `bk-` prefix

This DS is Tailwind CSS v4. Style with utility classes, exactly as the real
codebase does. **Never hardcode a colour** — no `bg-white`, no `text-gray-500`,
no hex. Only the tokens below.

| Family | Utilities (real names — all exist) |
|---|---|
| Surfaces | `bg-bk-milk` #fffdf6 page · `bg-bk-surface` #ffffff cards · `bg-bk-mat` #f1e1e1 passe-partout frame · `bg-bk-tint` #f5e8e8 pink haze · `bg-bk-tint-2` #f1e4de warm haze |
| Accent | `bg-bk-accent` #c58686 dusty rose · `bg-bk-accent-strong` #b37070 hover · `bg-bk-accent-light` #d49494 · `text-bk-on-accent` #fefcf5 text on rose |
| Text | `text-bk-ink` #612525 bordeaux, headings · `text-bk-ink-soft` #8f6563 body · `text-bk-ink-mute` #b09a92 meta |
| Lines | `border-bk-line` · `border-bk-line-strong` (dashed dividers) · `border-bk-tint-line` |
| Fonts | `font-display` / `font-heading` = Cormorant Garamond · `font-sans` / `font-body` = Manrope |

Each colour also exists as `text-`, `border-`, `from-`, `to-`, `ring-`,
`outline-`, and with `hover:` / `focus-visible:` / `group-hover:` states.

**Shadows are the one exception.** They are raw custom properties, not `@theme`
tokens, so **`shadow-bk-card` does not exist**. Reach them only through the
arbitrary value:

```jsx
<div className="shadow-[var(--bk-shadow-card)]">
```
Available: `card`, `card-hover`, `soft`, `lift`, `cta`, `cta-hover`, `pill`,
`mat`, `mat-hover`, `badge`, `white`, `white-hover`. Shadows are warm — tinted
with bordeaux, never grey.

**Stay on the spacing scale.** `gap-4`, `px-6`, `mt-10` — the stylesheet is
precompiled, so an arbitrary value like `gap-[7px]` resolves to **nothing**.
The scale (0–32 plus `0.5`/`1.5`/`2.5`/`3.5`), `sm:`/`md:`/`lg:`/`xl:`
prefixes, and the standard `text-*`, `rounded-*`, `w-*`, `max-w-*`,
`grid-cols-*` families are all available. `var(--bk-*)` also works directly in
`style={{}}` when you need a token outside a utility.

## 3. Signature moves — use these, they are the brand

- **The pill** — `SectionHeading` with `variant="pill"`: the italic accent half
  of a heading sits on a white shadowed card. The landing's most recognisable move.
- **The passe-partout** — `MatCard`: a pink frame around a white card, like a
  photo in an album mat. Use it for content cards on the landing.
- **Serif numerals** — `OrderMark`: section numbers ("01") in the antiqua.
  The order carries meaning (the course runs first question → first cake).
- `Badge` already contains its own `✦` — never repeat it in the label.

## 4. Landing vocabulary vs app vocabulary

Two component families, same tokens, different volume of decor. **Do not mix
them.** The landing primitives (`Section`, `Badge`, `SectionHeading`, `CtaLink`,
`MatCard`, `Tag`) speak sales-page: mandatory accent in the heading,
`text-center`, scroll-reveal, a CTA nailed to 276px. The app primitives
(`Panel`, `Chip`, `PageHeader`, `OrderMark`) are a working environment: quieter
shadows, denser rhythm. `Button`/`Card`/`Input`/`Label` are the ShadCN base and
still carry ShadCN's grey defaults — prefer the `bk-*` primitives outside forms.

## 5. Contrast — where the rose may not go

Rose on milk is 2.9:1, below the WCAG AA 4.5:1 bar. In the app zone it must not
carry a fact: use `text-bk-ink` (11.5:1) or `text-bk-ink-soft` (4.9:1). On pink
haze only bordeaux passes (9.8:1). Known and accepted: white on the rose CTA is
2.87:1 — the owner's call, do not "fix" it by inventing a darker rose.

## 6. Read these before composing

- `styles.css` and its imports — the real, complete truth.
- `guidelines/docs/design-passport.md` — every value, pulled from code and
  checked against production. The best single reference for this brand.
- `components/<group>/<Name>/<Name>.prompt.md` — per-component API and examples.

## 7. A shape that works

```jsx
const { Section, SectionHeading, MatCard, Tag, CtaLink } = window.BkDS;

<div className="landing">
  <Section>
    <SectionHeading title="Что внутри" accent="библиотеки" />
    <div className="mt-8 grid gap-4 sm:grid-cols-2 lg:grid-cols-3">
      <MatCard>
        <div className="flex items-start justify-between gap-3">
          <h3 className="text-lg font-semibold">Кремы и начинки</h3>
          <Tag>9 тем</Tag>
        </div>
        <p className="text-sm leading-relaxed text-bk-ink-soft">
          От базового крем-чиза до стабильных начинок под жару.
        </p>
      </MatCard>
    </div>
    <div className="mt-10 flex justify-center">
      <CtaLink href="#offer">Получить доступ</CtaLink>
    </div>
  </Section>
</div>
```

Library components for the controls; `bk-*` utilities for your own layout glue.

# BkDS (biblioteka-konditera@0.1.0)

This design system is the published biblioteka-konditera React library, bundled as a single
browser global. All 20 components are the real upstream code.

## Where things are

- `_ds_bundle.js` — the whole-DS bundle at the project root; loads every component to `window.BkDS`. First line is a `/* @ds-bundle: … */` metadata header.
- `styles.css` — the single stylesheet entry: it `@import`s the tokens, fonts, and component styles (`_ds_bundle.css`). Link this one file.
- `components/<group>/<Name>/<Name>.prompt.md` (example JSX + variants), `<Name>.d.ts` (types), `<Name>.html` (variant grid).
- `tokens/*.css` — CSS custom properties, names verbatim from upstream.
- `fonts/` — `@font-face` files + `fonts.css` (when the package ships fonts).
- `guidelines/` — the design system's own usage guidance (2 doc(s), see `guidelines/index.md`). Read these before composing larger layouts.

For a specific component, `read_file("components/<group>/<Name>/<Name>.prompt.md")`.

## Loading

Add these two lines to your page once (React must be on the page first):

```html
<link rel="stylesheet" href="styles.css">
<script src="_ds_bundle.js"></script>
```

Components are then available at `window.BkDS.*`. Mount into a dedicated child node (e.g. `<div id="ds-root">`), not the host page's own React root, so the two trees don't collide:

```jsx
const { Badge } = window.BkDS;
ReactDOM.createRoot(document.getElementById('ds-root')).render(<Badge />);
```

## Tokens

258 CSS custom properties from biblioteka-konditera. Names are
preserved verbatim from upstream. They are declared inside `_ds_bundle.css` (this DS ships one compiled stylesheet rather than separate token files).

- **color** (41): `--color-green-600`, `--color-gray-500`, `--color-black`, …
- **spacing** (6): `--tw-space-y-reverse`, `--tw-space-x-reverse`, `--tw-ring-inset`, …
- **typography** (26): `--font-sans`, `--font-mono`, `--font-weight-thin`, …
- **radius** (3): `--radius-xs`, `--radius-md`, `--radius`
- **shadow** (19): `--tw-shadow`, `--tw-ring-shadow`, `--bk-shadow-card`, …
- **other** (163): `--spacing`, `--container-xs`, `--container-sm`, …

## Components

### landing
- `Badge`
- `CtaLink`
- `MatCard`
- `Section` —       .
- `SectionHeading`
- `Tag`

### general
- `Button`
- `Card`
- `CardAction`
- `CardContent`
- `CardDescription`
- `CardFooter`
- `CardHeader`
- `CardTitle`
- `Input`
- `Label`

### lms
- `Chip` —  :  , , ,  .
- `OrderMark` —   / .
- `PageHeader` —  : -, h1, .
- `Panel` —    ( + LMS).
