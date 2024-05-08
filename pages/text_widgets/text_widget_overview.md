<a name="text-widgets-overview"></a>
# Text Widget's Overview
This page is meant to be a hub for text widget related pages for easy navigation as well as include information that covers multiple pages.

---

<a name="table-of-contents"></a>
## Table of Contents

> **[<span>&#8962;</span> Back to Main Compendium Page](../../README.md)**
>  
> ### Internal Page Links
> 
> - [Text Layout Widget](#text-layout-widget)
> - [Other Object Types](#other-object-types)
> - &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Line Model](#line-model)
> - &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Text Layout](#text-layout)
> 
> ### Child Page Links
> 
> - [Text Block](text_block_widget.md)
> - [Rich Text Block](rich_text_block.md)
> - [Text Decorators](text_decorators.md)
> - [Text Marshallers](text_marshallers.md)

---

<a name="text-layout-widget"></a>
## Text Layout Widget

*@TODO Add graph visuals for the hierarchy*

`UTextLayoutWidget` is the base class for all core text widgets in Unreal Engine, these widgets inherit from Text Layout Widget:
- `TextBlock`[Compendium Explanation](text_block_widget.md)
- `RichTextBlock`[Compendium Explanation](rich_text_block.md)
- `MultiLineEditableText`
- `MultiLineEditableTextBox`

*@TODO Add pictures for these properties*

This class is intended to hold common options for text layout, that underlying Slate widget's would read from such as:
- `Shaped Text Options`: Common data options for shaping the text.
  - [Optional Override]`Text Shaping Method`: The method for how the text should be shaped within this widget.
    - `Auto`: Automatically picks the fastest possible shaping method(either Kerning Only or Full Shaping) based on the reading direction of the text. Left-to-right text uses the Kerning Only method, and right-to-left text uses the Full Shaping method.
    - `Kerning Only`: Provides fake shaping using only the kerning data of each letter. Can be faster but won't rendering right-to-left or bi-directional glyphs (such as Arabic) correctly. Basically an optimization of fast but only for simple glyphs such as numbers. 
    - `Full Shaping`: Provides full text shaping, allows more accurate rendering of complex right-to-left or bi-directional glyphs (such as Arabic). This mode will perform ligature(binding) replacement for all languages(such as combined "fi" glyph in English).
  - [Optional Override]`Text Flow Direction`: The directions that text can flow(direction that you read the text) within a paragraph of text.
    - `Auto`: Automatically detects the flow direction for each paragraph from its text.
    - `Left to Right`: Force text to be flowed left-to-right.
    - `Right to Left`: Force text to be flowed right-to-left.
- `Justification`: The orientation to align the text with the margin.
  - `Left`: Justify the text logically to the left.
    - When flowing left-to-right: Will align text visually to the left.
    - When flowing right-to-right: Will align text visually to the right.
  - `Center`: Justify the text logically to the center, text flow direction does not affect this justification.
  - `Right`: Justify the text logically to the right.
    - When flowing left-to-right: Will align text visually to the right.
    - When flowing right-to-right: Will align text visually to the left.
- `Wrapping Policy`: The wrapping policy to use if a word is too long to be broken by the default line-break iterator.
  - `Default Wrapping`: No fallback, uses the default line-break iterator.
  - `Allow Per Character Wrapping`: Fallback to per-character wrapping if a word is too long.
- `Auto Wrapping Text`: Flag for if this text can automatically wrap based on the computed horizontal space.
- `Wrapping Text At`: Optional value for wrapping a new line when text exceeds a certain width; if this value is less than or equal to zero then no wrapping occurs.
- `Line Height Percentage`: The amount of scaling to apply to each text line's height.
- `Margin`: The amount of blank space left around the edges of a text area(usually used for padding space around the widget).

**[<span>&#11014;</span> Back to Top](#table-of-contents)**
<a name="other-object-types"></a>
## Other Object Types

Brief summary explanations of important object types that don't need their own document but need to be mentioned and explained plainly.  

<a name="line-model"></a>
### Line Model

`FLineModel`

A container representing a single string with no manual breaks.
It is intended to be a representation of a line of text, its properties, line specific operations while staying efficient.
Works in tandem with [Text Layouts](#text-layout)

<a name="text-layout"></a>
### Text Layout

`FTextLayout`

A class for computing how the text layout is presented, intended to handle arranging and displaying text like a text editor would.
A text layout is intended to handle arranging and styling text, while being performant and supporting localization and interactive text(hyperlinks and such).

**[<span>&#11014;</span> Back to Top](#table-of-contents)**
