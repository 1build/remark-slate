# remark-slate

> Transform the contents of a slate 0.50+ editor into markdown and back again.

[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]

[**remark**][remark] plugin to compile Markdown as a [Slate](https://www.slatejs.org/) 0.50+ compatible object.

## Usage

### Serializing from slate object to markdown:

`remark-slate` exports a relatively opinionated `serialize` function that is meant to be invoked with a slate 0.50+ state object and will transform the object into a markdown document.

```js
import { serialize } from 'remark-slate';

export default ({ onChange }) => {
  const [value, setValue] = useState(initialValue);

  React.useEffect(() => {
    // serialize slate state to a markdown string
    onChange(value.map((v) => serialize(v)).join());
  }, [onChange, value]);

  return (
    <Slate editor={editor} value={value} onChange={(value) => setValue(value)}>
      <Editable
        renderElement={renderElement}
        renderLeaf={renderLeaf}
        placeholder="Enter some rich text…"
        ...
      />
      ...
    </Slate>
  );
};
```

### Deserializing from markdown to slate object:

When deserializing, this package is meant to be used with [remark-parse](https://github.com/remarkjs/remark/tree/master/packages/remark-parse)

Say we have the following file, `example.md`:

```markdown
# Heading one

## Heading two

### Heading three

#### Heading four

##### Heading five

###### Heading six

Normal paragraph

_italic text_

**bold text**

~~strike through text~~

[hyperlink](https://jackhanford.com)

> A block quote.

- bullet list item 1
- bullet list item 2

1. ordered list item 1
1. ordered list item 2
```

And our script, `example.js`, looks as follows:

```js
import fs from 'fs';
import unified from 'unified';
import markdown from 'remark-parse';
import slate from 'remark-slate';

unified()
  .use(markdown)
  .use(slate)
  .process(fs.readFileSync('example.md'), (err, file) => {
    if (err) throw err;
    console.log({ file });
  });
```

Would result in the following:

```json
[
  {
    "type": "heading_one",
    "children": [
      {
        "text": "Heading one"
      }
    ]
  },
  {
    "type": "heading_two",
    "children": [
      {
        "text": "Heading two"
      }
    ]
  },
  {
    "type": "heading_three",
    "children": [
      {
        "text": "Heading three"
      }
    ]
  },
  {
    "type": "heading_four",
    "children": [
      {
        "text": "Heading four"
      }
    ]
  },
  {
    "type": "heading_five",
    "children": [
      {
        "text": "Heading five"
      }
    ]
  },
  {
    "type": "heading_six",
    "children": [
      {
        "text": "Heading six"
      }
    ]
  },
  {
    "type": "paragraph",
    "children": [
      {
        "text": "Normal paragraph"
      }
    ]
  },
  {
    "type": "paragraph",
    "children": [
      {
        "text": "italic text",
        "italic": true
      }
    ]
  },
  {
    "type": "paragraph",
    "children": [
      {
        "text": "bold text",
        "italic": true
      }
    ]
  },
  {
    "type": "paragraph",
    "children": [
      {
        "text": "strike through text",
        "strikeThrough": true
      }
    ]
  },
  {
    "type": "paragraph",
    "children": [
      {
        "type": "link",
        "link": "https://jackhanford.com",
        "children": [
          {
            "text": "hyperkink"
          }
        ]
      }
    ]
  },
  {
    "type": "block_quote",
    "children": [
      {
        "type": "paragraph",
        "children": [
          {
            "text": "A block quote."
          }
        ]
      }
    ]
  },
  {
    "type": "ul_list",
    "children": [
      {
        "type": "list_item",
        "children": [
          {
            "type": "paragraph",
            "children": [
              {
                "text": "bullet list item 1"
              }
            ]
          }
        ]
      },
      {
        "type": "list_item",
        "children": [
          {
            "type": "paragraph",
            "children": [
              {
                "text": "bullet list item 2"
              }
            ]
          }
        ]
      }
    ]
  },
  {
    "type": "ol_list",
    "children": [
      {
        "type": "list_item",
        "children": [
          {
            "type": "paragraph",
            "children": [
              {
                "text": "ordered list item 1"
              }
            ]
          }
        ]
      },
      {
        "type": "list_item",
        "children": [
          {
            "type": "paragraph",
            "children": [
              {
                "text": "ordered list item 2"
              }
            ]
          }
        ]
      }
    ]
  }
]
```

```js
{
  type: 'heading',
  depth: 1,
  children: [{ value: 'Big text' }]
}
```

…would yield:

```json
{
  "type": "heading_one",
  "children": [{ "text": "Big text" }]
}
```

## Local Development

Below is a list of commands you will probably find useful.

### `npm start` or `yarn start`

Runs the project in development/watch mode. The project will be rebuilt upon changes.

Your library will be rebuilt if you make edits.

### `npm run build` or `yarn build`

Bundles the package to the `dist` folder.
The package is optimized and bundled with Rollup into multiple formats (CommonJS, UMD, and ES Module).

### `npm test` or `yarn test`

Runs the test watcher (Jest) in an interactive mode.
By default, runs tests related to files changed since the last commit.

## License (MIT)

```
WWWWWW||WWWWWW
 W W W||W W W
      ||
    ( OO )__________
     /  |           \
    /o o|    MIT     \
    \___/||_||__||_|| *
         || ||  || ||
        _||_|| _||_||
       (__|__|(__|__|
```

Copyright © 2020-present [Jack Hanford](http://jackhanford.com), jackhanford@gmail.com

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

<!-- Definitions -->

[downloads-badge]: https://img.shields.io/npm/dm/remark-slate.svg
[downloads]: https://www.npmjs.com/package/remark-slate
[size-badge]: https://img.shields.io/bundlephobia/minzip/remark-slate.svg
[size]: https://bundlephobia.com/result?p=remark-slate
[remark]: https://github.com/remarkjs/remark
