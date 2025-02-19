# API Reference <a name="API Reference"></a>

## Constructs <a name="Constructs"></a>

### EsbuildAsset <a name="@mrgrain/cdk-esbuild.EsbuildAsset"></a>

Represents a generic esbuild asset.

You should always use `TypeScriptAsset` or `JavaScriptAsset`.

#### Initializers <a name="@mrgrain/cdk-esbuild.EsbuildAsset.Initializer"></a>

```typescript
import { EsbuildAsset } from '@mrgrain/cdk-esbuild'

new EsbuildAsset(scope: Construct, id: string, props: AssetProps)
```

##### `scope`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildAsset.parameter.scope"></a>

- *Type:* [`constructs.Construct`](#constructs.Construct)

---

##### `id`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildAsset.parameter.id"></a>

- *Type:* `string`

---

##### `props`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildAsset.parameter.props"></a>

- *Type:* [`@mrgrain/cdk-esbuild.AssetProps`](#@mrgrain/cdk-esbuild.AssetProps)

---





### JavaScriptAsset <a name="@mrgrain/cdk-esbuild.JavaScriptAsset"></a>

Bundles the entry points and creates a CDK asset which is uploaded to the bootstrapped CDK S3 bucket during deployment.

The asset can be used by other constructs.

#### Initializers <a name="@mrgrain/cdk-esbuild.JavaScriptAsset.Initializer"></a>

```typescript
import { JavaScriptAsset } from '@mrgrain/cdk-esbuild'

new JavaScriptAsset(scope: Construct, id: string, props: AssetProps)
```

##### `scope`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptAsset.parameter.scope"></a>

- *Type:* [`constructs.Construct`](#constructs.Construct)

---

##### `id`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptAsset.parameter.id"></a>

- *Type:* `string`

---

##### `props`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptAsset.parameter.props"></a>

- *Type:* [`@mrgrain/cdk-esbuild.AssetProps`](#@mrgrain/cdk-esbuild.AssetProps)

---





### TypeScriptAsset <a name="@mrgrain/cdk-esbuild.TypeScriptAsset"></a>

Bundles the entry points and creates a CDK asset which is uploaded to the bootstrapped CDK S3 bucket during deployment.

The asset can be used by other constructs.

#### Initializers <a name="@mrgrain/cdk-esbuild.TypeScriptAsset.Initializer"></a>

```typescript
import { TypeScriptAsset } from '@mrgrain/cdk-esbuild'

new TypeScriptAsset(scope: Construct, id: string, props: AssetProps)
```

##### `scope`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptAsset.parameter.scope"></a>

- *Type:* [`constructs.Construct`](#constructs.Construct)

---

##### `id`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptAsset.parameter.id"></a>

- *Type:* `string`

---

##### `props`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptAsset.parameter.props"></a>

- *Type:* [`@mrgrain/cdk-esbuild.AssetProps`](#@mrgrain/cdk-esbuild.AssetProps)

---





## Structs <a name="Structs"></a>

### AssetProps <a name="@mrgrain/cdk-esbuild.AssetProps"></a>

#### Initializer <a name="[object Object].Initializer"></a>

```typescript
import { AssetProps } from '@mrgrain/cdk-esbuild'

const assetProps: AssetProps = { ... }
```

##### `buildOptions`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.AssetProps.property.buildOptions"></a>

```typescript
public readonly buildOptions: BuildOptions;
```

- *Type:* [`@mrgrain/cdk-esbuild.BuildOptions`](#@mrgrain/cdk-esbuild.BuildOptions)

Build options passed on to esbuild. Please refer to the esbuild Build API docs for details.

* `buildOptions.outdir: string`
The actual path for the output directory is defined by CDK. However setting this option allows to write files into a subdirectory. \
For example `{ outdir: 'js' }` will create an asset with a single directory called `js`, which contains all built files. This approach can be useful for static website deployments, where JavaScript code should be placed into a subdirectory. \
*Cannot be used together with `outfile`*.
* `buildOptions.outfile: string`
Relative path to a file inside the CDK asset output directory.
For example `{ outfile: 'js/index.js' }` will create an asset with a single directory called `js`, which contains a single file `index.js`. This can be useful to rename the entry point. \
*Cannot be used with multiple entryPoints or together with `outdir`.*
* `buildOptions.absWorkingDir: string`
Absolute path to the [esbuild working directory](https://esbuild.github.io/api/#working-directory) and defaults to the [current working directory](https://en.wikipedia.org/wiki/Working_directory). \
If paths cannot be found, a good starting point is to look at the concatenation of `absWorkingDir + entryPoint`. It must always be a valid absolute path pointing to the entry point. When needed, the probably easiest way to set absWorkingDir is to use a combination of `resolve` and `__dirname` (see "Library authors" section in the documentation).

> https://esbuild.github.io/api/#build-api

---

##### `buildProvider`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.AssetProps.property.buildProvider"></a>

```typescript
public readonly buildProvider: IBuildProvider;
```

- *Type:* [`@mrgrain/cdk-esbuild.IBuildProvider`](#@mrgrain/cdk-esbuild.IBuildProvider)
- *Default:* new EsbuildProvider()

The esbuild Build API implementation to be used.

Configure the default `EsbuildProvider` for more options or
provide a custom `IBuildProvider` as an escape hatch.

---

##### `copyDir`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.AssetProps.property.copyDir"></a>

```typescript
public readonly copyDir: string | string[] | {[ key: string ]: string | string[]};
```

- *Type:* `string` | `string`[] | {[ key: string ]: `string` | `string`[]}

Copy additional files to the code [asset staging directory](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.AssetStaging.html#absolutestagedpath), before the build runs. Files copied like this will be overwritten by esbuild if they share the same name as any of the outputs.

* When provided with a `string` or `array`, all files are copied to the root of asset staging directory.
* When given a `map`, the key indicates the destination relative to the asset staging directory and the value is a list of all sources to be copied.

Therefore the following values for `copyDir` are all equivalent:
```
{ copyDir: "path/to/source" }
{ copyDir: ["path/to/source"] }
{ copyDir: { ".": "path/to/source" } }
{ copyDir: { ".": ["path/to/source"] } }
```
The destination cannot be outside of the asset staging directory.
If you are receiving the error "Cannot copy files to outside of the asset staging directory."
you are likely using `..` or an absolute path as key on the `copyDir` map.
Instead use only relative paths and avoid `..`.

---

##### `entryPoints`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.AssetProps.property.entryPoints"></a>

```typescript
public readonly entryPoints: string | string[] | {[ key: string ]: string};
```

- *Type:* `string` | `string`[] | {[ key: string ]: `string`}

A path or list or map of paths to the entry points of your code.

Relative paths are by default resolved from the current working directory.
To change the working directory, see `buildOptions.absWorkingDir`.

Absolute paths can be used if files are part of the working directory.

Examples:
  - `'src/index.ts'`
  - `require.resolve('./lambda')`
  - `['src/index.ts', 'src/util.ts']`
  - `{one: 'src/two.ts', two: 'src/one.ts'}`

---

##### `assetHash`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.AssetProps.property.assetHash"></a>

```typescript
public readonly assetHash: string;
```

- *Type:* `string`

A hash of this asset, which is available at construction time.

As this is a plain string, it can be used in construct IDs in order to enforce creation of a new resource when the content hash has changed.

Defaults to a hash of all files in the resulting bundle.

---

### BuildOptions <a name="@mrgrain/cdk-esbuild.BuildOptions"></a>

#### Initializer <a name="[object Object].Initializer"></a>

```typescript
import { BuildOptions } from '@mrgrain/cdk-esbuild'

const buildOptions: BuildOptions = { ... }
```

##### `absWorkingDir`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.absWorkingDir"></a>

```typescript
public readonly absWorkingDir: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#working-directory.

---

##### `alias`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.alias"></a>

```typescript
public readonly alias: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#alias.

---

##### `allowOverwrite`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.allowOverwrite"></a>

```typescript
public readonly allowOverwrite: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#allow-overwrite.

---

##### `assetNames`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.assetNames"></a>

```typescript
public readonly assetNames: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#asset-names.

---

##### `banner`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.banner"></a>

```typescript
public readonly banner: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#banner.

---

##### `bundle`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.bundle"></a>

```typescript
public readonly bundle: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#bundle.

---

##### `charset`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.charset"></a>

```typescript
public readonly charset: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#charset.

---

##### `chunkNames`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.chunkNames"></a>

```typescript
public readonly chunkNames: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#chunk-names.

---

##### `color`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.color"></a>

```typescript
public readonly color: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#color.

---

##### `conditions`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.conditions"></a>

```typescript
public readonly conditions: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#conditions.

---

##### `define`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.define"></a>

```typescript
public readonly define: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#define.

---

##### `drop`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.drop"></a>

```typescript
public readonly drop: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#drop.

---

##### `entryNames`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.entryNames"></a>

```typescript
public readonly entryNames: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#entry-names.

---

##### `external`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.external"></a>

```typescript
public readonly external: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#external.

---

##### `footer`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.footer"></a>

```typescript
public readonly footer: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#footer.

---

##### `format`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.format"></a>

```typescript
public readonly format: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#format.

---

##### `globalName`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.globalName"></a>

```typescript
public readonly globalName: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#global-name.

---

##### `ignoreAnnotations`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.ignoreAnnotations"></a>

```typescript
public readonly ignoreAnnotations: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#ignore-annotations.

---

##### `inject`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.inject"></a>

```typescript
public readonly inject: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#inject.

---

##### `jsx`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.jsx"></a>

```typescript
public readonly jsx: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx.

---

##### `jsxDev`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.jsxDev"></a>

```typescript
public readonly jsxDev: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#jsx-development.

---

##### `jsxFactory`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.jsxFactory"></a>

```typescript
public readonly jsxFactory: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx-factory.

---

##### `jsxFragment`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.jsxFragment"></a>

```typescript
public readonly jsxFragment: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx-fragment.

---

##### `jsxImportSource`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.jsxImportSource"></a>

```typescript
public readonly jsxImportSource: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx-import-source.

---

##### `jsxSideEffects`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.jsxSideEffects"></a>

```typescript
public readonly jsxSideEffects: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#jsx-side-effects.

---

##### `keepNames`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.keepNames"></a>

```typescript
public readonly keepNames: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#keep-names.

---

##### `legalComments`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.legalComments"></a>

```typescript
public readonly legalComments: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#legal-comments.

---

##### `loader`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.loader"></a>

```typescript
public readonly loader: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#loader.

---

##### `logLevel`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.logLevel"></a>

```typescript
public readonly logLevel: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#log-level.

---

##### `logLimit`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.logLimit"></a>

```typescript
public readonly logLimit: number;
```

- *Type:* `number`

Documentation: https://esbuild.github.io/api/#log-limit.

---

##### `logOverride`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.logOverride"></a>

```typescript
public readonly logOverride: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#log-override.

---

##### `mainFields`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.mainFields"></a>

```typescript
public readonly mainFields: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#main-fields.

---

##### `mangleCache`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.mangleCache"></a>

```typescript
public readonly mangleCache: {[ key: string ]: string | boolean};
```

- *Type:* {[ key: string ]: `string` | `boolean`}

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `mangleProps`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.mangleProps"></a>

```typescript
public readonly mangleProps: any;
```

- *Type:* `any`

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `mangleQuoted`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.mangleQuoted"></a>

```typescript
public readonly mangleQuoted: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `metafile`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.metafile"></a>

```typescript
public readonly metafile: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#metafile.

---

##### `minify`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.minify"></a>

```typescript
public readonly minify: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `minifyIdentifiers`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.minifyIdentifiers"></a>

```typescript
public readonly minifyIdentifiers: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `minifySyntax`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.minifySyntax"></a>

```typescript
public readonly minifySyntax: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `minifyWhitespace`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.minifyWhitespace"></a>

```typescript
public readonly minifyWhitespace: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `nodePaths`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.nodePaths"></a>

```typescript
public readonly nodePaths: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#node-paths.

---

##### `outbase`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.outbase"></a>

```typescript
public readonly outbase: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#outbase.

---

##### `outdir`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.outdir"></a>

```typescript
public readonly outdir: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#outdir.

---

##### `outExtension`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.outExtension"></a>

```typescript
public readonly outExtension: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#out-extension.

---

##### `outfile`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.outfile"></a>

```typescript
public readonly outfile: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#outfile.

---

##### `packages`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.packages"></a>

```typescript
public readonly packages: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#packages.

---

##### `platform`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.platform"></a>

```typescript
public readonly platform: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#platform.

---

##### `preserveSymlinks`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.preserveSymlinks"></a>

```typescript
public readonly preserveSymlinks: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#preserve-symlinks.

---

##### `publicPath`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.publicPath"></a>

```typescript
public readonly publicPath: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#public-path.

---

##### `pure`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.pure"></a>

```typescript
public readonly pure: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#pure.

---

##### `reserveProps`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.reserveProps"></a>

```typescript
public readonly reserveProps: any;
```

- *Type:* `any`

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `resolveExtensions`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.resolveExtensions"></a>

```typescript
public readonly resolveExtensions: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#resolve-extensions.

---

##### `sourcemap`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.sourcemap"></a>

```typescript
public readonly sourcemap: boolean | string;
```

- *Type:* `boolean` | `string`

Documentation: https://esbuild.github.io/api/#sourcemap.

---

##### `sourceRoot`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.sourceRoot"></a>

```typescript
public readonly sourceRoot: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#source-root.

---

##### `sourcesContent`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.sourcesContent"></a>

```typescript
public readonly sourcesContent: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#sources-content.

---

##### `splitting`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.splitting"></a>

```typescript
public readonly splitting: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#splitting.

---

##### `supported`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.supported"></a>

```typescript
public readonly supported: {[ key: string ]: boolean};
```

- *Type:* {[ key: string ]: `boolean`}

Documentation: https://esbuild.github.io/api/#supported.

---

##### `target`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.target"></a>

```typescript
public readonly target: string | string[];
```

- *Type:* `string` | `string`[]

Documentation: https://esbuild.github.io/api/#target.

---

##### `treeShaking`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.treeShaking"></a>

```typescript
public readonly treeShaking: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#tree-shaking.

---

##### `tsconfig`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.tsconfig"></a>

```typescript
public readonly tsconfig: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#tsconfig.

---

##### `write`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BuildOptions.property.write"></a>

```typescript
public readonly write: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#write.

---

### BundlerProps <a name="@mrgrain/cdk-esbuild.BundlerProps"></a>

#### Initializer <a name="[object Object].Initializer"></a>

```typescript
import { BundlerProps } from '@mrgrain/cdk-esbuild'

const bundlerProps: BundlerProps = { ... }
```

##### `buildOptions`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BundlerProps.property.buildOptions"></a>

```typescript
public readonly buildOptions: BuildOptions;
```

- *Type:* [`@mrgrain/cdk-esbuild.BuildOptions`](#@mrgrain/cdk-esbuild.BuildOptions)

Build options passed on to esbuild. Please refer to the esbuild Build API docs for details.

* `buildOptions.outdir: string`
The actual path for the output directory is defined by CDK. However setting this option allows to write files into a subdirectory. \
For example `{ outdir: 'js' }` will create an asset with a single directory called `js`, which contains all built files. This approach can be useful for static website deployments, where JavaScript code should be placed into a subdirectory. \
*Cannot be used together with `outfile`*.
* `buildOptions.outfile: string`
Relative path to a file inside the CDK asset output directory.
For example `{ outfile: 'js/index.js' }` will create an asset with a single directory called `js`, which contains a single file `index.js`. This can be useful to rename the entry point. \
*Cannot be used with multiple entryPoints or together with `outdir`.*
* `buildOptions.absWorkingDir: string`
Absolute path to the [esbuild working directory](https://esbuild.github.io/api/#working-directory) and defaults to the [current working directory](https://en.wikipedia.org/wiki/Working_directory). \
If paths cannot be found, a good starting point is to look at the concatenation of `absWorkingDir + entryPoint`. It must always be a valid absolute path pointing to the entry point. When needed, the probably easiest way to set absWorkingDir is to use a combination of `resolve` and `__dirname` (see "Library authors" section in the documentation).

> https://esbuild.github.io/api/#build-api

---

##### `buildProvider`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BundlerProps.property.buildProvider"></a>

```typescript
public readonly buildProvider: IBuildProvider;
```

- *Type:* [`@mrgrain/cdk-esbuild.IBuildProvider`](#@mrgrain/cdk-esbuild.IBuildProvider)
- *Default:* new EsbuildProvider()

The esbuild Build API implementation to be used.

Configure the default `EsbuildProvider` for more options or
provide a custom `IBuildProvider` as an escape hatch.

---

##### `copyDir`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.BundlerProps.property.copyDir"></a>

```typescript
public readonly copyDir: string | string[] | {[ key: string ]: string | string[]};
```

- *Type:* `string` | `string`[] | {[ key: string ]: `string` | `string`[]}

Copy additional files to the code [asset staging directory](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.AssetStaging.html#absolutestagedpath), before the build runs. Files copied like this will be overwritten by esbuild if they share the same name as any of the outputs.

* When provided with a `string` or `array`, all files are copied to the root of asset staging directory.
* When given a `map`, the key indicates the destination relative to the asset staging directory and the value is a list of all sources to be copied.

Therefore the following values for `copyDir` are all equivalent:
```
{ copyDir: "path/to/source" }
{ copyDir: ["path/to/source"] }
{ copyDir: { ".": "path/to/source" } }
{ copyDir: { ".": ["path/to/source"] } }
```
The destination cannot be outside of the asset staging directory.
If you are receiving the error "Cannot copy files to outside of the asset staging directory."
you are likely using `..` or an absolute path as key on the `copyDir` map.
Instead use only relative paths and avoid `..`.

---

### CodeConfig <a name="@mrgrain/cdk-esbuild.CodeConfig"></a>

Result of binding `Code` into a `Function`.

#### Initializer <a name="[object Object].Initializer"></a>

```typescript
import { CodeConfig } from '@mrgrain/cdk-esbuild'

const codeConfig: CodeConfig = { ... }
```

##### `image`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.CodeConfig.property.image"></a>

```typescript
public readonly image: CodeImageConfig;
```

- *Type:* [`aws-cdk-lib.aws_lambda.CodeImageConfig`](#aws-cdk-lib.aws_lambda.CodeImageConfig)
- *Default:* code is not an ECR container image

Docker image configuration (mutually exclusive with `s3Location` and `inlineCode`).

---

##### `inlineCode`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.CodeConfig.property.inlineCode"></a>

```typescript
public readonly inlineCode: string;
```

- *Type:* `string`
- *Default:* code is not inline code

Inline code (mutually exclusive with `s3Location` and `image`).

---

##### `s3Location`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.CodeConfig.property.s3Location"></a>

```typescript
public readonly s3Location: Location;
```

- *Type:* [`aws-cdk-lib.aws_s3.Location`](#aws-cdk-lib.aws_s3.Location)
- *Default:* code is not an s3 location

The location of the code in S3 (mutually exclusive with `inlineCode` and `image`).

---

### CompilerOptions <a name="@mrgrain/cdk-esbuild.CompilerOptions"></a>

#### Initializer <a name="[object Object].Initializer"></a>

```typescript
import { CompilerOptions } from '@mrgrain/cdk-esbuild'

const compilerOptions: CompilerOptions = { ... }
```

##### `importsNotUsedAsValues`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.CompilerOptions.property.importsNotUsedAsValues"></a>

```typescript
public readonly importsNotUsedAsValues: string;
```

- *Type:* `string`

---

##### `jsxFactory`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.CompilerOptions.property.jsxFactory"></a>

```typescript
public readonly jsxFactory: string;
```

- *Type:* `string`

---

##### `jsxFragmentFactory`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.CompilerOptions.property.jsxFragmentFactory"></a>

```typescript
public readonly jsxFragmentFactory: string;
```

- *Type:* `string`

---

##### `preserveValueImports`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.CompilerOptions.property.preserveValueImports"></a>

```typescript
public readonly preserveValueImports: boolean;
```

- *Type:* `boolean`

---

##### `useDefineForClassFields`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.CompilerOptions.property.useDefineForClassFields"></a>

```typescript
public readonly useDefineForClassFields: boolean;
```

- *Type:* `boolean`

---

### EsbuildProviderProps <a name="@mrgrain/cdk-esbuild.EsbuildProviderProps"></a>

Configure the default EsbuildProvider.

#### Initializer <a name="[object Object].Initializer"></a>

```typescript
import { EsbuildProviderProps } from '@mrgrain/cdk-esbuild'

const esbuildProviderProps: EsbuildProviderProps = { ... }
```

##### `esbuildBinaryPath`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.EsbuildProviderProps.property.esbuildBinaryPath"></a>

```typescript
public readonly esbuildBinaryPath: string;
```

- *Type:* `string`

Path to the binary used by esbuild.

This is the same as setting the ESBUILD_BINARY_PATH environment variable.

---

##### `esbuildModulePath`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.EsbuildProviderProps.property.esbuildModulePath"></a>

```typescript
public readonly esbuildModulePath: string;
```

- *Type:* `string`
- *Default:* `CDK_ESBUILD_MODULE_PATH` or package resolution (see description)

Absolute path to the esbuild module JS file.

E.g. "/home/user/.npm/node_modules/esbuild/lib/main.js"

If not set, the module path will be determined in the following order:

- Use a path from the `CDK_ESBUILD_MODULE_PATH` environment variable
- In TypeScript, fallback to the default Node.js package resolution mechanism
- All other languages (Python, Go, .NET, Java) use an automatic "best effort" resolution mechanism. \
   The exact algorithm of this mechanism is considered an implementation detail and should not be relied on.
   If `esbuild` cannot be found, it might be installed dynamically to a temporary location.
   To opt-out of this behavior, set either `esbuildModulePath` or `CDK_ESBUILD_MODULE_PATH` env variable.

Use the static methods on `EsbuildSource` to customize the default behavior.

---

### JavaScriptCodeProps <a name="@mrgrain/cdk-esbuild.JavaScriptCodeProps"></a>

#### Initializer <a name="[object Object].Initializer"></a>

```typescript
import { JavaScriptCodeProps } from '@mrgrain/cdk-esbuild'

const javaScriptCodeProps: JavaScriptCodeProps = { ... }
```

##### `buildOptions`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptCodeProps.property.buildOptions"></a>

```typescript
public readonly buildOptions: BuildOptions;
```

- *Type:* [`@mrgrain/cdk-esbuild.BuildOptions`](#@mrgrain/cdk-esbuild.BuildOptions)

Build options passed on to esbuild. Please refer to the esbuild Build API docs for details.

* `buildOptions.outdir: string`
The actual path for the output directory is defined by CDK. However setting this option allows to write files into a subdirectory. \
For example `{ outdir: 'js' }` will create an asset with a single directory called `js`, which contains all built files. This approach can be useful for static website deployments, where JavaScript code should be placed into a subdirectory. \
*Cannot be used together with `outfile`*.
* `buildOptions.outfile: string`
Relative path to a file inside the CDK asset output directory.
For example `{ outfile: 'js/index.js' }` will create an asset with a single directory called `js`, which contains a single file `index.js`. This can be useful to rename the entry point. \
*Cannot be used with multiple entryPoints or together with `outdir`.*
* `buildOptions.absWorkingDir: string`
Absolute path to the [esbuild working directory](https://esbuild.github.io/api/#working-directory) and defaults to the [current working directory](https://en.wikipedia.org/wiki/Working_directory). \
If paths cannot be found, a good starting point is to look at the concatenation of `absWorkingDir + entryPoint`. It must always be a valid absolute path pointing to the entry point. When needed, the probably easiest way to set absWorkingDir is to use a combination of `resolve` and `__dirname` (see "Library authors" section in the documentation).

> https://esbuild.github.io/api/#build-api

---

##### `buildProvider`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptCodeProps.property.buildProvider"></a>

```typescript
public readonly buildProvider: IBuildProvider;
```

- *Type:* [`@mrgrain/cdk-esbuild.IBuildProvider`](#@mrgrain/cdk-esbuild.IBuildProvider)
- *Default:* new EsbuildProvider()

The esbuild Build API implementation to be used.

Configure the default `EsbuildProvider` for more options or
provide a custom `IBuildProvider` as an escape hatch.

---

##### `copyDir`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptCodeProps.property.copyDir"></a>

```typescript
public readonly copyDir: string | string[] | {[ key: string ]: string | string[]};
```

- *Type:* `string` | `string`[] | {[ key: string ]: `string` | `string`[]}

Copy additional files to the code [asset staging directory](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.AssetStaging.html#absolutestagedpath), before the build runs. Files copied like this will be overwritten by esbuild if they share the same name as any of the outputs.

* When provided with a `string` or `array`, all files are copied to the root of asset staging directory.
* When given a `map`, the key indicates the destination relative to the asset staging directory and the value is a list of all sources to be copied.

Therefore the following values for `copyDir` are all equivalent:
```
{ copyDir: "path/to/source" }
{ copyDir: ["path/to/source"] }
{ copyDir: { ".": "path/to/source" } }
{ copyDir: { ".": ["path/to/source"] } }
```
The destination cannot be outside of the asset staging directory.
If you are receiving the error "Cannot copy files to outside of the asset staging directory."
you are likely using `..` or an absolute path as key on the `copyDir` map.
Instead use only relative paths and avoid `..`.

---

##### `assetHash`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptCodeProps.property.assetHash"></a>

```typescript
public readonly assetHash: string;
```

- *Type:* `string`

A hash of this asset, which is available at construction time.

As this is a plain string, it can be used in construct IDs in order to enforce creation of a new resource when the content hash has changed.

Defaults to a hash of all files in the resulting bundle.

---

### JavaScriptSourceProps <a name="@mrgrain/cdk-esbuild.JavaScriptSourceProps"></a>

#### Initializer <a name="[object Object].Initializer"></a>

```typescript
import { JavaScriptSourceProps } from '@mrgrain/cdk-esbuild'

const javaScriptSourceProps: JavaScriptSourceProps = { ... }
```

##### `buildOptions`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptSourceProps.property.buildOptions"></a>

```typescript
public readonly buildOptions: BuildOptions;
```

- *Type:* [`@mrgrain/cdk-esbuild.BuildOptions`](#@mrgrain/cdk-esbuild.BuildOptions)

Build options passed on to esbuild. Please refer to the esbuild Build API docs for details.

* `buildOptions.outdir: string`
The actual path for the output directory is defined by CDK. However setting this option allows to write files into a subdirectory. \
For example `{ outdir: 'js' }` will create an asset with a single directory called `js`, which contains all built files. This approach can be useful for static website deployments, where JavaScript code should be placed into a subdirectory. \
*Cannot be used together with `outfile`*.
* `buildOptions.outfile: string`
Relative path to a file inside the CDK asset output directory.
For example `{ outfile: 'js/index.js' }` will create an asset with a single directory called `js`, which contains a single file `index.js`. This can be useful to rename the entry point. \
*Cannot be used with multiple entryPoints or together with `outdir`.*
* `buildOptions.absWorkingDir: string`
Absolute path to the [esbuild working directory](https://esbuild.github.io/api/#working-directory) and defaults to the [current working directory](https://en.wikipedia.org/wiki/Working_directory). \
If paths cannot be found, a good starting point is to look at the concatenation of `absWorkingDir + entryPoint`. It must always be a valid absolute path pointing to the entry point. When needed, the probably easiest way to set absWorkingDir is to use a combination of `resolve` and `__dirname` (see "Library authors" section in the documentation).

> https://esbuild.github.io/api/#build-api

---

##### `buildProvider`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptSourceProps.property.buildProvider"></a>

```typescript
public readonly buildProvider: IBuildProvider;
```

- *Type:* [`@mrgrain/cdk-esbuild.IBuildProvider`](#@mrgrain/cdk-esbuild.IBuildProvider)
- *Default:* new EsbuildProvider()

The esbuild Build API implementation to be used.

Configure the default `EsbuildProvider` for more options or
provide a custom `IBuildProvider` as an escape hatch.

---

##### `copyDir`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptSourceProps.property.copyDir"></a>

```typescript
public readonly copyDir: string | string[] | {[ key: string ]: string | string[]};
```

- *Type:* `string` | `string`[] | {[ key: string ]: `string` | `string`[]}

Copy additional files to the code [asset staging directory](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.AssetStaging.html#absolutestagedpath), before the build runs. Files copied like this will be overwritten by esbuild if they share the same name as any of the outputs.

* When provided with a `string` or `array`, all files are copied to the root of asset staging directory.
* When given a `map`, the key indicates the destination relative to the asset staging directory and the value is a list of all sources to be copied.

Therefore the following values for `copyDir` are all equivalent:
```
{ copyDir: "path/to/source" }
{ copyDir: ["path/to/source"] }
{ copyDir: { ".": "path/to/source" } }
{ copyDir: { ".": ["path/to/source"] } }
```
The destination cannot be outside of the asset staging directory.
If you are receiving the error "Cannot copy files to outside of the asset staging directory."
you are likely using `..` or an absolute path as key on the `copyDir` map.
Instead use only relative paths and avoid `..`.

---

##### `assetHash`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptSourceProps.property.assetHash"></a>

```typescript
public readonly assetHash: string;
```

- *Type:* `string`

A hash of this asset, which is available at construction time.

As this is a plain string, it can be used in construct IDs in order to enforce creation of a new resource when the content hash has changed.

Defaults to a hash of all files in the resulting bundle.

---

### ProviderBuildOptions <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions"></a>

#### Initializer <a name="[object Object].Initializer"></a>

```typescript
import { ProviderBuildOptions } from '@mrgrain/cdk-esbuild'

const providerBuildOptions: ProviderBuildOptions = { ... }
```

##### `absWorkingDir`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.absWorkingDir"></a>

```typescript
public readonly absWorkingDir: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#working-directory.

---

##### `alias`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.alias"></a>

```typescript
public readonly alias: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#alias.

---

##### `allowOverwrite`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.allowOverwrite"></a>

```typescript
public readonly allowOverwrite: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#allow-overwrite.

---

##### `assetNames`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.assetNames"></a>

```typescript
public readonly assetNames: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#asset-names.

---

##### `banner`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.banner"></a>

```typescript
public readonly banner: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#banner.

---

##### `bundle`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.bundle"></a>

```typescript
public readonly bundle: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#bundle.

---

##### `charset`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.charset"></a>

```typescript
public readonly charset: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#charset.

---

##### `chunkNames`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.chunkNames"></a>

```typescript
public readonly chunkNames: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#chunk-names.

---

##### `color`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.color"></a>

```typescript
public readonly color: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#color.

---

##### `conditions`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.conditions"></a>

```typescript
public readonly conditions: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#conditions.

---

##### `define`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.define"></a>

```typescript
public readonly define: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#define.

---

##### `drop`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.drop"></a>

```typescript
public readonly drop: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#drop.

---

##### `entryNames`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.entryNames"></a>

```typescript
public readonly entryNames: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#entry-names.

---

##### `external`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.external"></a>

```typescript
public readonly external: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#external.

---

##### `footer`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.footer"></a>

```typescript
public readonly footer: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#footer.

---

##### `format`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.format"></a>

```typescript
public readonly format: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#format.

---

##### `globalName`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.globalName"></a>

```typescript
public readonly globalName: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#global-name.

---

##### `ignoreAnnotations`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.ignoreAnnotations"></a>

```typescript
public readonly ignoreAnnotations: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#ignore-annotations.

---

##### `inject`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.inject"></a>

```typescript
public readonly inject: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#inject.

---

##### `jsx`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.jsx"></a>

```typescript
public readonly jsx: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx.

---

##### `jsxDev`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.jsxDev"></a>

```typescript
public readonly jsxDev: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#jsx-development.

---

##### `jsxFactory`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.jsxFactory"></a>

```typescript
public readonly jsxFactory: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx-factory.

---

##### `jsxFragment`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.jsxFragment"></a>

```typescript
public readonly jsxFragment: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx-fragment.

---

##### `jsxImportSource`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.jsxImportSource"></a>

```typescript
public readonly jsxImportSource: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx-import-source.

---

##### `jsxSideEffects`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.jsxSideEffects"></a>

```typescript
public readonly jsxSideEffects: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#jsx-side-effects.

---

##### `keepNames`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.keepNames"></a>

```typescript
public readonly keepNames: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#keep-names.

---

##### `legalComments`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.legalComments"></a>

```typescript
public readonly legalComments: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#legal-comments.

---

##### `loader`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.loader"></a>

```typescript
public readonly loader: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#loader.

---

##### `logLevel`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.logLevel"></a>

```typescript
public readonly logLevel: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#log-level.

---

##### `logLimit`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.logLimit"></a>

```typescript
public readonly logLimit: number;
```

- *Type:* `number`

Documentation: https://esbuild.github.io/api/#log-limit.

---

##### `logOverride`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.logOverride"></a>

```typescript
public readonly logOverride: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#log-override.

---

##### `mainFields`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.mainFields"></a>

```typescript
public readonly mainFields: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#main-fields.

---

##### `mangleCache`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.mangleCache"></a>

```typescript
public readonly mangleCache: {[ key: string ]: string | boolean};
```

- *Type:* {[ key: string ]: `string` | `boolean`}

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `mangleProps`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.mangleProps"></a>

```typescript
public readonly mangleProps: any;
```

- *Type:* `any`

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `mangleQuoted`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.mangleQuoted"></a>

```typescript
public readonly mangleQuoted: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `metafile`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.metafile"></a>

```typescript
public readonly metafile: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#metafile.

---

##### `minify`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.minify"></a>

```typescript
public readonly minify: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `minifyIdentifiers`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.minifyIdentifiers"></a>

```typescript
public readonly minifyIdentifiers: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `minifySyntax`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.minifySyntax"></a>

```typescript
public readonly minifySyntax: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `minifyWhitespace`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.minifyWhitespace"></a>

```typescript
public readonly minifyWhitespace: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `nodePaths`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.nodePaths"></a>

```typescript
public readonly nodePaths: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#node-paths.

---

##### `outbase`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.outbase"></a>

```typescript
public readonly outbase: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#outbase.

---

##### `outdir`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.outdir"></a>

```typescript
public readonly outdir: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#outdir.

---

##### `outExtension`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.outExtension"></a>

```typescript
public readonly outExtension: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#out-extension.

---

##### `outfile`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.outfile"></a>

```typescript
public readonly outfile: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#outfile.

---

##### `packages`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.packages"></a>

```typescript
public readonly packages: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#packages.

---

##### `platform`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.platform"></a>

```typescript
public readonly platform: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#platform.

---

##### `preserveSymlinks`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.preserveSymlinks"></a>

```typescript
public readonly preserveSymlinks: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#preserve-symlinks.

---

##### `publicPath`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.publicPath"></a>

```typescript
public readonly publicPath: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#public-path.

---

##### `pure`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.pure"></a>

```typescript
public readonly pure: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#pure.

---

##### `reserveProps`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.reserveProps"></a>

```typescript
public readonly reserveProps: any;
```

- *Type:* `any`

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `resolveExtensions`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.resolveExtensions"></a>

```typescript
public readonly resolveExtensions: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#resolve-extensions.

---

##### `sourcemap`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.sourcemap"></a>

```typescript
public readonly sourcemap: boolean | string;
```

- *Type:* `boolean` | `string`

Documentation: https://esbuild.github.io/api/#sourcemap.

---

##### `sourceRoot`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.sourceRoot"></a>

```typescript
public readonly sourceRoot: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#source-root.

---

##### `sourcesContent`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.sourcesContent"></a>

```typescript
public readonly sourcesContent: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#sources-content.

---

##### `splitting`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.splitting"></a>

```typescript
public readonly splitting: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#splitting.

---

##### `supported`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.supported"></a>

```typescript
public readonly supported: {[ key: string ]: boolean};
```

- *Type:* {[ key: string ]: `boolean`}

Documentation: https://esbuild.github.io/api/#supported.

---

##### `target`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.target"></a>

```typescript
public readonly target: string | string[];
```

- *Type:* `string` | `string`[]

Documentation: https://esbuild.github.io/api/#target.

---

##### `treeShaking`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.treeShaking"></a>

```typescript
public readonly treeShaking: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#tree-shaking.

---

##### `tsconfig`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.tsconfig"></a>

```typescript
public readonly tsconfig: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#tsconfig.

---

##### `write`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.write"></a>

```typescript
public readonly write: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#write.

---

##### `entryPoints`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderBuildOptions.property.entryPoints"></a>

```typescript
public readonly entryPoints: string[] | {[ key: string ]: string};
```

- *Type:* `string`[] | {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#entry-points.

---

### ProviderTransformOptions <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions"></a>

#### Initializer <a name="[object Object].Initializer"></a>

```typescript
import { ProviderTransformOptions } from '@mrgrain/cdk-esbuild'

const providerTransformOptions: ProviderTransformOptions = { ... }
```

##### `banner`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.banner"></a>

```typescript
public readonly banner: string;
```

- *Type:* `string`

---

##### `charset`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.charset"></a>

```typescript
public readonly charset: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#charset.

---

##### `color`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.color"></a>

```typescript
public readonly color: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#color.

---

##### `define`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.define"></a>

```typescript
public readonly define: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#define.

---

##### `drop`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.drop"></a>

```typescript
public readonly drop: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#drop.

---

##### `footer`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.footer"></a>

```typescript
public readonly footer: string;
```

- *Type:* `string`

---

##### `format`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.format"></a>

```typescript
public readonly format: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#format.

---

##### `globalName`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.globalName"></a>

```typescript
public readonly globalName: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#global-name.

---

##### `ignoreAnnotations`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.ignoreAnnotations"></a>

```typescript
public readonly ignoreAnnotations: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#ignore-annotations.

---

##### `jsx`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.jsx"></a>

```typescript
public readonly jsx: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx.

---

##### `jsxDev`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.jsxDev"></a>

```typescript
public readonly jsxDev: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#jsx-development.

---

##### `jsxFactory`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.jsxFactory"></a>

```typescript
public readonly jsxFactory: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx-factory.

---

##### `jsxFragment`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.jsxFragment"></a>

```typescript
public readonly jsxFragment: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx-fragment.

---

##### `jsxImportSource`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.jsxImportSource"></a>

```typescript
public readonly jsxImportSource: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx-import-source.

---

##### `jsxSideEffects`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.jsxSideEffects"></a>

```typescript
public readonly jsxSideEffects: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#jsx-side-effects.

---

##### `keepNames`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.keepNames"></a>

```typescript
public readonly keepNames: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#keep-names.

---

##### `legalComments`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.legalComments"></a>

```typescript
public readonly legalComments: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#legal-comments.

---

##### `loader`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.loader"></a>

```typescript
public readonly loader: string;
```

- *Type:* `string`

---

##### `logLevel`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.logLevel"></a>

```typescript
public readonly logLevel: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#log-level.

---

##### `logLimit`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.logLimit"></a>

```typescript
public readonly logLimit: number;
```

- *Type:* `number`

Documentation: https://esbuild.github.io/api/#log-limit.

---

##### `logOverride`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.logOverride"></a>

```typescript
public readonly logOverride: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#log-override.

---

##### `mangleCache`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.mangleCache"></a>

```typescript
public readonly mangleCache: {[ key: string ]: string | boolean};
```

- *Type:* {[ key: string ]: `string` | `boolean`}

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `mangleProps`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.mangleProps"></a>

```typescript
public readonly mangleProps: any;
```

- *Type:* `any`

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `mangleQuoted`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.mangleQuoted"></a>

```typescript
public readonly mangleQuoted: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `minify`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.minify"></a>

```typescript
public readonly minify: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `minifyIdentifiers`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.minifyIdentifiers"></a>

```typescript
public readonly minifyIdentifiers: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `minifySyntax`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.minifySyntax"></a>

```typescript
public readonly minifySyntax: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `minifyWhitespace`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.minifyWhitespace"></a>

```typescript
public readonly minifyWhitespace: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `platform`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.platform"></a>

```typescript
public readonly platform: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#platform.

---

##### `pure`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.pure"></a>

```typescript
public readonly pure: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#pure.

---

##### `reserveProps`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.reserveProps"></a>

```typescript
public readonly reserveProps: any;
```

- *Type:* `any`

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `sourcefile`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.sourcefile"></a>

```typescript
public readonly sourcefile: string;
```

- *Type:* `string`

---

##### `sourcemap`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.sourcemap"></a>

```typescript
public readonly sourcemap: boolean | string;
```

- *Type:* `boolean` | `string`

Documentation: https://esbuild.github.io/api/#sourcemap.

---

##### `sourceRoot`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.sourceRoot"></a>

```typescript
public readonly sourceRoot: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#source-root.

---

##### `sourcesContent`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.sourcesContent"></a>

```typescript
public readonly sourcesContent: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#sources-content.

---

##### `supported`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.supported"></a>

```typescript
public readonly supported: {[ key: string ]: boolean};
```

- *Type:* {[ key: string ]: `boolean`}

Documentation: https://esbuild.github.io/api/#supported.

---

##### `target`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.target"></a>

```typescript
public readonly target: string | string[];
```

- *Type:* `string` | `string`[]

Documentation: https://esbuild.github.io/api/#target.

---

##### `treeShaking`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.treeShaking"></a>

```typescript
public readonly treeShaking: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#tree-shaking.

---

##### `tsconfigRaw`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ProviderTransformOptions.property.tsconfigRaw"></a>

```typescript
public readonly tsconfigRaw: string | TsconfigOptions;
```

- *Type:* `string` | [`@mrgrain/cdk-esbuild.TsconfigOptions`](#@mrgrain/cdk-esbuild.TsconfigOptions)

---

### TransformerProps <a name="@mrgrain/cdk-esbuild.TransformerProps"></a>

#### Initializer <a name="[object Object].Initializer"></a>

```typescript
import { TransformerProps } from '@mrgrain/cdk-esbuild'

const transformerProps: TransformerProps = { ... }
```

##### `transformOptions`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformerProps.property.transformOptions"></a>

```typescript
public readonly transformOptions: TransformOptions;
```

- *Type:* [`@mrgrain/cdk-esbuild.TransformOptions`](#@mrgrain/cdk-esbuild.TransformOptions)

Transform options passed on to esbuild.

Please refer to the esbuild Transform API docs for details.

> https://esbuild.github.io/api/#transform-api

---

##### `transformProvider`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformerProps.property.transformProvider"></a>

```typescript
public readonly transformProvider: ITransformProvider;
```

- *Type:* [`@mrgrain/cdk-esbuild.ITransformProvider`](#@mrgrain/cdk-esbuild.ITransformProvider)
- *Default:* new DefaultEsbuildProvider()

The esbuild Transform API implementation to be used.

Configure the default `EsbuildProvider` for more options or
provide a custom `ITransformProvider` as an escape hatch.

---

### TransformOptions <a name="@mrgrain/cdk-esbuild.TransformOptions"></a>

#### Initializer <a name="[object Object].Initializer"></a>

```typescript
import { TransformOptions } from '@mrgrain/cdk-esbuild'

const transformOptions: TransformOptions = { ... }
```

##### `banner`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.banner"></a>

```typescript
public readonly banner: string;
```

- *Type:* `string`

---

##### `charset`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.charset"></a>

```typescript
public readonly charset: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#charset.

---

##### `color`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.color"></a>

```typescript
public readonly color: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#color.

---

##### `define`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.define"></a>

```typescript
public readonly define: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#define.

---

##### `drop`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.drop"></a>

```typescript
public readonly drop: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#drop.

---

##### `footer`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.footer"></a>

```typescript
public readonly footer: string;
```

- *Type:* `string`

---

##### `format`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.format"></a>

```typescript
public readonly format: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#format.

---

##### `globalName`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.globalName"></a>

```typescript
public readonly globalName: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#global-name.

---

##### `ignoreAnnotations`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.ignoreAnnotations"></a>

```typescript
public readonly ignoreAnnotations: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#ignore-annotations.

---

##### `jsx`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.jsx"></a>

```typescript
public readonly jsx: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx.

---

##### `jsxDev`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.jsxDev"></a>

```typescript
public readonly jsxDev: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#jsx-development.

---

##### `jsxFactory`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.jsxFactory"></a>

```typescript
public readonly jsxFactory: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx-factory.

---

##### `jsxFragment`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.jsxFragment"></a>

```typescript
public readonly jsxFragment: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx-fragment.

---

##### `jsxImportSource`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.jsxImportSource"></a>

```typescript
public readonly jsxImportSource: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#jsx-import-source.

---

##### `jsxSideEffects`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.jsxSideEffects"></a>

```typescript
public readonly jsxSideEffects: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#jsx-side-effects.

---

##### `keepNames`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.keepNames"></a>

```typescript
public readonly keepNames: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#keep-names.

---

##### `legalComments`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.legalComments"></a>

```typescript
public readonly legalComments: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#legal-comments.

---

##### `loader`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.loader"></a>

```typescript
public readonly loader: string;
```

- *Type:* `string`

---

##### `logLevel`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.logLevel"></a>

```typescript
public readonly logLevel: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#log-level.

---

##### `logLimit`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.logLimit"></a>

```typescript
public readonly logLimit: number;
```

- *Type:* `number`

Documentation: https://esbuild.github.io/api/#log-limit.

---

##### `logOverride`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.logOverride"></a>

```typescript
public readonly logOverride: {[ key: string ]: string};
```

- *Type:* {[ key: string ]: `string`}

Documentation: https://esbuild.github.io/api/#log-override.

---

##### `mangleCache`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.mangleCache"></a>

```typescript
public readonly mangleCache: {[ key: string ]: string | boolean};
```

- *Type:* {[ key: string ]: `string` | `boolean`}

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `mangleProps`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.mangleProps"></a>

```typescript
public readonly mangleProps: any;
```

- *Type:* `any`

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `mangleQuoted`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.mangleQuoted"></a>

```typescript
public readonly mangleQuoted: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `minify`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.minify"></a>

```typescript
public readonly minify: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `minifyIdentifiers`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.minifyIdentifiers"></a>

```typescript
public readonly minifyIdentifiers: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `minifySyntax`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.minifySyntax"></a>

```typescript
public readonly minifySyntax: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `minifyWhitespace`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.minifyWhitespace"></a>

```typescript
public readonly minifyWhitespace: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#minify.

---

##### `platform`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.platform"></a>

```typescript
public readonly platform: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#platform.

---

##### `pure`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.pure"></a>

```typescript
public readonly pure: string[];
```

- *Type:* `string`[]

Documentation: https://esbuild.github.io/api/#pure.

---

##### `reserveProps`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.reserveProps"></a>

```typescript
public readonly reserveProps: any;
```

- *Type:* `any`

Documentation: https://esbuild.github.io/api/#mangle-props.

---

##### `sourcefile`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.sourcefile"></a>

```typescript
public readonly sourcefile: string;
```

- *Type:* `string`

---

##### `sourcemap`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.sourcemap"></a>

```typescript
public readonly sourcemap: boolean | string;
```

- *Type:* `boolean` | `string`

Documentation: https://esbuild.github.io/api/#sourcemap.

---

##### `sourceRoot`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.sourceRoot"></a>

```typescript
public readonly sourceRoot: string;
```

- *Type:* `string`

Documentation: https://esbuild.github.io/api/#source-root.

---

##### `sourcesContent`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.sourcesContent"></a>

```typescript
public readonly sourcesContent: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#sources-content.

---

##### `supported`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.supported"></a>

```typescript
public readonly supported: {[ key: string ]: boolean};
```

- *Type:* {[ key: string ]: `boolean`}

Documentation: https://esbuild.github.io/api/#supported.

---

##### `target`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.target"></a>

```typescript
public readonly target: string | string[];
```

- *Type:* `string` | `string`[]

Documentation: https://esbuild.github.io/api/#target.

---

##### `treeShaking`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.treeShaking"></a>

```typescript
public readonly treeShaking: boolean;
```

- *Type:* `boolean`

Documentation: https://esbuild.github.io/api/#tree-shaking.

---

##### `tsconfigRaw`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TransformOptions.property.tsconfigRaw"></a>

```typescript
public readonly tsconfigRaw: string | TsconfigOptions;
```

- *Type:* `string` | [`@mrgrain/cdk-esbuild.TsconfigOptions`](#@mrgrain/cdk-esbuild.TsconfigOptions)

---

### TsconfigOptions <a name="@mrgrain/cdk-esbuild.TsconfigOptions"></a>

#### Initializer <a name="[object Object].Initializer"></a>

```typescript
import { TsconfigOptions } from '@mrgrain/cdk-esbuild'

const tsconfigOptions: TsconfigOptions = { ... }
```

##### `compilerOptions`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TsconfigOptions.property.compilerOptions"></a>

```typescript
public readonly compilerOptions: CompilerOptions;
```

- *Type:* [`@mrgrain/cdk-esbuild.CompilerOptions`](#@mrgrain/cdk-esbuild.CompilerOptions)

---

### TypeScriptCodeProps <a name="@mrgrain/cdk-esbuild.TypeScriptCodeProps"></a>

#### Initializer <a name="[object Object].Initializer"></a>

```typescript
import { TypeScriptCodeProps } from '@mrgrain/cdk-esbuild'

const typeScriptCodeProps: TypeScriptCodeProps = { ... }
```

##### `buildOptions`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptCodeProps.property.buildOptions"></a>

```typescript
public readonly buildOptions: BuildOptions;
```

- *Type:* [`@mrgrain/cdk-esbuild.BuildOptions`](#@mrgrain/cdk-esbuild.BuildOptions)

Build options passed on to esbuild. Please refer to the esbuild Build API docs for details.

* `buildOptions.outdir: string`
The actual path for the output directory is defined by CDK. However setting this option allows to write files into a subdirectory. \
For example `{ outdir: 'js' }` will create an asset with a single directory called `js`, which contains all built files. This approach can be useful for static website deployments, where JavaScript code should be placed into a subdirectory. \
*Cannot be used together with `outfile`*.
* `buildOptions.outfile: string`
Relative path to a file inside the CDK asset output directory.
For example `{ outfile: 'js/index.js' }` will create an asset with a single directory called `js`, which contains a single file `index.js`. This can be useful to rename the entry point. \
*Cannot be used with multiple entryPoints or together with `outdir`.*
* `buildOptions.absWorkingDir: string`
Absolute path to the [esbuild working directory](https://esbuild.github.io/api/#working-directory) and defaults to the [current working directory](https://en.wikipedia.org/wiki/Working_directory). \
If paths cannot be found, a good starting point is to look at the concatenation of `absWorkingDir + entryPoint`. It must always be a valid absolute path pointing to the entry point. When needed, the probably easiest way to set absWorkingDir is to use a combination of `resolve` and `__dirname` (see "Library authors" section in the documentation).

> https://esbuild.github.io/api/#build-api

---

##### `buildProvider`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptCodeProps.property.buildProvider"></a>

```typescript
public readonly buildProvider: IBuildProvider;
```

- *Type:* [`@mrgrain/cdk-esbuild.IBuildProvider`](#@mrgrain/cdk-esbuild.IBuildProvider)
- *Default:* new EsbuildProvider()

The esbuild Build API implementation to be used.

Configure the default `EsbuildProvider` for more options or
provide a custom `IBuildProvider` as an escape hatch.

---

##### `copyDir`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptCodeProps.property.copyDir"></a>

```typescript
public readonly copyDir: string | string[] | {[ key: string ]: string | string[]};
```

- *Type:* `string` | `string`[] | {[ key: string ]: `string` | `string`[]}

Copy additional files to the code [asset staging directory](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.AssetStaging.html#absolutestagedpath), before the build runs. Files copied like this will be overwritten by esbuild if they share the same name as any of the outputs.

* When provided with a `string` or `array`, all files are copied to the root of asset staging directory.
* When given a `map`, the key indicates the destination relative to the asset staging directory and the value is a list of all sources to be copied.

Therefore the following values for `copyDir` are all equivalent:
```
{ copyDir: "path/to/source" }
{ copyDir: ["path/to/source"] }
{ copyDir: { ".": "path/to/source" } }
{ copyDir: { ".": ["path/to/source"] } }
```
The destination cannot be outside of the asset staging directory.
If you are receiving the error "Cannot copy files to outside of the asset staging directory."
you are likely using `..` or an absolute path as key on the `copyDir` map.
Instead use only relative paths and avoid `..`.

---

##### `assetHash`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptCodeProps.property.assetHash"></a>

```typescript
public readonly assetHash: string;
```

- *Type:* `string`

A hash of this asset, which is available at construction time.

As this is a plain string, it can be used in construct IDs in order to enforce creation of a new resource when the content hash has changed.

Defaults to a hash of all files in the resulting bundle.

---

### TypeScriptSourceProps <a name="@mrgrain/cdk-esbuild.TypeScriptSourceProps"></a>

#### Initializer <a name="[object Object].Initializer"></a>

```typescript
import { TypeScriptSourceProps } from '@mrgrain/cdk-esbuild'

const typeScriptSourceProps: TypeScriptSourceProps = { ... }
```

##### `buildOptions`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptSourceProps.property.buildOptions"></a>

```typescript
public readonly buildOptions: BuildOptions;
```

- *Type:* [`@mrgrain/cdk-esbuild.BuildOptions`](#@mrgrain/cdk-esbuild.BuildOptions)

Build options passed on to esbuild. Please refer to the esbuild Build API docs for details.

* `buildOptions.outdir: string`
The actual path for the output directory is defined by CDK. However setting this option allows to write files into a subdirectory. \
For example `{ outdir: 'js' }` will create an asset with a single directory called `js`, which contains all built files. This approach can be useful for static website deployments, where JavaScript code should be placed into a subdirectory. \
*Cannot be used together with `outfile`*.
* `buildOptions.outfile: string`
Relative path to a file inside the CDK asset output directory.
For example `{ outfile: 'js/index.js' }` will create an asset with a single directory called `js`, which contains a single file `index.js`. This can be useful to rename the entry point. \
*Cannot be used with multiple entryPoints or together with `outdir`.*
* `buildOptions.absWorkingDir: string`
Absolute path to the [esbuild working directory](https://esbuild.github.io/api/#working-directory) and defaults to the [current working directory](https://en.wikipedia.org/wiki/Working_directory). \
If paths cannot be found, a good starting point is to look at the concatenation of `absWorkingDir + entryPoint`. It must always be a valid absolute path pointing to the entry point. When needed, the probably easiest way to set absWorkingDir is to use a combination of `resolve` and `__dirname` (see "Library authors" section in the documentation).

> https://esbuild.github.io/api/#build-api

---

##### `buildProvider`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptSourceProps.property.buildProvider"></a>

```typescript
public readonly buildProvider: IBuildProvider;
```

- *Type:* [`@mrgrain/cdk-esbuild.IBuildProvider`](#@mrgrain/cdk-esbuild.IBuildProvider)
- *Default:* new EsbuildProvider()

The esbuild Build API implementation to be used.

Configure the default `EsbuildProvider` for more options or
provide a custom `IBuildProvider` as an escape hatch.

---

##### `copyDir`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptSourceProps.property.copyDir"></a>

```typescript
public readonly copyDir: string | string[] | {[ key: string ]: string | string[]};
```

- *Type:* `string` | `string`[] | {[ key: string ]: `string` | `string`[]}

Copy additional files to the code [asset staging directory](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.AssetStaging.html#absolutestagedpath), before the build runs. Files copied like this will be overwritten by esbuild if they share the same name as any of the outputs.

* When provided with a `string` or `array`, all files are copied to the root of asset staging directory.
* When given a `map`, the key indicates the destination relative to the asset staging directory and the value is a list of all sources to be copied.

Therefore the following values for `copyDir` are all equivalent:
```
{ copyDir: "path/to/source" }
{ copyDir: ["path/to/source"] }
{ copyDir: { ".": "path/to/source" } }
{ copyDir: { ".": ["path/to/source"] } }
```
The destination cannot be outside of the asset staging directory.
If you are receiving the error "Cannot copy files to outside of the asset staging directory."
you are likely using `..` or an absolute path as key on the `copyDir` map.
Instead use only relative paths and avoid `..`.

---

##### `assetHash`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptSourceProps.property.assetHash"></a>

```typescript
public readonly assetHash: string;
```

- *Type:* `string`

A hash of this asset, which is available at construction time.

As this is a plain string, it can be used in construct IDs in order to enforce creation of a new resource when the content hash has changed.

Defaults to a hash of all files in the resulting bundle.

---

## Classes <a name="Classes"></a>

### EsbuildBundler <a name="@mrgrain/cdk-esbuild.EsbuildBundler"></a>

Low-level construct that can be used where `BundlingOptions` are required.

This class directly interfaces with esbuild and provides almost no configuration safeguards.

#### Initializers <a name="@mrgrain/cdk-esbuild.EsbuildBundler.Initializer"></a>

```typescript
import { EsbuildBundler } from '@mrgrain/cdk-esbuild'

new EsbuildBundler(entryPoints: string | string[] | {[ key: string ]: string}, props: BundlerProps)
```

##### `entryPoints`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildBundler.parameter.entryPoints"></a>

- *Type:* `string` | `string`[] | {[ key: string ]: `string`}

A path or list or map of paths to the entry points of your code.

Relative paths are by default resolved from the current working directory.
To change the working directory, see `buildOptions.absWorkingDir`.

Absolute paths can be used if files are part of the working directory.

Examples:
  - `'src/index.ts'`
  - `require.resolve('./lambda')`
  - `['src/index.ts', 'src/util.ts']`
  - `{one: 'src/two.ts', two: 'src/one.ts'}`

---

##### `props`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildBundler.parameter.props"></a>

- *Type:* [`@mrgrain/cdk-esbuild.BundlerProps`](#@mrgrain/cdk-esbuild.BundlerProps)

Props to change the behavior of the bundler.

---



#### Properties <a name="Properties"></a>

##### `entryPoints`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildBundler.property.entryPoints"></a>

```typescript
public readonly entryPoints: string | string[] | {[ key: string ]: string};
```

- *Type:* `string` | `string`[] | {[ key: string ]: `string`}

A path or list or map of paths to the entry points of your code.

Relative paths are by default resolved from the current working directory.
To change the working directory, see `buildOptions.absWorkingDir`.

Absolute paths can be used if files are part of the working directory.

Examples:
  - `'src/index.ts'`
  - `require.resolve('./lambda')`
  - `['src/index.ts', 'src/util.ts']`
  - `{one: 'src/two.ts', two: 'src/one.ts'}`

---

##### ~~`image`~~<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildBundler.property.image"></a>

- *Deprecated:* This value is ignored since the bundler is always using a locally installed version of esbuild. However the property is required to comply with the `BundlingOptions` interface.

```typescript
public readonly image: DockerImage;
```

- *Type:* [`aws-cdk-lib.DockerImage`](#aws-cdk-lib.DockerImage)

---

##### `local`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildBundler.property.local"></a>

```typescript
public readonly local: ILocalBundling;
```

- *Type:* [`aws-cdk-lib.ILocalBundling`](#aws-cdk-lib.ILocalBundling)

Implementation of `ILocalBundling` interface, responsible for calling esbuild functions.

---

##### `props`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildBundler.property.props"></a>

```typescript
public readonly props: BundlerProps;
```

- *Type:* [`@mrgrain/cdk-esbuild.BundlerProps`](#@mrgrain/cdk-esbuild.BundlerProps)

Props to change the behavior of the bundler.

---


### EsbuildCode <a name="@mrgrain/cdk-esbuild.EsbuildCode"></a>

Represents a generic esbuild code bundle.

You should always use `TypeScriptCode` or `JavaScriptCode`.

#### Initializers <a name="@mrgrain/cdk-esbuild.EsbuildCode.Initializer"></a>

```typescript
import { EsbuildCode } from '@mrgrain/cdk-esbuild'

new EsbuildCode(entryPoints: string | string[] | {[ key: string ]: string}, props: JavaScriptCodeProps | TypeScriptCodeProps)
```

##### `entryPoints`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildCode.parameter.entryPoints"></a>

- *Type:* `string` | `string`[] | {[ key: string ]: `string`}

A path or list or map of paths to the entry points of your code.

Relative paths are by default resolved from the current working directory.
To change the working directory, see `buildOptions.absWorkingDir`.

Absolute paths can be used if files are part of the working directory.

Examples:
  - `'src/index.ts'`
  - `require.resolve('./lambda')`
  - `['src/index.ts', 'src/util.ts']`
  - `{one: 'src/two.ts', two: 'src/one.ts'}`

---

##### `props`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildCode.parameter.props"></a>

- *Type:* [`@mrgrain/cdk-esbuild.JavaScriptCodeProps`](#@mrgrain/cdk-esbuild.JavaScriptCodeProps) | [`@mrgrain/cdk-esbuild.TypeScriptCodeProps`](#@mrgrain/cdk-esbuild.TypeScriptCodeProps)

Props to change the behavior of the bundler.

Default values for `props.buildOptions`:
- `bundle=true`
- `platform=node`
- `target=nodeX` with X being the major node version running locally

---

#### Methods <a name="Methods"></a>

##### `bind` <a name="@mrgrain/cdk-esbuild.EsbuildCode.bind"></a>

```typescript
public bind(scope: Construct)
```

###### `scope`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildCode.parameter.scope"></a>

- *Type:* [`constructs.Construct`](#constructs.Construct)

---

##### `bindToResource` <a name="@mrgrain/cdk-esbuild.EsbuildCode.bindToResource"></a>

```typescript
public bindToResource(resource: CfnResource, options?: ResourceBindOptions)
```

###### `resource`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildCode.parameter.resource"></a>

- *Type:* [`aws-cdk-lib.CfnResource`](#aws-cdk-lib.CfnResource)

---

###### `options`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.EsbuildCode.parameter.options"></a>

- *Type:* [`aws-cdk-lib.aws_lambda.ResourceBindOptions`](#aws-cdk-lib.aws_lambda.ResourceBindOptions)

---


#### Properties <a name="Properties"></a>

##### `entryPoints`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildCode.property.entryPoints"></a>

```typescript
public readonly entryPoints: string | string[] | {[ key: string ]: string};
```

- *Type:* `string` | `string`[] | {[ key: string ]: `string`}

A path or list or map of paths to the entry points of your code.

Relative paths are by default resolved from the current working directory.
To change the working directory, see `buildOptions.absWorkingDir`.

Absolute paths can be used if files are part of the working directory.

Examples:
  - `'src/index.ts'`
  - `require.resolve('./lambda')`
  - `['src/index.ts', 'src/util.ts']`
  - `{one: 'src/two.ts', two: 'src/one.ts'}`

---

##### ~~`isInline`~~<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildCode.property.isInline"></a>

- *Deprecated:* this value is ignored since inline is now determined based on the the inlineCode field of CodeConfig returned from bind().

```typescript
public readonly isInline: boolean;
```

- *Type:* `boolean`

Determines whether this Code is inline code or not.

---


### EsbuildProvider <a name="@mrgrain/cdk-esbuild.EsbuildProvider"></a>

- *Implements:* [`@mrgrain/cdk-esbuild.IBuildProvider`](#@mrgrain/cdk-esbuild.IBuildProvider), [`@mrgrain/cdk-esbuild.ITransformProvider`](#@mrgrain/cdk-esbuild.ITransformProvider)

Default esbuild implementation calling esbuild's JavaScript API.

#### Initializers <a name="@mrgrain/cdk-esbuild.EsbuildProvider.Initializer"></a>

```typescript
import { EsbuildProvider } from '@mrgrain/cdk-esbuild'

new EsbuildProvider(props?: EsbuildProviderProps)
```

##### `props`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.EsbuildProvider.parameter.props"></a>

- *Type:* [`@mrgrain/cdk-esbuild.EsbuildProviderProps`](#@mrgrain/cdk-esbuild.EsbuildProviderProps)

---

#### Methods <a name="Methods"></a>

##### `buildSync` <a name="@mrgrain/cdk-esbuild.EsbuildProvider.buildSync"></a>

```typescript
public buildSync(options: ProviderBuildOptions)
```

###### `options`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildProvider.parameter.options"></a>

- *Type:* [`@mrgrain/cdk-esbuild.ProviderBuildOptions`](#@mrgrain/cdk-esbuild.ProviderBuildOptions)

---

##### `transformSync` <a name="@mrgrain/cdk-esbuild.EsbuildProvider.transformSync"></a>

```typescript
public transformSync(input: string, options?: ProviderTransformOptions)
```

###### `input`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildProvider.parameter.input"></a>

- *Type:* `string`

---

###### `options`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.EsbuildProvider.parameter.options"></a>

- *Type:* [`@mrgrain/cdk-esbuild.ProviderTransformOptions`](#@mrgrain/cdk-esbuild.ProviderTransformOptions)

---

#### Static Functions <a name="Static Functions"></a>

##### `defaultBuildProvider` <a name="@mrgrain/cdk-esbuild.EsbuildProvider.defaultBuildProvider"></a>

```typescript
import { EsbuildProvider } from '@mrgrain/cdk-esbuild'

EsbuildProvider.defaultBuildProvider()
```

##### `defaultTransformationProvider` <a name="@mrgrain/cdk-esbuild.EsbuildProvider.defaultTransformationProvider"></a>

```typescript
import { EsbuildProvider } from '@mrgrain/cdk-esbuild'

EsbuildProvider.defaultTransformationProvider()
```

##### `overrideDefaultBuildProvider` <a name="@mrgrain/cdk-esbuild.EsbuildProvider.overrideDefaultBuildProvider"></a>

```typescript
import { EsbuildProvider } from '@mrgrain/cdk-esbuild'

EsbuildProvider.overrideDefaultBuildProvider(provider: IBuildProvider)
```

###### `provider`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildProvider.parameter.provider"></a>

- *Type:* [`@mrgrain/cdk-esbuild.IBuildProvider`](#@mrgrain/cdk-esbuild.IBuildProvider)

---

##### `overrideDefaultProvider` <a name="@mrgrain/cdk-esbuild.EsbuildProvider.overrideDefaultProvider"></a>

```typescript
import { EsbuildProvider } from '@mrgrain/cdk-esbuild'

EsbuildProvider.overrideDefaultProvider(provider: IEsbuildProvider)
```

###### `provider`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildProvider.parameter.provider"></a>

- *Type:* [`@mrgrain/cdk-esbuild.IEsbuildProvider`](#@mrgrain/cdk-esbuild.IEsbuildProvider)

---

##### `overrideDefaultTransformationProvider` <a name="@mrgrain/cdk-esbuild.EsbuildProvider.overrideDefaultTransformationProvider"></a>

```typescript
import { EsbuildProvider } from '@mrgrain/cdk-esbuild'

EsbuildProvider.overrideDefaultTransformationProvider(provider: ITransformProvider)
```

###### `provider`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.EsbuildProvider.parameter.provider"></a>

- *Type:* [`@mrgrain/cdk-esbuild.ITransformProvider`](#@mrgrain/cdk-esbuild.ITransformProvider)

---



### EsbuildSource <a name="@mrgrain/cdk-esbuild.EsbuildSource"></a>


#### Static Functions <a name="Static Functions"></a>

##### `anywhere` <a name="@mrgrain/cdk-esbuild.EsbuildSource.anywhere"></a>

```typescript
import { EsbuildSource } from '@mrgrain/cdk-esbuild'

EsbuildSource.anywhere()
```

##### `auto` <a name="@mrgrain/cdk-esbuild.EsbuildSource.auto"></a>

```typescript
import { EsbuildSource } from '@mrgrain/cdk-esbuild'

EsbuildSource.auto()
```

##### `globalPaths` <a name="@mrgrain/cdk-esbuild.EsbuildSource.globalPaths"></a>

```typescript
import { EsbuildSource } from '@mrgrain/cdk-esbuild'

EsbuildSource.globalPaths()
```

##### `install` <a name="@mrgrain/cdk-esbuild.EsbuildSource.install"></a>

```typescript
import { EsbuildSource } from '@mrgrain/cdk-esbuild'

EsbuildSource.install()
```

##### `nodeJs` <a name="@mrgrain/cdk-esbuild.EsbuildSource.nodeJs"></a>

```typescript
import { EsbuildSource } from '@mrgrain/cdk-esbuild'

EsbuildSource.nodeJs()
```

##### `platformDefault` <a name="@mrgrain/cdk-esbuild.EsbuildSource.platformDefault"></a>

```typescript
import { EsbuildSource } from '@mrgrain/cdk-esbuild'

EsbuildSource.platformDefault()
```



### InlineJavaScriptCode <a name="@mrgrain/cdk-esbuild.InlineJavaScriptCode"></a>

An implementation of `lambda.InlineCode` using the esbuild Transform API. Inline function code is limited to 4 KiB after transformation.

#### Initializers <a name="@mrgrain/cdk-esbuild.InlineJavaScriptCode.Initializer"></a>

```typescript
import { InlineJavaScriptCode } from '@mrgrain/cdk-esbuild'

new InlineJavaScriptCode(code: string, props?: TransformerProps)
```

##### `code`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.InlineJavaScriptCode.parameter.code"></a>

- *Type:* `string`

The inline code to be transformed.

---

##### `props`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.InlineJavaScriptCode.parameter.props"></a>

- *Type:* [`@mrgrain/cdk-esbuild.TransformerProps`](#@mrgrain/cdk-esbuild.TransformerProps)

Props to change the behavior of the transformer.

Default values for `props.transformOptions`:
- `loader='js'`
- `platform=node`
- `target=nodeX` with X being the major node version running locally

> https://esbuild.github.io/api/#transform-api

---

#### Methods <a name="Methods"></a>

##### `bind` <a name="@mrgrain/cdk-esbuild.InlineJavaScriptCode.bind"></a>

```typescript
public bind(scope: Construct)
```

###### `scope`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.InlineJavaScriptCode.parameter.scope"></a>

- *Type:* [`constructs.Construct`](#constructs.Construct)

---


#### Properties <a name="Properties"></a>

##### `isInline`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.InlineJavaScriptCode.property.isInline"></a>

```typescript
public readonly isInline: boolean;
```

- *Type:* `boolean`

Determines whether this Code is inline code or not.

---


### InlineTypeScriptCode <a name="@mrgrain/cdk-esbuild.InlineTypeScriptCode"></a>

An implementation of `lambda.InlineCode` using the esbuild Transform API. Inline function code is limited to 4 KiB after transformation.

#### Initializers <a name="@mrgrain/cdk-esbuild.InlineTypeScriptCode.Initializer"></a>

```typescript
import { InlineTypeScriptCode } from '@mrgrain/cdk-esbuild'

new InlineTypeScriptCode(code: string, props?: TransformerProps)
```

##### `code`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.InlineTypeScriptCode.parameter.code"></a>

- *Type:* `string`

The inline code to be transformed.

---

##### `props`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.InlineTypeScriptCode.parameter.props"></a>

- *Type:* [`@mrgrain/cdk-esbuild.TransformerProps`](#@mrgrain/cdk-esbuild.TransformerProps)

Props to change the behavior of the transformer.

Default values for `transformOptions`:
- `loader='ts'`
- `platform=node`
- `target=nodeX` with X being the major node version running locally

> https://esbuild.github.io/api/#transform-api

---

#### Methods <a name="Methods"></a>

##### `bind` <a name="@mrgrain/cdk-esbuild.InlineTypeScriptCode.bind"></a>

```typescript
public bind(scope: Construct)
```

###### `scope`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.InlineTypeScriptCode.parameter.scope"></a>

- *Type:* [`constructs.Construct`](#constructs.Construct)

---


#### Properties <a name="Properties"></a>

##### `isInline`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.InlineTypeScriptCode.property.isInline"></a>

```typescript
public readonly isInline: boolean;
```

- *Type:* `boolean`

Determines whether this Code is inline code or not.

---


### JavaScriptCode <a name="@mrgrain/cdk-esbuild.JavaScriptCode"></a>

Represents the deployed JavaScript Code.

#### Initializers <a name="@mrgrain/cdk-esbuild.JavaScriptCode.Initializer"></a>

```typescript
import { JavaScriptCode } from '@mrgrain/cdk-esbuild'

new JavaScriptCode(entryPoints: string | string[] | {[ key: string ]: string}, props?: JavaScriptCodeProps)
```

##### `entryPoints`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptCode.parameter.entryPoints"></a>

- *Type:* `string` | `string`[] | {[ key: string ]: `string`}

A path or list or map of paths to the entry points of your code.

Relative paths are by default resolved from the current working directory.
To change the working directory, see `buildOptions.absWorkingDir`.

Absolute paths can be used if files are part of the working directory.

Examples:
  - `'src/index.ts'`
  - `require.resolve('./lambda')`
  - `['src/index.ts', 'src/util.ts']`
  - `{one: 'src/two.ts', two: 'src/one.ts'}`

---

##### `props`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptCode.parameter.props"></a>

- *Type:* [`@mrgrain/cdk-esbuild.JavaScriptCodeProps`](#@mrgrain/cdk-esbuild.JavaScriptCodeProps)

Props to change the behavior of the bundler.

Default values for `props.buildOptions`:
- `bundle=true`
- `platform=node`
- `target=nodeX` with X being the major node version running locally

---





### JavaScriptSource <a name="@mrgrain/cdk-esbuild.JavaScriptSource"></a>

- *Implements:* [`aws-cdk-lib.aws_s3_deployment.ISource`](#aws-cdk-lib.aws_s3_deployment.ISource)

#### Initializers <a name="@mrgrain/cdk-esbuild.JavaScriptSource.Initializer"></a>

```typescript
import { JavaScriptSource } from '@mrgrain/cdk-esbuild'

new JavaScriptSource(entryPoints: string | string[] | {[ key: string ]: string}, props?: JavaScriptSourceProps)
```

##### `entryPoints`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptSource.parameter.entryPoints"></a>

- *Type:* `string` | `string`[] | {[ key: string ]: `string`}

A path or list or map of paths to the entry points of your code.

Relative paths are by default resolved from the current working directory.
To change the working directory, see `buildOptions.absWorkingDir`.

Absolute paths can be used if files are part of the working directory.

Examples:
  - `'src/index.ts'`
  - `require.resolve('./lambda')`
  - `['src/index.ts', 'src/util.ts']`
  - `{one: 'src/two.ts', two: 'src/one.ts'}`

---

##### `props`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptSource.parameter.props"></a>

- *Type:* [`@mrgrain/cdk-esbuild.JavaScriptSourceProps`](#@mrgrain/cdk-esbuild.JavaScriptSourceProps)

Props to change the behavior of the bundler.

Default values for `props.buildOptions`:
- `bundle=true`
- `platform=browser`

---

#### Methods <a name="Methods"></a>

##### `bind` <a name="@mrgrain/cdk-esbuild.JavaScriptSource.bind"></a>

```typescript
public bind(scope: Construct, context?: DeploymentSourceContext)
```

###### `scope`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptSource.parameter.scope"></a>

- *Type:* [`constructs.Construct`](#constructs.Construct)

---

###### `context`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptSource.parameter.context"></a>

- *Type:* [`aws-cdk-lib.aws_s3_deployment.DeploymentSourceContext`](#aws-cdk-lib.aws_s3_deployment.DeploymentSourceContext)

---


#### Properties <a name="Properties"></a>

##### `assetClass`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.JavaScriptSource.property.assetClass"></a>

```typescript
public readonly assetClass: JavaScriptAsset;
```

- *Type:* [`@mrgrain/cdk-esbuild.JavaScriptAsset`](#@mrgrain/cdk-esbuild.JavaScriptAsset)

---


### TypeScriptCode <a name="@mrgrain/cdk-esbuild.TypeScriptCode"></a>

Represents the deployed TypeScript Code.

#### Initializers <a name="@mrgrain/cdk-esbuild.TypeScriptCode.Initializer"></a>

```typescript
import { TypeScriptCode } from '@mrgrain/cdk-esbuild'

new TypeScriptCode(entryPoints: string | string[] | {[ key: string ]: string}, props?: TypeScriptCodeProps)
```

##### `entryPoints`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptCode.parameter.entryPoints"></a>

- *Type:* `string` | `string`[] | {[ key: string ]: `string`}

A path or list or map of paths to the entry points of your code.

Relative paths are by default resolved from the current working directory.
To change the working directory, see `buildOptions.absWorkingDir`.

Absolute paths can be used if files are part of the working directory.

Examples:
  - `'src/index.ts'`
  - `require.resolve('./lambda')`
  - `['src/index.ts', 'src/util.ts']`
  - `{one: 'src/two.ts', two: 'src/one.ts'}`

---

##### `props`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptCode.parameter.props"></a>

- *Type:* [`@mrgrain/cdk-esbuild.TypeScriptCodeProps`](#@mrgrain/cdk-esbuild.TypeScriptCodeProps)

Props to change the behavior of the bundler.

Default values for `props.buildOptions`:
- `bundle=true`
- `platform=node`
- `target=nodeX` with X being the major node version running locally

---





### TypeScriptSource <a name="@mrgrain/cdk-esbuild.TypeScriptSource"></a>

- *Implements:* [`aws-cdk-lib.aws_s3_deployment.ISource`](#aws-cdk-lib.aws_s3_deployment.ISource)

#### Initializers <a name="@mrgrain/cdk-esbuild.TypeScriptSource.Initializer"></a>

```typescript
import { TypeScriptSource } from '@mrgrain/cdk-esbuild'

new TypeScriptSource(entryPoints: string | string[] | {[ key: string ]: string}, props?: TypeScriptSourceProps)
```

##### `entryPoints`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptSource.parameter.entryPoints"></a>

- *Type:* `string` | `string`[] | {[ key: string ]: `string`}

A path or list or map of paths to the entry points of your code.

Relative paths are by default resolved from the current working directory.
To change the working directory, see `buildOptions.absWorkingDir`.

Absolute paths can be used if files are part of the working directory.

Examples:
  - `'src/index.ts'`
  - `require.resolve('./lambda')`
  - `['src/index.ts', 'src/util.ts']`
  - `{one: 'src/two.ts', two: 'src/one.ts'}`

---

##### `props`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptSource.parameter.props"></a>

- *Type:* [`@mrgrain/cdk-esbuild.TypeScriptSourceProps`](#@mrgrain/cdk-esbuild.TypeScriptSourceProps)

Props to change the behavior of the bundler.

Default values for `props.buildOptions`:
- `bundle=true`
- `platform=browser`

---

#### Methods <a name="Methods"></a>

##### `bind` <a name="@mrgrain/cdk-esbuild.TypeScriptSource.bind"></a>

```typescript
public bind(scope: Construct, context?: DeploymentSourceContext)
```

###### `scope`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptSource.parameter.scope"></a>

- *Type:* [`constructs.Construct`](#constructs.Construct)

---

###### `context`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptSource.parameter.context"></a>

- *Type:* [`aws-cdk-lib.aws_s3_deployment.DeploymentSourceContext`](#aws-cdk-lib.aws_s3_deployment.DeploymentSourceContext)

---


#### Properties <a name="Properties"></a>

##### `assetClass`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.TypeScriptSource.property.assetClass"></a>

```typescript
public readonly assetClass: TypeScriptAsset;
```

- *Type:* [`@mrgrain/cdk-esbuild.TypeScriptAsset`](#@mrgrain/cdk-esbuild.TypeScriptAsset)

---


## Protocols <a name="Protocols"></a>

### IBuildProvider <a name="@mrgrain/cdk-esbuild.IBuildProvider"></a>

- *Implemented By:* [`@mrgrain/cdk-esbuild.EsbuildProvider`](#@mrgrain/cdk-esbuild.EsbuildProvider), [`@mrgrain/cdk-esbuild.IBuildProvider`](#@mrgrain/cdk-esbuild.IBuildProvider), [`@mrgrain/cdk-esbuild.IEsbuildProvider`](#@mrgrain/cdk-esbuild.IEsbuildProvider)

Provides an implementation of the esbuild Build API.

#### Methods <a name="Methods"></a>

##### `buildSync` <a name="@mrgrain/cdk-esbuild.IBuildProvider.buildSync"></a>

```typescript
public buildSync(options: ProviderBuildOptions)
```

###### `options`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.IBuildProvider.parameter.options"></a>

- *Type:* [`@mrgrain/cdk-esbuild.ProviderBuildOptions`](#@mrgrain/cdk-esbuild.ProviderBuildOptions)

---


### IEsbuildProvider <a name="@mrgrain/cdk-esbuild.IEsbuildProvider"></a>

- *Extends:* [`@mrgrain/cdk-esbuild.IBuildProvider`](#@mrgrain/cdk-esbuild.IBuildProvider), [`@mrgrain/cdk-esbuild.ITransformProvider`](#@mrgrain/cdk-esbuild.ITransformProvider)

- *Implemented By:* [`@mrgrain/cdk-esbuild.IEsbuildProvider`](#@mrgrain/cdk-esbuild.IEsbuildProvider)

Provides an implementation of the esbuild Build & Transform API.



### ITransformProvider <a name="@mrgrain/cdk-esbuild.ITransformProvider"></a>

- *Implemented By:* [`@mrgrain/cdk-esbuild.EsbuildProvider`](#@mrgrain/cdk-esbuild.EsbuildProvider), [`@mrgrain/cdk-esbuild.IEsbuildProvider`](#@mrgrain/cdk-esbuild.IEsbuildProvider), [`@mrgrain/cdk-esbuild.ITransformProvider`](#@mrgrain/cdk-esbuild.ITransformProvider)

Provides an implementation of the esbuild Transform API.

#### Methods <a name="Methods"></a>

##### `transformSync` <a name="@mrgrain/cdk-esbuild.ITransformProvider.transformSync"></a>

```typescript
public transformSync(input: string, options?: ProviderTransformOptions)
```

###### `input`<sup>Required</sup> <a name="@mrgrain/cdk-esbuild.ITransformProvider.parameter.input"></a>

- *Type:* `string`

---

###### `options`<sup>Optional</sup> <a name="@mrgrain/cdk-esbuild.ITransformProvider.parameter.options"></a>

- *Type:* [`@mrgrain/cdk-esbuild.ProviderTransformOptions`](#@mrgrain/cdk-esbuild.ProviderTransformOptions)

---


