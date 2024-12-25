# @sryd/cssinjs

Component level cssinjs solution. It's a subset of [Emotion](https://emotion.sh/) with design token logic wrapper. Please feel free to use emotion directly if you want to find a web cssinjs solution. cssinjs related dep packages:

- stylis
- @emotion/hash
- @emotion/unitless

## Install

```bash
npm install @sryd/cssinjs
```

or

```bash
yarn add @sryd/cssinjs
```

```bash
pnpm add @sryd/cssinjs
```

## Development

```
npm install
npm start
```

## License

@sryd/cssinjs is released under the MIT license.

## API

### StyleProvider

| Prop | Desc | Type | Default |
| --- | --- | --- | --- |
| autoClear | Clear inject style element when component remove. | boolean | false |
| cache | Config cssinjs cache entity. Only set when you need ssr to extract style on you own. | CacheEntity | - |
| hashPriority | Use `:where` selector to reduce hashId css selector priority | `'low' \| 'high'` | `'low'` |
| container | Tell cssinjs where to inject style in. | Element \| ShadowRoot | `document.head` |
| ssrInline | Component wil render inline `<style />` for fallback in SSR. Not recommend. | boolean | false |
| transformers | Transform css before inject in document. Please note that `transformers` do not support dynamic update | Transformer[] | - |

### createCache

return CacheEntity for StyleProvider.

### createTheme

Create theme object. When same algorithm provided, it will return same object.

### Design Token related API

Since `@ant-design/cssinjs` use strong constraints for cache hit performance, we recommend to view demo `basic.tsx` for usage and `animation.tsx` for animation usage.

### extractStyle

Extracts the styles from the cache and returns them as a string.

#### Parameters

- `cache` (Cache): The cache instance containing the styles.
- `options` (object | boolean, optional): Options for extracting the styles.
  - `plain` (boolean, optional): If true, the styles will be returned in plain format. Default is false.
  - `types` (string | string[], optional): The types of styles to extract. Default is ['style', 'token', 'cssVar'].

#### Returns

- (string): The extracted styles as a string.

#### Example

```typescript
import { extractStyle, createCache } from '@ant-design/cssinjs';

// 创建并填充缓存
const cache = createCache();
// 注意：在实际使用中，缓存通常会在渲染组件时自动填充

// 提取样式
const styles = extractStyle(cache, { plain: true, types: ['style', 'token'] });

// 使用提取的样式
const styleElement = document.createElement('style');
styleElement.innerHTML = styles;
document.head.appendChild(styleElement);
```

## Transform

When you need transform CSSObject before inject style. You can use `transformers` to handle this:

```tsx | pure
import {
  legacyLogicalPropertiesTransformer,
  StyleProvider,
} from '@ant-design/cssinjs';

export default () => (
  <StyleProvider transformers={[legacyLogicalPropertiesTransformer]}>
    <MyApp />
  </StyleProvider>
);
```

Follow are the transform we provide:

#### legacyLogicalPropertiesTransformer

Convert logical properties to legacy properties. e.g. `marginBlockStart` to `marginTop`:

- inset
- margin
- padding
- border

#### px2remTransformer

Convert pixel units to rem units. [px2remTransformer.options](./src/transformers/px2rem.ts)
