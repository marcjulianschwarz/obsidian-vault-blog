---
blog-title: How to Slim Down a Vite App
blog-published: 2025-10-25
blog-tags:
  - EN
  - TypeScript
  - Vite
---

I recently encountered this warning in one of my Vite apps:

![MagSafe iPhone 12 Hülle](/images/rollup-chunk-warning.png)

This chunking warning will often appear if you add large dependencies to a project. To find out which dependencies are the culprit, analyze the bundle size using the rollup visualizer plugin. It can be configured like this in `vite.config.ts`:

```ts
import { visualizer } from "rollup-plugin-visualizer";

export default defineConfig({
  plugins: [
    react(),
    tailwindcss(),
    visualizer({
      filename: "stats.html",
      emitFile: true,
      template: "treemap",
    }),
  ],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
});
```

After building the project (e.g. by running `pnpm build`) you can open the `stats.html` file from the `dist` folder (e.g. by running `open ./dist/stats.html`).
Depending on the selected `template`, different visualization types will open:

## Treemap

![Rollup Visualizer Treemap](/images/rollup-visualizer-treemap.png)