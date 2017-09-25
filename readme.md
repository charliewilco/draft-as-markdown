# draft-to-markdown

`draft-to-markdown.now.sh/` is the micro service version of the really awesome [markdown-draft-js](https://github.com/Rosey/markdown-draft-js) module. Submitting a `POST` request to this endpoint will send back a stream of a blob of your `EditorState` as a formatted markdown file with simple front matter.

## Usage

**POST** __`/`__ render markdown text

```javascript
{
    content: '<RawContent>',
    title: '<DocumentTitle>'
}
```

```javascript
import { saveAs } from 'file-saver'
import { EditorState, convertToRaw } from 'draft-js'

const body = {
  title: 'Something Detailed Post'
  content: convertToRaw(EditorState.getCurrentContent())
}

const exportMD = async (body: Object) => {
  const res = await fetch('https://draft-to-markdown.now.sh/', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(body)
  })

  const blob = await res.blob()

  saveAs(blob, `${body.title.replace(/\s+/g, '-').toLowerCase()}.md`)
}
```
