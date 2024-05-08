<a name="text-decorators"></a>
# Text Decorators

---

<a name="table-of-contents"></a>
## Table of Contents

> **[<span>&#8962;</span> Back to Main Compendium Page](../../README.md)**
>
> **[<span>&#11013;</span> Back to Overview Page](text_widget_overview.md)**
> 
> ### Related Page Links
> 
> - [Rich Text Block Widget](rich_text_block.md)
> - [Text Marshallers](text_marshallers.md)

---

<a name="general-description"></a>
## General Description

Text Decorators are used to add custom styling or interactive elements withing text blocks dynamically.
An example would be adding a hyperlink, an image within the text without having to have two text block widget's and an image widget placed in-between them, an in-game tooltip, etc.

You can configure which decorators to use by listing them in a data table with each entry in the table linking to the decorator type and how it can affect the resulted text.

> An important aspect of decorators when you configure them on a per rich text block basis, 
> is that they are applied in the order that they are configured so if the last decorator is intended to affect all of them.
> 
> You can see an example of this in the Lyra example project where they will configure a list of decorators with the last on being used to apply that decorator to all of the remaining text that wasn't specifically being modified by other decorators.

They are injected into the rich text layout marshaller([Text Marshallers Page - Rich Text Marshaller](text_marshallers.md#rich-text-marshaller)) and then applied when text properties are changed/set.

---

<a name="important-types"></a>
### Important Types

*@TODO Add graph visuals for this hierarchy*

- `ITextDecorator`: Base class, used as an interface for all text decorator types.
  - `FRichTextDecorator`[Parent Class: `ITextDecorator`]: Base class for all rich text decorator types to execute decorator functionality, intended to be inherited from for project specific code IN COORDINATION with `URichTextBlockDecorator`.
    - `FRichInlineImage` [Parent Class: `FRichTextDecorator`]: A default rich text inline image decorator for utility usage.
- `URichTextBlock`[Parent Class: `UTextLayoutWidget`]: UMG implementation of the Rich Text Block.
- `SRichTextBlock`[Parent Class: `SWidget`]: Slate Widget implementation of the Rich Text Block.
- `URichTextBlockDecorator`[Parent Class: `UObject`]: Base class for all rich text decorator types to configure decorator functionality, intended to be inherited from for project specific code IN COORDINATION with `FRichTextDecorator`.
- `SRichInlineImage`[Parent Class: `SCompoundWidget`]: The default rich inline image widget to display for `FRichInlineImage`.

---

<a name="important-functions"></a>
## Important Functions

- `FRichTextDecorator::Supports`: Override this to check if a text run has tags that are supported by this decorator. Refer to `FRichInlineImage::Supports` as example usage.
- `FRichTextDecorator::CreateDecoratorWidget`: Override this to create a custom widget(like an image, button, etc). Refer to `FRichInlineImage::CreateDecoratorWidget` as example usage.
- `FRichTextDecorator::CreateDecoratorText`: Override this to generate text at runtime, change the style, etc.
- `URichTextBlockDecorator::CreateDecorator`: Override this for creating your custom decorator class to execute functionality on it. Refer to `URichTextBlockImageDecorator::CreateDecorator` as example usage.

<a name="parsing-for-a-decorator"></a>
## Parsing for a Decorator

Each time when a decorator is checked, it is given an already parsed and separated text run(so its ready for you to input your own functionality!).
You can parse for a decorator type in your own code by comparing the `FTextRunParseResults::Name` property against a literal string wrapped with `TEXT()`.
For parsing parameters you can check within `FTextRunParseResults::MetaData`[`TMap<FString, FString>`, key is the parameter ID and value is the parameter's value] and use the `Contains` function and input the parameter wrapped with `TEXT()`.
For getting its value as well, you can use the `Find` function instead of `Contains`.

Example situation: `<img id="MyCoolImage" width="40" height="desired" stretch="ScaleToFitY"/>`
```c++
virtual TSharedPtr<SWidget> CreateDecoratorWidget(const FTextRunInfo& RunInfo, const FTextBlockStyle& TextStyle) const override
{
    const bool bWarnIfMissing = true;
    // Checking for the parameter "id"
	const FSlateBrush* Brush = Decorator->FindImageBrush(*RunInfo.MetaData[TEXT("id")], bWarnIfMissing);
	if(!ensure(Brush))
	{
	    return TSharedPtr<SWidget>();
	}
	
	// Checking for the parameter "width"
    TOptional<int32> Width;
    if (const FString* WidthString = RunInfo.MetaData.Find(TEXT("width")))
    {
        int32 WidthTemp;
        // Parse if its an integer
        if (FDefaultValueHelper::ParseInt(*WidthString, WidthTemp))
        {
            Width = WidthTemp;
        }
        // Otherwise try to use the image's size X if the user inputted "desired"
        else if (FCString::Stricmp(GetData(*WidthString), TEXT("desired")) == 0)
        {
            Width = Brush->ImageSize.X;
        }
    }
    
    // Checking for the parameters "height"
    TOptional<int32> Height;
    if (const FString* HeightString = RunInfo.MetaData.Find(TEXT("height")))
    {
        int32 HeightTemp;
        // Parse if its an integer
        if (FDefaultValueHelper::ParseInt(*HeightString, HeightTemp))
        {
            Height = HeightTemp;
        }        
        // Otherwise try to use the image's size Y if the user inputted "desired"
        else if (FCString::Stricmp(GetData(*HeightString), TEXT("desired")) == 0)
        {
            Height = Brush->ImageSize.Y;
        }        
    }
    
    // Grab the "stretch" enum parameter if applicable
    EStretch::Type Stretch = EStretch::ScaleToFit;
    if (const FString* SstretchString = RunInfo.MetaData.Find(TEXT("stretch")))
    {
        const UEnum* StretchEnum = StaticEnum<EStretch::Type>();
        int64 StretchValue = StretchEnum->GetValueByNameString(*SstretchString);
        if (StretchValue != INDEX_NONE)
        {
            Stretch = static_cast<EStretch::Type>(StretchValue);
        }
    }
    // Acutally create the widget will all of the parameters we figured out while parsing in this function
    return SNew(SRichInlineImage, Brush, TextStyle, Width, Height, Stretch);
}
```

<a name="image-inline-decorator"></a>
## Inline Image Decorator

Decorator Types:
- `URichTextBlockImageDecorator`: Configuration.
- `FRichInlineImage`: Functionality.
- `FRichImageRow`: Data Table Row Type.
- `SRichInlineImage`: Compound Widget to display inline.

The default inline image decorator adds an image widget within the text with some configurations
(you are welcome to make your own and use that instead if it doesn't provide all of the configuration tags that you need).

For tagging that you want an inline image, enter into the text the image ID
(which is retrieved via the row name that the data table that you input into the decorator, you can see where this ID is checked within `URichTextBlockImageDecorator::FindImageBrush`).
This is primarily done in `FRichInlineImage::Supports`.

Example: `<img id="MyId"/>`
- `<`: Starting the decorator "tag" statement.
- `img `: The name of the decorator tag type(if there is a decorator looking for that), the space afterwards is the signal for the system to know that we're done writing out the name of the tag.
- `id`: A parameter of the `img` tag. It is also required for the tag to consider it usable for a decorator. This specifies that we need to lookup an image ID from the data table's row name.
- `=`: Specifying the next set of text is the value for `id`.
- `"MyId"`: Surrounded in quotation marks for parsing purposes, `MyId` is the value of `id`. This decorator is coded to use `id`'s value as the data table row name to lookup.
- `/>`: Signals to the system that we are done with this decorator tag statement.

This decorator will display a `SRichInlineImage` compound widget which is slotted in this fashion:
- ChildSlot:
  - `SBox`: Inputs Width & Height parameters from text markdown.
    - `SScaleBox`: Inputs the Stretch parameter from text markdown, sets the stretch direction[EStretchDirection] to down only, with its vertical alignment being center.
      - `SImage`: Inputs the required ID parameter from text markdown for the image asset.

### Tag Options

This is handled within `FRichInlineImage::CreateDecoratorWidget`.

The available tags that this decorator offers are(case sensitive), you can combine multiple or have all of them in a single decorator tag statement:
- `width` [Example: `<img id="MyId" width="40"/>`](`int32`): Adjusts the image widget to be the specified width value.
  - `desired` [Example: `<img id="MyId" width="desired"/>`]: Optional value to use the desired width of the image widget instead of an `int32`.
- `height` [Example: `<img id="MyId" height="40"/>`](`int32`): Adjusts the image widget to be the specified height value.
  - `desired` [Example: `<img id="MyId" height="desired"/>`]: Optional value to use the desired height of the image widget instead of an `int32`.
- `stretch` [Example: `<img id="MyId" stretch="ScaleToFitY"`](`EStretch::Type`): Stretches the image widget's stretch value. Default value is `EStretch::ScaleToFit`. 
  - `EStretch::Type` Options:
    - `None`: Does not scale the content.
    - `Fill`: Scales the content non-uniformly filling the entire space of the area.
    - `ScaleToFit`[DEFAULT]: Scales the content uniformly (preserving aspect ratio) until it can no longer scale the content without clipping it.
    - `ScaleToFitX`: Scales the content uniformly (preserving aspect ratio) until it can no longer scale the content without clipping it along the x-axis, the y-axis can/will be clipped.
    - `ScaleToFitY`: Scales the content uniformly (preserving aspect ratio) until it can no longer scale the content without clipping it along the y-axis, the x-axis can/will be clipped.
    - `ScaleToFill`: Scales the content uniformly (preserving aspect ratio), until all sides meet or exceed the size of the area.  Will result in clipping the longer side.
    - `ScaleBySafeZone`: Scales the content according to the size of the safe zone currently applied to the viewport. *WARNING* This is not properly accounted for at the moment due to user specification dependency.
    - `UserSpecified`: Scales the content by the scale specified by the user. *WARNING* This is not properly accounted for at the moment due to user specification dependency.
    - `UserSpecifiedWithClipping`: Scales the content by the scale specified by the user and also clips. *WARNING* This is not properly accounted for at the moment due to user specification dependency.
