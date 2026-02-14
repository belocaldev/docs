# @belocal/js-sdk

Runtime JS SDK for on-demand translation with platform-specific builds.

## Install
```bash
npm i @belocal/js-sdk
```

## Usage

The SDK automatically selects the optimal build for your environment:

```javascript
import { BelocalEngine } from '@belocal/js-sdk';

const engine = new BelocalEngine({
  apiKey: 'your-api-key'
});

const translated = await engine.t('Hello world', 'es');
console.log(translated); // "Hola mundo"
```

### Options

```javascript
const engine = new BelocalEngine({
  apiKey: 'your-api-key',           // Required: Your BeLocal API key
  debug: false                      // Optional: Enable debug logging
});
```

## Automatic Environment Detection

The SDK uses conditional exports to automatically load the optimal build:

- **Browser environments**: Uses `fetch` API with optimized bundle size (~1.3KB)
- **Node.js environments**: Uses HTTP/2 with connection pooling and automatic retries (~4.3KB)
- **CommonJS**: Supported in both environments

## Build Outputs

| File | Environment | Format | Size | Features |
|------|-------------|--------|------|----------|
| `browser.mjs` | Browser | ESM | ~1.3KB | Fetch API, minified |
| `browser.cjs` | Browser | CJS | ~1.3KB | Fetch API, minified |
| `node.mjs` | Node.js | ESM | ~4.3KB | HTTP/2, retries |
| `node.cjs` | Node.js | CJS | ~4.8KB | HTTP/2, retries |

## TypeScript Support

Full TypeScript support with exported types:

```typescript
import { BelocalEngine, type BelocalEngineOptions, type Lang, type KV } from '@belocal/js-sdk';

const options: BelocalEngineOptions = {
  apiKey: 'your-api-key',
  debug: true
};

const engine = new BelocalEngine(options);
```

## Development

### Testing

```bash
# Run all tests
npm test

# Run tests in watch mode
npm run test:watch
```

## License

MIT