# @belocal/react

React SDK for Belocal translation service. Provides easy-to-use React components and hooks for translating your application content.

## Installation

```bash
npm install @belocal/react
```

## Quick Start

### 1. Wrap your app with BelocalProvider

```tsx
import { BelocalProvider } from '@belocal/react';

function App() {
  return (
    <BelocalProvider
      apiKey="your-api-key"
      defaultLang="en"
    >
      <YourApp />
    </BelocalProvider>
  );
}
```

### 2. Use the Translate component

```tsx
import { Translate } from '@belocal/react';

function Welcome() {
  return (
    <div>
      <h1>
        <Translate 
          text="Welcome to our app!" 
          fallback="Loading..." 
        />
      </h1>
      
      <p>
        <Translate 
          text="Hello {name}!" 
          context="greeting message"
          lang="es" // Override default language
        />
      </p>
    </div>
  );
}
```

### 3. Use the useBelocal hook

```tsx
import { useBelocal } from '@belocal/react';

function CustomTranslation() {
  const { engine, defaultLang } = useBelocal();
  const [translation, setTranslation] = useState('');

  useEffect(() => {
    engine.t('Hello world!', defaultLang)
      .then(setTranslation)
      .catch(console.error);
  }, [engine, defaultLang]);

  return <span>{translation}</span>;
}
```

## API Reference

### BelocalProvider

The root provider component that initializes the Belocal engine.

#### Props

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| `apiKey` | `string` | ✅ | Your Belocal API key |
| `defaultLang` | `string` | ❌ | Default language for translations |
| `children` | `React.ReactNode` | ✅ | Child components |

### Translate

Component for displaying translated text.

#### Props

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| `text` | `string` | ✅ | Text to translate |
| `lang` | `string` | ❌ | Target language (overrides defaultLang) |
| `source_lang` | `string` | ❌ | Source language of the text |
| `context` | `string` | ❌ | Context description for the translating string (e.g., "sku title", "button label") |
| `fallback` | `string` | ❌ | Text to show while loading |

#### Language Priority

The language selection follows this priority:
1. `lang` prop (highest priority)
2. `defaultLang` from BelocalProvider
3. No translation if neither is provided

### useBelocal

Hook for accessing the Belocal engine and configuration.

#### Returns

| Property | Type | Description |
|----------|------|-------------|
| `engine` | `BelocalEngine` | Belocal engine instance |
| `defaultLang` | `string \| undefined` | Default language from provider |

#### Throws

- Error if used outside of `BelocalProvider`

## Error Handling

- **Network errors**: Component falls back to original text
- **Missing provider**: `useBelocal` throws an error
- **Invalid context**: Console error, falls back to original text

## TypeScript Support

Full TypeScript support with exported types:

```tsx
import type { 
  BelocalProviderProps, 
  TranslateProps 
} from '@belocal/react';
```

## Examples

### Conditional Translation

```tsx
<Translate 
  text="Good morning!"
  lang={userPreferredLanguage}
  fallback="Loading translation..."
/>
```

### Variable Interpolation

```tsx
<Translate 
  text="You have {count} new messages"
  context="notification message"
/>
```

### Manual Translation

```tsx
function ManualTranslation() {
  const { engine } = useBelocal();
  
  const handleTranslate = async () => {
    try {
      const result = await engine.t('Custom text', 'fr');
      console.log(result);
    } catch (error) {
      console.error('Translation failed:', error);
    }
  };

  return <button onClick={handleTranslate}>Translate</button>;
}
```

## License

MIT

## Support

For issues and questions, please visit [GitHub Issues](https://github.com/belocal/sdk/issues).
