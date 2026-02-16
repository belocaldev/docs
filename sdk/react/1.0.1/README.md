# @belocal/react

React SDK for Belocal. Components and hooks built on `@belocal/js-sdk`.

## Installation

```bash
npm install @belocal/react
```

## Usage

1. Wrap your app with `BelocalProvider`:

```tsx
import { BelocalProvider } from '@belocal/react';

<BelocalProvider apiKey="your-api-key" defaultLang="en">
  <YourApp />
</BelocalProvider>
```

2. `Translate` — wrapper around `engine.translate()`:

**Minimal**

```tsx
import { Translate } from '@belocal/react';

<Translate text="Hello!" lang="es" />
```

**All params**

```tsx
<Translate
  text="crane"
  lang="ru"             // target language
  sourceLang="en"       // source language
  context="website dedicated to animals"
  managed={true}        // use managed translation
  ctx={{ user_type: 'product', entity_key: 'birds', entity_id: 'crane-001' }}
  onSuccess={(t) => {}} // callback on success
  onError={(e) => {}}   // callback on error
  dots={true}           // animated dots while loading
/>
```
