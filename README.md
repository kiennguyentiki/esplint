# ESplint :face_with_head_bandage:

## Install

```
npm install esplint --save-dev
```

### Recommended Usage

```
npm install lint-staged husky --save-dev
```

```diff json
{
  "scripts": {
+   "precommit": "lint-staged",
+   "prepush": "esplint --tidy . ",
  },
+ "lint-staged": {
+   "*.js": ["esplint --enforced --fix", "git add"]
+ }
}
```

## CLI options

```
esplint [options] files

Configuration:
  --enforced  Turn on enforced rules
  --tidy      Remove unnecessary rules

Commands:
  --init      Create ESplint rules for all failing eslint rules
  --generate  Generates an .eslintrc file for your editor
```

### `--tidy`

This option allows ESplint to keep your `.esplintrc` file clean. If it runs and a rule has no errors/warnings, it will be replaced with the `"target"` setting.

For example,

```json
{
  "rules": {
    "no-console": {
      "substitute": "warn",
      "target": "error"
    }
  }
}
```

would be replaced with:

```json
{
  "rules": {
    "no-console": "error"
  }
}
```

> **Important Note:** Make sure you run ESplint against all the files you wish to lint when the `--tidy` is on. Otherwise it will replace runs when it sees no errors/warnings on only a subsection of your code.

### `--enforce`

This option runes ESplint with all the rules with property `"enforce": true` set to their `"target"` setting.

### `--generate`

Uses `.esplintrc` to generate a `.eslintrc`. This is helpful if your editor has a linter that can look for `.eslintrc`.

If you change `.esplintrc`, you will have to rerun `esplint --generate` so those changes reflect in `.eslintrc`.

Any changes ESplint makes to `.esplintrc` (such as `--tidy`) will automatically re-generate if you have a `.eslintrc`.

## Checks

* `.eslintrc` version -> warning (might want to --generate)
* substitute === target -> error
* substitute === 'off' -> error
* target === 'off' -> error

## Configuration

```json
// .esplintrc

{
  "rules": {
    "no-console": {
      "enforce": true,
      "substitute": "warn",
      "target": "error"
    }
  }
}
```

Generates

```json
// .eslintrc

// Generated by esplint
// Don't change this file.
// Change `.esplintrc` and run `esplint --generate`.
{
  "esplint-version": "5aef23",
  "rules": {
    "no-console": "warn"
  }
}
```
