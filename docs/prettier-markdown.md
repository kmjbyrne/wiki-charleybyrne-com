---
title: Prettier Markdown Formatting
category: Workflow
tags:
  - prettier
  - markdown
  - config
  - workflow
---

## Prettier Config

Prettier needs the `proseWrap` key set to enforce strict column width formatting
with Markdown.

Additionally, having `tabWidth` as 4 (which is for some languages a rational
default) will case prettier to indent lists by 4 also. This will create MD050
violations.

```js
{
  "semi": true,
  "singleQuote": false,
  "overrides": [
    {
      "files": "*.md",
      "options": {
        "printWidth": 80,
        "proseWrap": "always",
        "tabWidth": 2
      }
    }
  ]
}
```
