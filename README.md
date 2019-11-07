# requirejs-monaco-ember-polyfill

This polyfill was created for Ember apps that use the ESM/Webpack configuration of [monaco-editor](https://github.com/Microsoft/monaco-editor-samples).

It attaches a fake `.s.contexts._config` with a value of `''` to `window.requirejs`.

## Usage

```js
import requirejsContext from 'requirejs-monaco-ember-polyfill';
import * as monaco from 'monaco-editor';

requirejsContext()
```

## Why is this needed?

This is needed because the `SimpleWorker.js` in `monaco-editor` has the following code:

```js
loaderConfiguration = self.requirejs.s.contexts._.config;
```

However, Ember apps have their own `requirejs` that do not have these properties/methods.
So, we need to stub or polyfill it to prevent type errors when monaco tries to look up the `config`.

This has to be a node module because it must resolve before monaco's workers initialize. Required dependencies are evaluated before any other code in a JavaScript file.

Without this polyfill, the Workers will fail to initialize.