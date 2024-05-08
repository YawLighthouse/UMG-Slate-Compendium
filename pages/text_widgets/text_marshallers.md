<a name="text-marshallers"></a>
# Text Marshallers

---

<a name="table-of-contents"></a>
## Table of Contents

> **[<span>&#8962;</span> Back to Main Compendium Page](../../README.md)**
>
> **[<span>&#11013;</span> Back to Overview Page](text_widget_overview.md)**
> 
> ### Internal Page Links
> 
> - [Plain Text Marshaller](#plain-text-marshaller)
> - [Rich Text Marshaller](#rich-text-marshaller)
> - [Output Log Text Layout Marshaller](#output-log-text-layout-marshaller)

---

<a name="general-description"></a>
## General Description

Text marshallers are classes meant to be subclassed for different raw text types to handle converting to/from text layout's(`FTextLayout`).
Can be used to Get/Set raw text To/From the intended text layout of that marshaller type.

> Text Marshallers are **NOT** intended to parse the raw text, that is intended to be done via decorators(or custom classes that you would insert into your custom marshaller).
> The marshaller is intended to handle *managing* the conversion process from Raw Text to the Converted Text Layout. An example of this is in the [Rich Text Marshaller](#rich-text-marshaller).

[[Wikipedia Description]](https://en.wikipedia.org/wiki/Marshalling_(computer_science)) A reminder about what "marshalling" as a concept is: similar or sometimes synonymous with serialization with the inverse of de-serialization is to "un-marshall" something,
used to describe an overall process of combining and/or resolving data so it can then be transferred over to another object. \
An example would be in physics we run all of our physics overlaps/collisions/etc on the physics thread and then when that thread ticks, it "marshall's"(really resolve's and figures out) those physics interactions to then output the final result to the game thread. \
So essentially the process of marshalling something is to resolve/figure out a bunch of data into a single simple to understand chunk of data.

---

<a name="important-types"></a>
## Important Types

*@TODO Add graph visuals for this section*

- `ITextLayoutMarshaller`: The literal base class as an interface for getting/setting raw text(`FString`) to/from a text layout(`FTextLayout`).
    - `FBaseTextLayoutMarshaller`: The intended base class for all marshallers.
        - `FPlainTextLayoutMarshaller`: Handles conversion between raw text and plain text.
        - `FRichTextLayoutMarshaller`: Handles conversion between raw text and rich text.
        - `FOutputLogTextLayoutMarshaller`: Handles conversion between output log messages(`FOutputLogMessage`) into a stylized lines for text layouts to consume.
- `FTextRange`: Container used to hold the beginning/end indexes within a string, usually used when splitting up a string into multiple strings without duplicating the string. It comprised of two integers for `BeginIndex` and `EndIndex`.
- `FRegexMatcher`: A regular expression pattern matcher in this case, used to match rich text markup elements. IE: `<ElementName AttributeName="AttributeValue">Content</>`.

<a name="important-functions"></a>
## Important functions

`static void FTextRange::CalculateLineRangesFromString(const FString& Input, TArray<FTextRange>& LineRanges)`: Handles iterating through an `FString` and splitting it into 
multiple `FTextRange`'s based on if there is a `\n`, `\r`, or an actual line break by the user using `FChar::IsLinebreak`.
- Line breaks are considered one of these:
  - If the iterated character goes past the line feed(which is how long until it can automatically new line using `TCharBase::CarriageReturn` & `TCharBase::LineFeed`).
  - If the iterated character is the next line(`TCharBase::NextLine`).
  - If the iterated character is a line separator(`TCharBase::LineSeparator`).
  - If the iterated character is a paragraph separator(`TCharBase::ParagraphSeparator`).

---

<a name="plain-text-marshaller"></a>
### Plain Text Marshaller

Class Name: `FPlainTextLayoutMarshaller`

Intended to handle regular text conversion's of `FString` into/from `FTextLayout`.
Can also be used to display the text as a password(hiding the characters with `*`).

#### Conversion to Text Layout from String

*@TODO Add graph visuals for this section*

When converting to `FTextLayout` from `FString`, these steps occur within `FPlainTextLayoutMarshaller::SetText`;

1. Calculate line ranges to display from string using `FTextRange::CalculateLineRangesFromString`(basically outputs to `TArray<FTextRange>`).
2. Iterate through those line ranges;
   1. Create a Slate Text Run(`IRun`) of that iterated line range, this could either be `FSlatePasswordRun` or `FSlateTextRun`; if this marshaller is suppose to display the text as a password `*` or not.
   2. Create the Line Highlight(`FTextLineHighlight`) for this iterated line range, this could be either/both of an underline and strikethrough highlight.
   3. Cache the line highlight and the run.
3. Cache the runs that were created into the text layout(`FTextLayout`).
4. Cache the highlights that were created into the text layout(`FTextLayout`).
5. Return that text layout that has the cached information from earlier steps.

#### Conversion from Text Layout to String

*@TODO Add graph visuals for this section*

Really this calls `FTextLayout::GetAsText` on the inputted `SourceTextLayout` which then uses `FTextLayout::GetAsTextAndOffsets`, so here are the steps that occur within `GetAsTextAndOffsets`;

1. Iterates through `LineModels`(`TArray<FLineModel>`).
   1. Appends a line terminator(`\n` or `\r`) to the end of the previous line since each line model was originally split up via those line terminator's in the conversion from string to text layout.
   2. Iterates through the text runs text from the modal's to the return string value.
   3. -OPTIONAL- If there is a text offset then add an offset between each line model. This is for documents to have larger string results.
      - When applying this offset, this is added to an `TArray<FOffsetEntry>` to contain one entry per line in the document. The array index is the line number and the entry contains the index in the flat string that marks the start of the line, along with the length of the line(not including any trailing `\n` character).

**[<span>&#11014;</span> Back to Top](#table-of-contents)**
<a name="rich-text-marshaller"></a>
### Rich Text Marshaller

Class Name: `FRichTextLayoutMarshaller`

Intended to handle text conversions of `FString` into/from `FTextLayout` that formats it as rich text.

<a name="rich-text-writer"></a>
#### Rich Text Markup Writer

*@TODO Add graph visuals for this section*

You can override the rich text markup writer within Rich Text Blocks by overriding `URichTextBlock::CreateMarkupWriter`.

Class Hierarchy:
- `IRichTextMarkupWriter`: The base interface class that can be inherited from for rich text markup writing.
  - `FDefaultRichTextMarkupWriter`: The default implementation class of rich text markup writer's that handles parsing for *ONLY* markup formatting.

<a name="default-rich-text-writer"></a>
##### Default Rich Text Markup Writer

*@TODO Add graph visuals for this section*

The rich text format that this writer looks for is: `<Name metakey1="metavalue1" metakey2="metavalue2">My fancy Text</>`

The write process(which outputs a `FString`) for the default writer for each rich text line is:

1. Append a line terminator to account for the previous line.
2. Iterate through each run inside that line to append information to the output string
   1. Check if there is a tag in this run
      1. If there is a tag then first append `<` to start the markup text.
      2. Append the tagged run's name(`Name` in the format) as following the correct format.
      3. Iterate through each meta data entry(`TPair<FString, FString>`) to add the tag and value:
         1. Append a space so now the string is `<Name `(sadly GitHub markdown doesn't allow for ending with a space to be visible)
         2. Append the meta data entry's key: `<Name metakey1`
         3. Append `=` and `"`: `<Name metakey1="`
         4. Append the meta data entry's value: `<Name metakey1="metavalue1`
         5. Append `"` to close the value field: `<Name metakey1="metavalue1"`
      4. Append `>` to close this tagged run: `<Name metakey1="metavalue1">`
   2. Append the actual text message(`My fancy Text`).
      - `FDefaultRichTextMarkupWriter::EscapeText` is called; intended to account for this run exiting due to an escape character being used:
        - `&` | `&amp`
        - `"` | `&quot`
        - `<` | `&lt;`
        - `>` | `&gt;`
   3. End this tagged run(if its tagged) by appending `</>` to close it: `<Name metakey1="metavalue1">My fancy Text</>`

<a name="rich-text-parser"></a>
#### Rich Text Parser

*@TODO Add graph visuals for this section*

Class Hierarchy:
- `IRichTextMarkupParser`: The base interface class that can be inherited from for rich text markup parsing.
  - `FDefaultRichTextMarkupParser`: The default implementation class of rich text markup parsing that handles parsing the inputted `FString` to be separated and returned as the parse results(`TArray<FTextLineParseResults>`) and a `FString` stripped of any markup.

<a name="default-rich-text-parser"></a>
##### Default Rich Text Markup Parser

*@TODO Add REGEX visuals for this section*

> This parser heavily uses regex expressions so it is recommended to learn regex expression syntax
> (its like markdown for filtering and finding things within strings for code to understand) for understanding the code.
>
> Regex Patterns used by the default parser:
> - `EscapeSequenceRegexPattern`: `(&quot;)|(&lt;)|(&gt;)|(&amp;)` OR `(&\";)|(&<;)|(&>;)|(&&;)` to account for HTML conversion.
> - `ElementRegexPattern`: `<([\\w\\d\\.-]+)((?: (?:[\\w\\d\\.-]+=(?>\".*?\")))+)?(?:(?:/>)|(?:>(.*?)</>))`
> - `AttributeRegexPattern`: `([\\w\\d\\.]+)=(?>\"(.*?)\")`

The parsing process is done within `FDefaultRichTextMarkupParser::Process` where it will go through these steps:

> We will be using the string: `<Name metakey1="metavalue1" metakey2="metavalue2">My fancy Text</>`, as the example for explaining the steps of this process.

1. Create an `LineRanges`(`TArray<FTextRange>`) variable to cache, modify and use with the output string.
2. Calculate line ranges to display from string using `FTextRange::CalculateLineRangesFromString`(basically outputs to `TArray<FTextRange>`).
3. Parses each line range via [FDefaultRichTextMarkupParser::ParseLineRanges](#default-rich-text-markup-parser--parse-line-ranges)
4. Parses each parsed line result via [FDefaultRichTextMarkupParser::HandleEscapeSequences](#default-rich-text-markup-parser--handle-escape-sequences) to account for escape sequence characters.

<a name="default-rich-text-parser-parse-line-ranges"></a>
###### Default Rich Text Markup Parser :: Parse Line Ranges

*@TODO Add REGEX visuals for this section*

Here is a breakdown of what `ParseLineRanges` does.

1. First creates a `FRegexMatcher` named `ElementRegexMatcher` to handle the parsing.
2. Iterates through each line range:
    1. Creates a `FTextLineParseResults` named `LineParseResults` to cache the range and line run results(which is an array).
        1. The regex matcher is used to handle finding the attributes and meta data of each line that gets broken apart into runs. This regex expression is using `ElementRegexPattern`.
        2. Iterate through the regex's results using `FRegexMatcher::FindNext`.
            1. (Group 1) `<([\w\d\.-]+)`: Caches the captured element name that comes after an opening `<`. which is the `<Name` part of the string.
            2. (Group 2) `((?: (?:[\w\d\.-]+=(?>\".*?\")))+)?`: Finds these sections `metakey1="metavalue1"` & `metakey2="metavalue2"` by capturing `metakey1`|`metakey2` as the attribute name's and `"metavalue1"`|`"metavalue2"` as the attribute value's.
            3. (Group 3) `(?:(?:/>)|(?:>(.*?)</>))`: Finds this section `>My fancy Text</>` and captures `My fancy Text` as the content text between the opening and closing tags.
            4. After these groups are found, we then run the attribute regex expression on the attribute's section(group 2) to iterate through each attribute using `FRegexMatcher::FindNext`. This regex expression is using `AttributeRegexPattern`.
                1. Cache the attribute(stored as a `TMap<FString, FTextRange>`) where it will store the middle of the attribute key and then store the begin/end of the attribute value for the text range.
            5. (If there is a next line to cache) Cache the line's intervening run which is basically intended to link these parsed results together while we're iterating through the line ranges.
            6. Cache the attributes and the content as `FTextRunParseResults` as this line range's run.
   2. (If there is more line ranges to iterate through)Add a dangling line run to account for if we have more line ranges that the regex isn't accounting for(to make sure the string is still linked together properly).
   3. (If we didn't add any run's) Add a blank run since that is more than likely what was inputted in.
   4. Caches the run's that were added this line range iteration into the function's output of parsed results(`TArray<FTextLineParseResults>`).

<a name="default-rich-text-parser-handle-escape-sequences"></a>
###### Default Rich Text Markup Parser :: Handle Escape Sequences

Escape sequences are special characters that allow for you to include characters in a string that wouldn't normally be included directly inside `ParseLineRanges`.
For example; if you wanted to include a `<` symbol within your rich text without it being considered the start of a tag, you could use an escape sequence to have this character be displayed as content text.

This function's job is to account for that AFTER `ParseLineRanges` occurs to adjust the outputted string so any escape sequence characters are taken care of.

Escape Sequence Characters:
- Quotations `"` | `&quot;`
- Left Arrow `<` | `&lt;`
- Right Arrow `>` | `&gt;`
- Ampersand `&` | `&amp;`

> **Example Text**
> 
> Raw Text: `"This is a &quot;quote&quot; &amp; &lt;sample&gt;"`
> 
> Result: `"This is a "quote" & <sample>"`

#### Conversion to Text Layout from String

*@TODO Add graph visuals for this section*

When converting to `FTextLayout` from `FString`, these steps occur within `FRichTextLayoutMarshaller::SetText`;

1. Runs the cached Parser of the marshaller(which occurs when the marshaller is created) to `Process` the inputted string and output the parsed line results as `TArray<FTextLineParseResults>` which can be override via `IRichTextMarkupParser::Process`.
   - The default parser in Unreal Engine is `FDefaultRichTextMarkupParser` as an example.
   - `FTextLineParseResults`: This is basically a container for text range and the parsed text runs(`TArray<FTextRunParseResults>`).
2. Iterates through the parsed lines which basically calls `FRichTextLayoutMarshaller::AppendRunsForText` and then caches it as a line run and cached highlight information to later add to the text layout.
   1. Tries to get the decorator of this marshaller by calling `FRichTextLayoutMarshaller::TryGetDecorator`;
      1. First iterates through any inline decorators and then iterates through regular decorators, for each iteration loop it checks if that decorator supports the text run and string via an overridable `ITextDecorator::Supports`.
   2. If there is a found decorator then it uses that to create the run and update the resulted string via `ITextDecorator::Create`. \
   Otherwise a run is manually via the text run's metadata and created a `FRunInfo` as a safety net;
   3. Iterate through the text run's metadata and add them to the run info.
   4. Apply regular text block style as one of the safety net decorator style set's.
   5. Basically this is converting the same way as plain text marshaller and adds it to the runs to return.
3. Adds the iterated and cached line runs to the text layout.
4. Adds the cached highlight information to the text layout.

#### Conversion from Text Layout to String

*@TODO Add graph visuals for this section*

When converting to `FTextLayout` from `FString`, these steps occur within `FRichTextLayoutMarshaller::GetText`;

1. Creates an empty array of `FRichTextLine` named `WriterLines` to output at the end of this function.
2. Iterates through the Line Models(`TArray<FLineModel>`) retrieved from the inputted text layout.
   1. Creates a single `WriterLine`(`FRichTextLine`), which is intended to hold a single line in a rich-text document that is comprised of multiple(or one) styled runs(rich text runs).
   2. Iterates through each `Runs`(`FRunModel`) to build out a rich text run and caches it for the parent loop's.
      1. Caches each run within `WriterLine` as a `FRichTextRun`.
   3. Caches that modified `WriterLine` into `WriterLines` array.
3. Tells the `Writer`(`TSharedPtr<IRichTextMarkupWriter>`) to write the cached `WriterLines` to output each line with the markup that persists that run layout.

**[<span>&#11014;</span> Back to Top](#table-of-contents)**
<a name="output-log-text-layout-marshaller"></a>
### Output Log Text Layout Marshaller

Since this is used for the output log, this marshaller has a `FCriticalSection` named `PendingMessagesCriticalSection` because output log runs its own "OutputDeviceRedirector" thread 
so this critical section is used to lock the marshaller from converting messages to prevent race conditions.

#### Conversion to Text Layout from String

*@TODO Add graph visuals for this section*

When converting `FTextLayout` from `FString`, these steps occur within `FOutputLogTextLayoutMarshaller::SetText`;

1. Caches `TextLayout` to the `TargetTextLayout`.
2. Resets the `NextPendingMessageIndex` to 0.
3. Calls `FOutputLogTextLayoutMarshaller::SubmitPendingMessages`:
   1. Checks if we can lock the marshaller to avoid submitting additional messages(since we can submit more next tick). If we didn't successfully lock then exit the function.
   2. If the next pending message index is valid then we will append all pending messages to the cached text layout and then update the pending message index to our current count(for next submit request).

#### Conversion from Text Layout to String

*@TODO Add graph visuals for this section*

Really this calls `FTextLayout::GetAsText` on the inputted `SourceTextLayout` which then uses `FTextLayout::GetAsTextAndOffsets`, so here are the steps that occur within `GetAsTextAndOffsets`;

1. Iterates through `LineModels`(`TArray<FLineModel>`).
    1. Appends a line terminator(`\n` or `\r`) to the end of the previous line since each line model was originally split up via those line terminator's in the conversion from string to text layout.
    2. Iterates through the text runs text from the modal's to the return string value.
    3. -OPTIONAL- If there is a text offset then add an offset between each line model. This is for documents to have larger string results.
        - When applying this offset, this is added to an `TArray<FOffsetEntry>` to contain one entry per line in the document. The array index is the line number and the entry contains the index in the flat string that marks the start of the line, along with the length of the line(not including any trailing `\n` character).

**[<span>&#11014;</span> Back to Top](#table-of-contents)**
