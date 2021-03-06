---
title: md-diff node client
permalink: /md-diff/
---

# Package Maintenance Info

Info for local developers to get the package up and running locally and publish it live

## Run package locally

1. In current package directory run `npm link`

   ```bash
   npm link
   ```

2. In the directory you want to consume the package, run the following:

   ```bash
   npm link @ads-vdh/md-diff
   ```

## Deployment

### Project Setup

```bash
npm config set scope ads-vdh
npm config set access public
```

### User Setup

Create an npm account and make sure you are added to [`ads-vdh`](https://www.npmjs.com/settings/ads-vdh/members) org account in order to publish

#### Login to npm using either of the methods

**A) Login to npm**

```bash
npm login
```

or

**B) For [multiple accounts](https://stackoverflow.com/a/50130282/1366033)**

Add `.npmrc` file in the current directory with the following info:

```ini
//registry.npmjs.org/:_authToken=***
```

### Publish Package

Revision version number in `package.json`

```bash
npm publish --access public
```

## Mocha Testing

* [MochaJS](https://mochajs.org/)
* [Debugging in Mocha in VS Code](https://codepunk.io/debugging-mocha-in-visual-studio-code/)

Each test should look something like this:

```js
it('should return del / ins on single word change', function() {
    // arrange
    let diffText = require("../src/index")
    let oldText = "your question site"
    let newText = "your answer site"
    let expected = "your <del>question</del> <ins>answer</ins> site"

    // act
    let actual = diffText(oldText, newText, false)

    // asset
    assert.equal(actual, expected);
});
```

## HTML Testing

Test output against [this fiddle](https://jsfiddle.net/KyleMit/7twbm1zv/) which applies the following styles:

```css
del {
    background: #ffc9c9;
    border-radius: 2px;
    text-decoration: line-through;
}

ins {
    background: #c9fff0;
    border-radius: 2px;
    text-decoration: underline;
}
```


### RegEx

[Tokenize Chars](https://regexr.com/53epc)

```js
/[\*,\."“”\[\]\(\)\n]/g
```

[Tokenize Chars Not in Delimetr](https://regexr.com/53npf)

```js
/[\*,\."“”\[\]\(\)\n](?=(?:[^~]*~~[^~]*~~)*[^~]*$)/g
```

[Detokenize Chars](https://regexr.com/53npo)

```js
/ (<\/?(?:ins|del)>)?([\*,\."“”\[\]\(\)\n])(<\/?(?:ins|del)>)? /g
```

[Extract URLs](https://regexr.com/52gn4)

```js
/https?:\/\/.*?(?=[\)\]\s])/g
```


[Match Text **NOT** in Delimiter](https://regexr.com/53nn7)

```js
/[^~]*(?=(?:[^~]*~[^~]*~)*[^~]*$)/g
```

[Match Text **NOT** in 2 Char delimiter](https://regexr.com/53npc)

```js
/[^~]*(?=(?:[^~]*~~[^~]*~~)*[^~]*$)/g
```

[Match Text Between Brackets](https://regexr.com/53lme)

```js
/(?<=<<).*?(?=>>)/g
```

[Match Text **NOT** Between Brackets](https://regexr.com/53nna)

```js
/[^<>]*(?=(?:[^>]*<[^<]*>)*[^<>]*$)/g
```

[Find NewLine in `<ins>`](https://regexr.com/53eih)


```js
/(?<=<ins>(?!<\/ins>)*?)(\\n\\n)(?=.*?<\/ins>)/g
```

[Find NewLine in `<ins>` or `<del>`](https://regexr.com/53nqm)

```js
/(?<=<(ins|del)>(?!<\/\1>)*?)(\\n\\n)(?=.*?<\/\1>)/g
```

[Match Multiple Tag Groups w/ Back Reference](https://regexr.com/53nre)

```js
/<(ins|del)>.*<\/\1>/g
```

[Match Multiple Tag Groups w/ **Named** Back Reference](https://regexr.com/53nrh)

```js
/<(?<tag>ins|del)>.*<\/\k<tag>/g
```

[Fix md link deltas](https://regexr.com/53nsr)

```js
/\]\(<del>(.*)<\/del> <ins>(.*)<\/ins>\)/g
```
