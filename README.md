# Markdown to PDF
This is the personal setup I use to compile Markdown documents into PDF documents (for example, when taking notes in class). It first compiles the Markdown document into HTML with `pandoc`, then using `wkhtmltopdf` with a custom CSS file, renders that HTML into a PDF. The setup is basically just 2 extremely simple shell scripts and a stylesheet.

I have the compiler (`md_to_pdf`) code as a function as a part of a larger general compiler script that identifies the file type and uses the appropriate compile function. This script can then be easily binded to a Vim mapping that lets me quickly compile all my documents without leaving the window/application from which I am writing it up. The compile script is a basic Shell script.

The compiler compiles the HTML and PDF files into their own folder in order to prevent the root folder from becoming cluttered in case of multiple markdown documents within the same directory. The HTML site can be viewed within the browser, and will have its own minor differences from the PDF export that are more suited to a website.

It also supports MathJax, allowing you to include LaTeX-style math within the Markdown document. MathJax will be enabled when passing a second parameter to the compile script, which will be used for the `--javascript-delay` setting for `wkhtmltopdf`, specifying a delay in milliseconds to wait for JavaScipt before generating the PDF. This delay is required for MathJax to render in time. On my 2016 laptop with an Intel Pentium N3710, I typically use a 1000ms delay.

`md_doc` is a simple shell script that bootstraps a simple Markdown file, with fields for title, author, and date, all of which will be displayed in the final document with appropriate positioning, along with a link to the custom CSS file that is included by Pandoc, and used by wkhtmltopdf.

The generated PDF is intended to be suitable for a physical print-out of the document on an A4 paper.

## File Locations/Setup
- The `Templates` folder should be placed within the home directory as `md_doc` refers to it this way (`~/Templates/document.md`). If you wish to place it elsewhere, you will have to modify the location in the `md_doc` script.
- Similarly, `markdown.css` should also be placed within the home directory, and if you wish to place it elsewhere, `Templates/document.md` will have to be modified to reflect it.
- The `md_to_pdf` and `md_doc` scripts can be placed anywhere. Personally, I have `md_doc` in `~/bin/` (which I add to my `PATH`) and `compile` (which includes the `md_to_pdf` code as a function in addition to functions for other file types) in `~/.scripts/`, which I bind to a Vim mapping and call from directly within my editor.

## Usage
- Run `md_doc <filename>` to create a markdown document file `<filename>.md` using the template providing the stylesheet.
- To generate the PDF from the Markdown file, run `/path/to/md_to_pdf <filename>` (including the `.md` extension). This will create `md_export_html` and `md_export_pdf` folders, containing the exported `.html` and `.pdf` files respectively.

## Styles/Markdown Coverage
The `markdown.css` file provides styles including the following:

- Dark grey text on white background with blue accents
- Lato font (needs to be installed on system, falls back to Calibri, Roboto, etc)
- Max `body` width (useful for viewing the `.html` file in the browser
- Document title
- Heading styles (`h1` - `h6`) with horizontal rule underneath
- Positioning styles for `author` and `date`
- Blockquote styles (`> ` in Markdown) using the blue accent
    - Red blockquotes (requires `::: {.red} [content] :::` in Markdown)
    - Cite styles (`> <cite></cite>`)
- Columns (`::: {.column}`)
- Custom unordered list bullet (cycles through 3 types depending on nest level, stops cycling after 6th level)
- Anchor link styles with hover styles and transitions for HTML browser view
- `<dt>` style
