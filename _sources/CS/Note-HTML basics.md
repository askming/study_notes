# HTML Basics

## What is HTML?

- **Elements**: opening tag, closing tag (sometimes optional), the content (sometimes optional). Elements can be nested.
  - Doctype: `<!DOCTYPE html>`
  - Root (the `<html>`) element: `<html> </html>`
  - `<head>` element: acts as a container for all the stuff you want to include on the HTML page that *isn't* the content you are showing to your page's viewers. 
  - meta element: `<meta chrset = "utf-8">`, sets the character set your document should use to UTF-8 which includes most characters from the vast majority of written languages.
  - `<title>` element: put within `<head> </head>`, sets the title of your page, which is the title that appears in the browser tab the page is loaded in. It is also used to describe the page when you bookmark/favourite it.
  - `<body>` element: contains *all* the content that you want to show to web users when they visit your page, whether that's text, images, videos, games, playable audio tracks or whatever else.
- **Attributes**: extra information about the element that doesn't apprea in the actual content, put at the opening tag of a element with attribute name + = + "attribute content"

## Images

- It embeds an image into our page in the position it appears. It does this via the `src` (source) attribute, which contains the path to our image file.

```{html}
<img src = "link_to_img.png" alt = "description tex">
```

- `alt` stands for "alternative"

## Marking up text

- Headings: HTML contains 6 heading levels, `<h1> - <h6>`

  ```{html}
  <h1> text </h1>
  <h2> text </h2>
  ```

  

- Paragraphs: for containing paragraphs of text

  ```{html}
  <p> this is a single paragraph </p>
  ```

  

- Lists

  - `<ul></ul>`: unordered list
  - `<ol></ol>`: ordered list
  - `<li>text</li>`: list item

## Links

- To add a link, we need to use a simple element — [`a`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a) — "a" being the short form for "anchor".

  ```{html}
  <a href = "https://">text</a>
  ```





[^html_basics]: https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/HTML_basics#Links