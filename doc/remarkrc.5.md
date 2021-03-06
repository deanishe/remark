# remarkrc(5) -- remark config files

## SYNOPSIS

**.remarkrc**, **package.json**

## DESCRIPTION

**remark** gets its configuration from the command line and **remarkrc** files.

For a list of available configuration options, see **remark**(1) or
**remarksetting**(7).

## FILES

`.remarkrc` and `package.json` **remarkrc**(5) configuration files
are in JSON.  `.remarkrc.js` is a CommonJS module exporting a JSON
object.

Automatically detected files named `package.json` use the `remarkConfig`
field, where other files are used as a whole.

## FIELDS

### output

```json
{
  "output": "man/"
}
```

The `output` field specifies whether files should be written to the
file-system (like `-o`, `--output` _path_ on **remark**(1)). It can
be either a boolean, or a string. In the case of a string, the value is
treated as the `target` or `directory` field for **mv**(1).

### settings

```json
{
  "settings": {
    "commonmark": true,
    "bullet": "*"
  }
}
```

Settings has an object mapping a setting to a value.
See **remarksetting**(7) for available settings.

### plugins

List:

```json
{
  "plugins": [
    "toc"
  ]
}
```

Options:

```json
{
  "plugins": {
    "github": {
      "repository": "foo/bar"
    }
  }
}
```

The `plugins` field has either an array of plugins, or an object mapping
plugins to their options.

When a plugin is prefixed with `remark-` (which is recommended), the prefix
can be omitted in the plugin list or map.

## CASCADE

Precedence is as follows:

*   Plug-ins and settings passed to **remark**(1);

*   Files passed to **remark**(1);

*   Files named `.remarkrc`, `.remarkrc.js`, and `remarkConfig` fields
    in `package.json` in the directory of the processed file, and in
    ancestral directories;

*   If no `.remarkrc`, `.remarkrc.js`, and `package.json` were detected
    in the directory of the file or its ancestral directories, per-user
    configuration files (`~/.remarkrc` and `~/.remarkrc.js`) are used;

If more than one `.remarkrc`, `.remarkrc.js`, or `package.json` are
found in a directory, the file named `.remarkrc` takes precedence in the
cascade over `.remarkrc.js`, which in turn precedes over `package.json`.

For example, for the following project:

```text
project
|-- docs
|   |-- .remarkrc
|   |-- doc.md
|
|-- .remarkrc.js
|-- package.json
|-- readme.md
```

Where `docs/.remarkrc` looks as follows:

```json
{
  "settings": {
    "bullet": "+"
  }
}
```

And `package.json` has:

```json
{
  "remarkConfig": {
    "settings": {
      "bullet": "*"
    }
  }
}
```

And `.remarkrc.js` has:

```js
module.exports = {
  settings: {
    bullet: '-'
  }
};
```

Then, when compiling `docs/doc.md`, **remark**(1) would use `bullet: "+"`
because `docs/.remarkrc` takes precedence over `.remarkrc.js` and
`package.json`.

When compiling `readme.md`, **remark**(1) would use `bullet: "-"`, because
`.remarkrc.js` takes precedence over `package.json`.

## BUGS

<https://github.com/wooorm/remark/issues>

## SEE ALSO

**remark**(1), **remarkignore**(5), **remarksetting**(7)
