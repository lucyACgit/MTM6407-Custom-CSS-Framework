# CoralKit

**CoralKit** is a small, themeable CSS framework built with Sass and Sass partials. It turns the *Basic Design System* Figma file (coral primary, warm cream secondary, Montserrat type, a 5/10/15/25/40/65 spacing scale) into a ready-to-use theme for standard HTML elements, plus a set of utility classes.

- Styles headings, paragraphs, links, lists, buttons, forms, inputs, checkboxes, radios, tables, tabs, cards, and `<details>`
- Utility classes for color, font weight, font size, margin, padding, border, and more
- Every design value is a Sass variable (and a CSS custom property), so it is easy to customize
- Compiled CSS is included, so no build step is needed to use it

Design source: [Basic Design System (Figma)](https://www.figma.com/design/5EjRlMWwdQ3jIDvRAVaBkS/Basic-Design%C2%A0System--Community-)

## Project structure

```
index.html              Demo page that uses the framework
css/
  framework.css         Compiled CSS (loaded by the browser)
  framework.min.css     Compiled + minified CSS
  custom.css            Your overrides, loaded AFTER framework.css
scss/
  framework.scss        Entry file: loads the partials below
  _tokens.scss          Design tokens: Sass variables, mixins, :root custom properties
  _base.scss            Styles for standard HTML elements (reset, headings, lists, buttons, forms, tables)
  _components.scss      Optional classed pieces (tabs, card, badge, layout, page sections)
  _utilities.scss       Utility classes generated with Sass maps and @each (.u-*)
package.json            Build scripts
```

## Installation

**Option 1: Use the compiled CSS (no tools needed)**

Copy `css/framework.css` (or `framework.min.css`) into your project and link it. The theme uses the Montserrat font, so add it too. Load your own `custom.css` last:

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600;700&display=swap">
<link rel="stylesheet" href="css/framework.css">
<link rel="stylesheet" href="css/custom.css">
```

**Option 2: Build from the Sass source**

```bash
npm ci               # install the exact Sass version from package-lock.json
npm run build        # scss/framework.scss -> css/framework.css and framework.min.css
npm run watch        # rebuild on every save while editing Sass
```

If a change does not show up: check that you saved the right `.scss` file, that the build printed no errors, and that the HTML links to `css/framework.css`.

## Usage

Standard HTML elements are styled automatically; no classes are needed for headings, lists, tables, inputs, etc.

```html
<h1>Heading</h1>
<p>Paragraph with a <a href="#">link</a>.</p>

<button>Primary</button>
<button class="btn-alt">Alternative</button>
<a class="button" href="#">Link styled as a button</a>

<label for="name">Full Name</label>
<input id="name" type="text" placeholder="Jane Doe">

<ul class="tabs">
  <li><a class="active" href="#">Tab One</a></li>
  <li><a href="#">Tab Two</a></li>
</ul>
```

### Components

| Class | Purpose |
|---|---|
| `.btn`, `.button`, `button` | Primary button (hover, focus, disabled included) |
| `.btn-alt`, `.btn-action` | Outlined / blue action button |
| `.btn-sm`, `.btn-lg`, `.btn-block` | Button sizes |
| `.tabs`, `.tabs-vertical` | Horizontal / vertical tabs (`.active` marks the selected tab) |
| `.card`, `.badge`, `.alert` | Content containers |
| `.form-group`, `.form-error`, `.form-hint`, `.is-invalid`, `.check` | Form helpers |
| `.table-striped`, `.table-bordered`, `.table-responsive` | Table variants |
| `.container`, `.row`, `.grid.grid-2`, `.grid-3` | Layout |

### Utility classes (`.u-` prefix)

Utilities do one small job and can be combined on the same element:

```html
<p class="u-weight-bold u-p-3 u-border u-text-brand u-bg-cream">...</p>
```

| Class | What it changes |
|---|---|
| `.u-weight-bold` | `font-weight: 700` |
| `.u-p-3` | `padding: 0.9375rem` (spacing step 3) |
| `.u-border` | `1px solid` border in the theme border color |
| `.u-text-brand` | Text color from `--brand` |
| `.u-bg-cream` | Background color from `--cream` |

| Category | Classes |
|---|---|
| Color | `.u-text-{color}`, `.u-bg-{color}`, `.u-border-{color}`; colors: `brand`, `cream`, `accent`, `grey`, `ink`, `white`, `muted` |
| Font weight | `.u-weight-regular`, `-medium`, `-semibold`, `-bold` |
| Font size | `.u-size-xs`, `-sm`, `-md`, `-lg`, `-xl`, `-2xl`, `-3xl` |
| Margin | `.u-m-{n}`, `.u-mt-`, `.u-mb-`, `.u-ml-`, `.u-mr-`, `.u-mx-`, `.u-my-`, `.u-mx-auto` |
| Padding | `.u-p-{n}`, `.u-pt-`, `.u-pb-`, `.u-pl-`, `.u-pr-`, `.u-px-`, `.u-py-` |
| Border | `.u-border`, `.u-border-t/r/b/l`, `.u-border-0`, `.u-border-2`, `.u-rounded`, `.u-rounded-lg`, `.u-rounded-pill`, `.u-rounded-0` |
| Other | `.u-text-center`, `.u-uppercase`, `.u-flex`, `.u-hidden`, `.u-gap-{n}`, `.u-shadow`, `.u-w-100` |

**Spacing scale:** `{n}` is a *step*, not pixels: `0` = 0, `1` = 0.3125rem (5px), `2` = 0.625rem (10px), `3` = 0.9375rem (15px), `4` = 1.5625rem (25px), `5` = 2.5rem (40px), `6` = 4.0625rem (65px). These match the Figma spacing tokens.

**Specificity:** utilities are loaded last and have the same specificity as component classes, so source order lets them win without `!important`. Base form styles use `:where()` so they never outrank a utility.

## Customization

Tokens exist in two forms: Sass variables (`$brand`, used at compile time) and CSS custom properties (`--brand`, available in the browser).

**1. Override in the browser with `custom.css` (no recompiling)**

```css
/* css/custom.css, loaded after framework.css */
:root {
  --brand: #2a6fdb;
  --brand-hover: #6a9ae5;
  --brand-disabled: #bfd3f4;
  --radius: 8px;
}
```

**2. Override Sass variables.** Every variable is `!default`, so configure it without editing the framework:

```scss
// my-theme.scss
@use "path/to/scss/framework" with (
  $brand: #2a6fdb,
  $cream: #eef4ff,
  $font-family-base: "Inter", sans-serif,
  $radius: 8px
);
```

Compile with `sass my-theme.scss css/my-theme.css`. Hover and disabled colors are derived from `$brand`, so they update automatically.

**3. Edit `scss/_tokens.scss` directly**, then run `npm run build`.

### Main tokens

| Sass variable | CSS property | Default | Description |
|---|---|---|---|
| `$brand` | `--brand` | `#f34e4e` | Main brand color (buttons, accents) |
| `$cream` | `--cream` | `#fff6ea` | Soft background color |
| `$accent` | `--accent` | `#7bb9fa` | Focus, checked, and active-tab color |
| `$grey`, `$ink` | `--grey`, `--ink` | `#d0d0d0`, `#282828` | Borders, text |
| `$font-family-base` | `--font` | Montserrat | Base font |
| `$headings` | | H1 48/60 … H4 18/22 | Heading size / line-height map |
| `$spacers` | `--space-1` … `--space-6` | 5 / 10 / 15 / 25 / 40 / 65 px | Spacing scale (rem) |
| `$radius` | `--radius` | `2px` | Corner radius |
| `$input-height` | | `2.5rem` | Height of buttons and inputs |

## Accessibility and responsive notes

- Links, buttons, `<summary>`, and form fields show a visible focus style (blue outline or ring) for keyboard navigation.
- Headings scale down on phones; long words wrap; wide tables can be wrapped in `.table-responsive`.
- Tested at a phone-width layout, with long unbroken text, and with a second page that overrides `--brand` through `custom.css`.

## Browser support

Current versions of Chrome, Edge, Firefox, and Safari.
