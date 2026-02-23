# @belocal/js-sdk

On-demand translation SDK for browser and Node.js.

## Install

```bash
npm i @belocal/js-sdk
```

## Usage

### Initialization

```javascript
import { BelocalEngine } from '@belocal/js-sdk';

const engine = new BelocalEngine({ apiKey: 'your-api-key' });
```

### translate

```javascript
const text = await engine.translate('Hello world', 'es');
```

### translateMany

```javascript
const texts = await engine.translateMany(['Hello', 'World'], 'es');
```

### All options

```javascript
const text = await engine.translate('crane', 'ru', {
  source_lang: 'en',
  context: 'website dedicated to animals',
  managed: true,
  ctx: {
    user_type: 'product',
    entity_key: 'birds',
    entity_id: 'crane-001'
  }
});
```

## API Reference

### translate(text, lang, options?)

Translates a single text string.

| Param | Type | Description |
|-------|------|-------------|
| text | string | Text to translate |
| lang | string | Target language code |
| options | [TranslateOptions](#translateoptions) | Optional translation options |

### translateMany(texts, lang, options?)

Translates multiple texts in a single batch.

| Param | Type | Description |
|-------|------|-------------|
| texts | string[] | Texts to translate |
| lang | string | Target language code |
| options | [TranslateOptions](#translateoptions) | Optional translation options |

### TranslateOptions

| Property | Type | Description |
|----------|------|-------------|
| source_lang | string | Source language of the text |
| context | string | Context for better understanding of what is being translated |
| managed | boolean | Use managed translations cache (available on the website in Managed Translations section) |
| ctx | object | User context, see [Ctx Properties](#ctx-properties) below |

#### Ctx Properties

| Property | Description |
|----------|-------------|
| user_type | `product` or `chat`. Correct type improves translation quality |
| entity_key, entity_id | Used together. When passed, a shared context is created across all phrases for the key entity_key + entity_id. This context is used for all translations for such a pair of values and is constantly updated |

## Options

```javascript
const engine = new BelocalEngine({
  apiKey: 'your-api-key',  // required
  batchWindowMs: 50,       // batch window in ms (default: 50)
  timeoutMs: 10000,        // request timeout in ms (default: 10000)
});
```

### batchWindowMs

The SDK batches multiple `translate()` calls made within a time window into a single HTTP request. When you call `translate()`, the request is not sent immediately — instead it is queued. After `batchWindowMs` milliseconds of inactivity, all queued texts are grouped by target language and sent as one batch request to `/v1/translate/multi`. This reduces the number of HTTP round-trips when translating many strings at once (e.g. rendering a page). Default is `50`ms.

## TypeScript

```typescript
import {
  BelocalEngine,
  type BelocalEngineOptions,
  type Lang,
  type TranslateOptions,
  type UserCtx,
  USER_TYPE_PRODUCT,
  USER_TYPE_CHAT
} from '@belocal/js-sdk';
```

## License

MIT
