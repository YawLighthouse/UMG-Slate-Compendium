<a name="rich-text-block-widget"></a>
# Rich Text Block Widget

---

<a name="table-of-contents"></a>
## Table of Contents

> **[<span>&#8962;</span> Back to Main Compendium Page](../../README.md)**
> 
> **[<span>&#11013;</span> Back to Overview Page](text_widget_overview.md)**
> 
> ### Related Page Links
> 
> - [Text Marshallers Page](text_marshallers.md)
> - [Text Decorators Page](text_decorators.md)

---

<a name="general-description"></a>
## General Description

Rich Text Block's handle displaying not just text but inline images and/or widgets, links, combinations of styles, colors, sizes, fonts, localization support, and other features within a single text block.
Rich Text Block's do not tick logic as they compute their results during the Paint pass of the widget.

---

<a name="important-types"></a>
### Important Types

*@TODO Add graph visuals for this hierarchy*

- `URichTextBlock`[Parent Class: `UTextLayoutWidget`]: UMG implementation of the Rich Text Block.
- `SRichTextBlock`[Parent Class: `SWidget`]: Slate Widget implementation of the Rich Text Block.
- `FDefaultRichTextMarkupParser`[[Compendium Explanation](text_marshallers.md#default-rich-text-markup-parser)]: The default implementation class of rich text markup parsing that handles parsing the inputted `FString` to be separated and returned as the parse results(`TArray<FTextLineParseResults>`) and a `FString` stripped of any markup.
- `FRichTextLayoutMarshaller`[[Compendium Explanation]](text_marshallers.md#rich-text-marshaller): The marshaller that is used when creating the rich text block layout of this widget.
- `ITextDecorator`[[Compendium Explanation]](text_decorators.md): Decorators that handle parsing the source text to apply custom styling and features to the resulted text of this widget.

---

<a name="construction"></a>
### Construction

When constructed(occurs when the blueprint widget is recompiled in the editor) the rich text widget will cache designer inputted data for drawing in this execution order:

1. Caches the markup parser(if applicable otherwise uses `FDefaultRichTextMarkupParser` if nothing is inputted).
2. Caches the text marshaller(if applicable otherwise uses `FRichTextLayoutMarshaller` if nothing is inputted).
3. Caches the optionally inputted decorator's within the marshaller.
4. Caches the text layout[`FSlateTextBlockLayout`] and caches the marshaller within that text layout.

### Painting

Within `OnPaint` the rich text block goes through a more direct drawing phase that is more composed than split for code paths that occur on the CPU.

<a name="rich-text-block--on-paint"></a>
#### Rich Text Block :: On Paint

This occurs in the paint pass in this execution order in the rich text block widget:

1. Starts the draw phase by calculating the geometry of the text block and its position on the screen from the cached text layout.
2. Builds up paint data by routing to the cached text layout's `FSlateTextBlockLayout::OnPaint` to then route to [FSlateTextLayout::OnPaint](#slate-text-layout--on-paint) which handles the actual drawing.
3. Recalculate the layout now that the paint data is ready while accounting for wrapping.
4. If the paint data is causing the new layout size to require the rich text widget to wrap, then invalidate the widget to fully populate the widget's geometry size change to the hierarchy.

<a name="slate-text-layout--on-paint"></a>
#### Slate Text Layout :: On Paint

This occurs in this execution order in slate text layout:

1. Starts by figuring out if we're suppose to apply the disabled effect or not(causing the text to be at a lower opacity).
2. Computes the scale factor of the widget so it scales within the layout.
3. Setting up a lambda to check if a line of text is visible within the viewport(recommend studying this function for your own code implementations).
Taking into account horizontal & vertical culling using `FSlateRect::DoRectanglesIntersect`. This is to skip elements that don't need to be drawn as an optimization.
4. Iterates through each text line and track which layer (found via the text line) is the highest layer or not. This is to have each element layered correctly to account for other widgets.
   - There is also checks to account for if the text should be cut off with Ellipsis(`...`).
   - You can also see a debug box for each line by enabling the console command `Slate.ShowTextDebugging 1`(this is noted in [Main Compendium Page - Debug Console Commands](../../README.md#91-debug-console-commands)).
   - Text overlay's are also included with this highest layer computation process via `FSlateTextLayout::OnPaintHighlights`.
5. Outputs the highest layer ID to paint and draw.

