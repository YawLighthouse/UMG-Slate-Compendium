<a name="text-block-widget"></a>
# Text Block Widget

---

<a name="table-of-contents"></a>
## Table of Contents

> **[<span>&#8962;</span> Back to Main Compendium Page](../../README.md)**
> 
> **[<span>&#11013;</span> Back to Overview Page](text_widget_overview.md)**

---

<a name="general-description"></a>
### General Description

Text Block's handle displaying static text that can be changed at runtime.
Text block's allow for a simple text mode which is intended for optimized rendering of text blocks but only works with text containing numbers or basic ASCII.

> Simple text mode does **NOT** allow for certain languages such as Arabic or Thai, that require complex text rendering support.
> It is useful for text that changes frequently and really shouldn't be used for localized user-facing text.

---

<a name="important-types"></a>
### Important Types

*@TODO Add graph visuals for this hierarchy*

- `UTextBlock`[Parent Class: `UTextLayoutWidget`] UMG implementation of the Text Block.
- `STextBlock`[Parent Class: `SLeafWidget`]: Slate Widget implementation of the Text Block.
- `FSlateTextBlockLayout`: Variable[`TextLayoutCache`] The class to handle caching the layout of the STextBlock as a proxy of the `FTextLayout`.
- `FPlainTextLayoutMarshaller`[[Compendium Explanation]](text_marshallers.md#plain-text-marshaller): The marshaller that is used when creating the text block layout of this widget.

---

<a name="painting"></a>
### Painting

Within `OnPaint` the text block goes through two paint routes on the CPU.

<a name="painting-simple-text-mode"></a>
#### Painting Simple Text Mode

*@TODO Add graph visuals for this section*

Located in `STextBlock::OnPaint`, this occurs in the paint pass in this execution order:

1. Retrieves the local shadow color, opacity, and offset values and checks if a shadow should be drawn based on these values.
2. Checks if the text should be rendered as enabled/disabled based on if the parent is enabled/disabled.
3. Retrieves the local text and font values to be rendered.
4. Checks if shadow rendering is enabled via `ShouldDropShadow`(`local boolean`) which modifies the font settings to remove the outline from the shadow.
   1. Calls `FSlateDrawElement::MakeText` to render the shadow text, with it being offset by `LocalShadowOffset`(`local FVector2D`) and styled according to the shadow font settings.
   2. Restores the outline size to the font's and increments the `LayerID`(`integer`) for the main text to be drawn at that layer ID in front of the shadow text.
5. Calls `FSlateDrawElement::MakeText` to render the main text at the `LayerID`(`integer`) and positions the text according to `AllottedGeometry`(`FGeometry`) and applies the style's and effects that are configured by the designer.
6. Returns the `LayerID`.

<a name="painting-non-simple-text-mode"></a>
#### Painting Non-Simple Text Mode

*@TODO Add graph visuals for this section*

Located in `STextBlock::OnPaint`, this occurs in the paint pass in this execution order:

1. Retrieves the last desired size of the text so we can check if the text size has changed after it was rendered.
2. Tells the `TextLayoutCache` to paint the text via [FSlateTextBlockLayout::OnPaint](#slate-text-block-layout--on-paint) at the `LayerID`(`integer`).
3. Caches the new desired size of the text after its been painted(rendered) via the text layout cache.
4. Checks if the desired size has changed and if wrapping is enabled, to then invalidate the layout of the widget since it requires the layout to be re-arranged.
5. Returns the `LayerID`.

<a name="slate-text-block-layout--onpaint"></a>
##### Slate Text Block Layout :: On Paint

*@TODO Add graph visuals for this section*

This occurs for painting the block layout in this execution order:

1. Caches size of the widget's geometry as `CachedSize`.
2. Computes the wrapping width to pass to the text layout for later use.
3. Locally caches the visual justification of the text layout and computes the auto scroll value based on that justification(can be zero).
4. Updates what part of the text layout is visible via `FTextLayout::SetVisibleRegion` & `FTextLayout::UpdateIfNeeded`.
5. Lets the text layout handle the final aspects of OnPaint with its own `FTextLayout::OnPaint` functionality.

<a name="slate-text-layout--on-paint"></a>
##### Slate Text Layout :: On Paint

This occurs for painting the text layout in this execution order:

1. Decides which drawing effects to show based on whether the parent widget is enabled/disabled. If disabled then uses the disabled effect.
2. Calculates the text's scaling factor from the pre-scaled offset to the painted scaling(including things like the widget's render transform and such).
3. Initializes the highest layer ID to keep track of front-most layer that's being used while painting.
4. Sets up a visibility check to find out if a line of text is visible within the bounds of the widget as an optimization to not compute un-rendered text.
5. Iterates over each line view, skipping lines that won't be rendered/visible.
   1. Renders background highlights for that line via `FSlateTextLayout::OnPaintHighlights`.
   2. Sets up layer indices for rendering text and debugging visuals(incase there is a debug draw layer to draw over).
   3. Computes the overflow policy and direction of this line and how it should be handled such as adding ellipsis at the end. 
   4. Render foreground highlights for that line via `FSlateTextLayout::OnPaintHighlights`.
   5. Updates the highest layer ID to be based on the highest layer during this line's rendering.
6. Returns the highest layer ID.


<a name="text-layout--flow-line-layout"></a>
##### Text Layout :: Flow Line Layout

This occurs when building the flow line layout in this execution order:

1. Locally caches the current Line Model index as `LineModel`(`FLineModel`) to prepare to lay out a single line of text.
2. Checks if `LineModel`'s amount of `RunRenderer`(`TArray<FTextRunRenderer>`)'s is greater than or equal to zero.
3. Locally caches if this line is wrapping by checking if `WrappingWidth` is greater than zero as `IsWrapping`.
4. Checks if the line doesn't have any break candidates or if we're not wrapping the text:
   1. Iterates over all of the run's and creates a line view block via `FTextLayout::CreateLineViewBlocks` for this line model only. 
5. Otherwise it starts processing each word and character so see if we need to move it to the start of the next line to ensure the text fits. Each time it has to create a new line we call `FTextLayout::CreateLineViewBlocks` to run the new line.
   1. Adjusts spacing between words and characters which would involve increasing/decreasing space to make sure the text looks balanced and easy to read.
   2. Aligns the positioning of the words and characters in the line to what the designer wanted via alignment & justification properties.
   3. Checks for special characters/elements of the text such as hyperlinks or embedded images to ensure they are correctly placed and rendered within the line.
   4. Caches line dimensions of width and height.
   5. Applies the line styling such as font size, color, bold/italic, etc.
   6. Caches word and character positioning of exact coordinates for the text to appear at. 
   7. Accounts for special cases. This is a per text widget basis such as: 
      - Truncating text(cutting it off)
      - Adding ellipsis("...")
      - Wrapping it to the next line
      - Applying different fonts
      - Applying inline images
      - Bidirectional text(such as Arabic or Hebrew)
      - Text highlighting and selection
      - Additional accessibility features such as high contract, larger text, etc.
   8. Notifying that this line of text is ready to be rendered(which will be sent to the rendering part of the engine later in the code execution).

<a name="text-layout--flow-highlights"></a>
##### Text Layout :: Flow Highlights

This occurs for applying highlights to specific parts of text in this execution order:

1. Ensure that this only occurs when updating a layout.
2. Iterate through each line(`FLineView`).
   1. Clear out any highlights in this line.
   2. Retrieve the text of this line as a line model(`FLineModel`).
   3. Iterate through each model's highlights(`FTextLineHighlight`).
      1. Check if the highlight is valid and relevant to the current line, otherwise skip it.
      2. Compute the text range(`FTextRange`) of the highlight for this line.
      3. If the highlight doesn't affect this line then skip this highlight.
      4. Iterate through each text block in the line.
         1. Intersect the block's range with the highlight range.
         2. Check if the block is part of the highlight's range.
         3. Checks if this block is the start of the new highlight and adjusts positioning and width accordingly to fit the block.
         4. Computes the highlight width and offset depending on whether its the start of the highlight or a continuation from the previous block and the direction of the text(Left/Right).
         5. If a block is only partially highlighted then adjust the highlight's width and position to fit just the portion.
         6. Cache the highlight data that was computed. 
      5. Apply the highlight to the line either as an foreground(overlay) or background(underlay) based on the z-order.


