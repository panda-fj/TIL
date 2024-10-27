# vite.config.ts - terserの圧縮

```ts
// vite.config.ts
import { defineConfig } from 'vite';

export default defineConfig({
  build: {
    minify: false,  // Terserによる圧縮を無効化
  },
});
```