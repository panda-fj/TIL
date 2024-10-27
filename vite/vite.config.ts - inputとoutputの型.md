# vite.config.ts - inputとoutput

## input

bundleしたいファイル

```ts
export type InputOption = string | string[] | Record<string, string>;
```

## output

### entryFileNames

`js`ファイルが対象

引数を指定した場合は`PreRenderedChunk`型をとる。

```ts
export interface PreRenderedChunk {
	exports: string[];
	facadeModuleId: string | null;
	isDynamicEntry: boolean;
	isEntry: boolean;
	isImplicitEntry: boolean;
	moduleIds: string[];
	name: string;
	type: 'chunk';
}
```

### assetFileNames

htmlファイル、cssファイル、画像ファイルなど、`js`ファイル以外が対象

引数を指定した場合は`PreRenderedAsset`型をとる

```ts
export interface PreRenderedAsset {
	/** @deprecated Use "names" instead. */
	name: string | undefined;
	names: string[];
	/** @deprecated Use "originalFileNames" instead. */
	originalFileName: string | null;
	originalFileNames: string[];
	source: string | Uint8Array;
	type: 'asset';
}
```