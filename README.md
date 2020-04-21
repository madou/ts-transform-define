# ts-transform-define

Allows you to create global constants which can be configured at compile time.
This is the TypeScript equivalent of https://webpack.js.org/plugins/define-plugin/.

## Usage

```bash
npm i ts-transform-define ttypescript
```

Update your tsconfig.json:

```json
{
  "compilerOptions": {
    "plugins": [
      {
        "transform": "ts-transform-define",
        "replace": {
          "PRODUCTION": "true",
          "VERSION": "'5fa3b9'",
          "BROWSER_SUPPORTS_HTML5": true,
          "TWO": "1+1",
          "typeof window": "'object'",
          "process.env.NODE_ENV": "process.env.NODE_ENV"
        }
      }
    ]
  }
}
```

> When replacing with environment variables it will pass them through.

Run TTypeScript instead of TypeScript:

```diff
-tsc
+ttsc
```

> Note that because the plugin does a direct text replacement,
> the value given to it must include actual quotes inside of the string itself.
> Typically,
> this is done either with alternate quotes,
> such as '"production"'.

**index.ts**

```js
if (!PRODUCTION) {
  console.log('Debug info');
}

if (PRODUCTION) {
  console.log('Production log');
}
```

After passing through the transformer:

```js
if (!true) {
  console.log('Debug info');
}
if (true) {
  console.log('Production log');
}
```

and then after a minification pass results in:

```js
console.log('Production log');
```
