# @belocal/web

Embeddable BeLocal browser script for websites.

## Usage on a website

Load the script, then call `init` with config:

```html
<script src="https://belocal.ams3.cdn.digitaloceanspaces.com/sdk/web/latest/web.min.js"></script>
<script>
  window.BelocalWeb.init({
    apiKey: "your-public-api-key",
    mode: "watch-data-attribute",
    baseLanguage: "en",
    availableLanguages: ["en", "ru", "de"],
    chosenLanguage: "ru",
    languageSwitcherOptions: {
      showSwitcher: true,
      onLanguageChange: (language) => {
        // optional custom behavior on language change
        window.BelocalWeb.chooseLanguage(language); // default behavior
      },
    },
  });
</script>
```

## BelocalWeb.init parameters

| Parameter | Required | Type | Description |
|-----------|----------|------|-------------|
| `apiKey` | ✓ | string | Public API key for the translation API |
| `mode` | ✓ | `"watch-data-attribute"` | Mode of operation. Currently only `watch-data-attribute` is supported — translates elements with `data-i18n-translate` |
| `baseLanguage` | | string | Base language of the site (default: `"en"`). Used as source language for translation |
| `availableLanguages` | ✓ | string[] | List of languages available for switching (used by the built-in switcher) |
| `chosenLanguage` | ✓ | string | Currently selected language. If differs from `baseLanguage`, triggers translation on init |
| `languageSwitcherOptions` | | object | Options for the built-in language switcher |
| `languageSwitcherOptions.showSwitcher` | | boolean | Show the built-in switcher UI (default: `false` if omitted) |
| `languageSwitcherOptions.onLanguageChange` | | (language: string) => void | Callback when user selects a language in the switcher |
| `batchWindowMs` | | number | Batch window in ms for grouping translate requests (default: `50`) |
| `timeoutMs` | | number | Request timeout in ms (default: `10000`) |
| `debug` | | boolean | Enable debug logging |
| `cacheTtlMs` | | number | TTL for in-memory cache in the browser (ms) |
| `cacheMaxSize` | | number | Max size of in-memory cache in the browser |

## Changing language without the built-in switcher

If you use your own language switcher (buttons, dropdown, links), set `showSwitcher: false` and call `window.BelocalWeb.chooseLanguage(language)` when the user selects a language:

```html
<script src="https://belocal.ams3.cdn.digitaloceanspaces.com/sdk/web/latest/web.min.js"></script>
<script>
  window.BelocalWeb.init({
    apiKey: "your-public-api-key",
    mode: "watch-data-attribute",
    baseLanguage: "en",
    availableLanguages: ["en", "ru", "de"],
    chosenLanguage: "en",
    languageSwitcherOptions: {
      showSwitcher: false,  // no built-in switcher
    },
  });
</script>

<!-- Your custom switcher -->
<nav>
  <button onclick="window.BelocalWeb.chooseLanguage('en')">English</button>
  <button onclick="window.BelocalWeb.chooseLanguage('ru')">Русский</button>
  <button onclick="window.BelocalWeb.chooseLanguage('de')">Deutsch</button>
</nav>
```

Or from JavaScript:

```javascript
// e.g. on click of your custom dropdown option
document.querySelector('#lang-ru').addEventListener('click', () => {
  window.BelocalWeb.chooseLanguage('ru');
});
```

To show a loading state during translation, listen for events:

```javascript
window.addEventListener('belocal-web:translation-start', () => {
  document.body.classList.add('belocal-translating');
});
window.addEventListener('belocal-web:translation-end', () => {
  document.body.classList.remove('belocal-translating');
});
```

## Data attributes on translatable elements

Elements with `data-i18n-translate` support optional attributes:

| Attribute | Description |
|-----------|-------------|
| `data-belocal-entity-key` | Entity key for managed translations |
| `data-belocal-entity-id` | Entity ID for managed translations |
| `data-belocal-context` | Translation context hint |
| `data-belocal-markup` | Markup type passed to context: `html`, `xml`, or `custom` |
| `data-belocal-user-type` | User type (e.g. `product`, `chat`, `review`) for translation context |

Example:

```html
<span data-i18n-translate data-belocal-user-type="product">Product description</span>
```

## Rich text inside translatable elements

Rich text is supported by default for elements marked with `data-i18n-translate`.

The web SDK does not send raw `innerHTML` to the translation API. Instead, it serializes supported child tags into protected placeholders, sends only text plus placeholders for translation, and then reconstructs the DOM after translation.

Supported child tags:

- `strong`, `b`, `em`, `i`, `u`, `small`, `sup`, `sub`
- `span`, `a`, `br`, `img`, `p`, `div`
- `ul`, `ol`, `li`
- `code`, `mark`, `cite`, `q`, `time`

Behavior notes:

- styling and link markup for supported tags is preserved
- `<img>` nodes are preserved in place (alt/title are not translated)
- if placeholders are damaged by the translation response, the SDK falls back to the original DOM for that node instead of rendering broken HTML
- unsupported nested tags are not reconstructed as rich text and may fall back to plain-text translation behavior

Example:

```html
<p data-i18n-translate>
  Hello <strong>world</strong>,
  <a href="/about" class="link">learn more</a>.
</p>
```

Image example:

```html
<div data-i18n-translate>
  New collection
  <img src="/hero.jpg" alt="Summer campaign" />
</div>
```
