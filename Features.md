


# Implemented Features


### Title

The title is extracted from the yaml metadata and then inserted both in the title page and as the first heading of the introduction.

### Running Header

The running header in .docx was tricky to implement. Before apaquarto 3.0.0, I set the running header with the officer package. This approach required the creation of a new reference document with every render. 

Now, a lua filter looks for the `shorttitle` field. If that is not found, the `title` and `subtitle` fields are used instead. The running header is assigned to the `description` field in the quarto metadata. This value will show up in the `Comments` field in the rendered .docx file, which is then inserted into the field box in the header.


### Authors and Affilations

After version 3.0.0, all these data are processed by Quarto and a lua filter.

## Author Notes

The author notes have many optional parts. These data are processed by a lua filter.

## Abstract


## Captions and References for Figures and Tables

Figure titles, captions, and notes are composed with a series of lua filters.

A note under a figure or a table is set with the apa-note chunk option. 

````

```{r}
#| label: fig-myfigure
#| fig-cap: This is my figure's caption.
#| apa-note: This is a note.

# Code for figure goes here

```
````


## APA Level 4 and 5 Headings Appear with Subsequent Text

Level 4 and 5 Headings remain as true headings that appear in the navigation tab in MS Word. Yet they appear as if they are in the same paragraph with subsequent text. This feature was implemented with apaquarto.lua filter that inserts openxml tags in the headers: 

```openxml

<w:vanish/><w:specVanish/>

```
This creates a *Style Separator* character that you can see in MS Word by clicking the Show/Hide ¶ button. BTW, the CTRL+ALT+Enter keyboard shortcut in Word will insert a style separator. See https://www.cadmanediting.com/the-style-separator-a-hidden-gem-in-ms-word

## Automatically convert css class tags to docx custom style tags

The docxstyler.lua filter converts an expandable list css tags to custom styles automatically. It was adapted from a function in a blog post by James Adams: https://jmablog.com/post/pandoc-filters/

Normally, custom styles in the .docx template can be added with Pandoc's custom-style tag. Suppose our reference .docx file has a custome style called `FigureNote`. We can get Pandoc to use this style like so:

```markdown

:::{custom-style="FigureNote"}
some text
:::

```

This is nice but involves more typing than I prefer. And it is less readable if we want css styles for other formats. For example, 

```markdown

:::{.FigureNote custom-style="FigureNote"}
some text
:::

```
With the docxstyler.lua filter, we need only write this:

```markdown

:::{.FigureNote}
some text
:::

```

I did not want **every** css class to be converted, so I restricted the conversion to what is in the `customclasses` table. Feel free to expand the list as needed.


## Replace ampersands with "and" in all in-text citations

The apa7.csl file works great, except that in-text citations separate author names with ampersands instead of the word *and*. The apaand.lua filter fixes that problem.

The filter was taken from code posted by [Samuel Dodson]{https://github.com/citation-style-language/styles/issues/3748#issuecomment-430871259}.


# Missing Features

* For .docx and .html, there is no option to place intermingled tables and figures at the end like there is in .pdf documents. I imagine that a lua filter solution is possible.
* Create landscape pages for wide figures and tables.
* Masked references
* Figure and Tables in .pdf jou mode do not fit automatically.