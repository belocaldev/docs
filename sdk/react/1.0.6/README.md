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
  ctx={{ user_type: 'product', entity_key: 'birds', entity_id: 'crane-001', translation_style: 'formal' }}
  onSuccess={(t) => {}} // callback on success
  onError={(e) => {}}   // callback on error
  dots={true}           // animated dots while loading
/>
```

#### Ctx Properties

| Property | Description |
|----------|-------------|
| user_type | Translation profile: `product` (e-commerce prompts) or `chat` (lighter model + chat history) |
| user_ctx | Free-form user instruction appended to the system prompt |
| cache_type | Cache mode: `temporary` (default), `managed` (persistent), `no-cache` (skip cache) |
| entity_key | Entity type key for context enrichment (e.g. `product`, `category`) |
| entity_id | Entity instance ID (used together with `entity_key`) |
| translation_style | Overrides the project-level translation style. Values: `auto` (no effect), `formal`, `informal`, `technical`, `marketing`, `literary` |
| markup | Markup preservation mode: `html`, `xml`, `custom` |
