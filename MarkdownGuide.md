# How Markdown works
When talking about *how Markdown works*, we are actually talking about how Markdown applications work. Well, it mainly consists of three steps, **Markdown Document** => **Markdown application renders Markdown into HTML** => **Display the rendered HTML**. 

In this *Markdown* guide, we are focusing on the first step, the remaining two steps are not something we really need to think about when writing a *Markdown* document.

---
# 1 Basic syntax
Basic syntax are supported on almost all *Markdown* applications. And of course, *Markdown* would be useless if you don't even know these basic syntax.
## 1.1 Headings
```
# Heading 1
## Heading 2
...
###### Heading 6
```

Alternatively, on the line below the text, add any number of `==` characters for **Heading 1** or `--` for **Heading 2**
## 1.2 Paragraphs
```
Paragraphs doesn't need any hint to start with.

But remember to separate paragraphs with a blank line.
```

***DO NOT INDENT PARAGRAPHS with spaces or tabs***

To create line breaks, end a line with **two or more spaces**
## 1.3 Emphasis
```
*italic*  
_italic_

**bold**  
__bold**

***bold and italic***  
___bold and italic___  
__*bold and italic*__  
**_bold and italic_**
```

Result:
> *italic*  
> _italic_
>
> **bold**  
> __bold__
>
> ***bold and italic***  
> ___bold and italic___  
> __*bold and italic*__  
> **_bold and italic_**  
## 1.4 Blockquotes
```
> Paragraphs following a '>' is a blockquote
```

This is the very basic usage of blockquotes, only one paragraph. You can also use *blockquotes* with **multiple paragraphs**, **other elements**, and **nested blockquotes**, which are illastrated below

```
> The fist paragraph
> 
> The second paragraph. See? Two paragraphs are seprated by a "blank blockquote", but still, they are both in the same blockquote
>
> ### A heading in a blockquote
>
> - bullet list, item 1
> - and, item 2. See? No "blank blockquote" mess up with bullet list.
>
> And **emphasized** text
```

It will look like this:
> The fist paragraph
> 
> The second paragraph. See? Two paragraphs are seprated by a "blank blockquote", but still, they are both in the same blockquote
>
> ### A heading in a blockquote
>
> - bullet list, item 1
> - and, item 2. See? No "blank blockquote" mess up with bullet list.
>
> And **emphasized** text
## 1.5 Lists
You can orgnize items into **ordered** or **unordered** lists
### 1.5.1 Ordered lists
```
1. First item
2. Second item
4. Haha! You must think I made a mistake
3. But the number doesn't really matter, except the fist one
7. And the order of the output list will still be rendered as 1, 2, 3, etc.
```
Result:
> 1. First item
> 2. Second item
> 4. Haha! You must think I made a mistake
> 3. But the number doesn't really matter, except the fist one
> 7. And the order of the output list will still be rendered as 1, 2, 3, etc.

The **FIRST** item should **ALWAYS** start with **`1.`**
### 1.5.2 Unordered lists
```
- An unordered list
- Can start with a '-'
* Or a '*'
+ Or a '+'
- And the resulting view are the same
```
Result:
> - An unordered list
> - Can start with a '-'
> * Or a '*'
> + Or a '+'
> - And the result view are the same

***NOTE*** | In some Markdown applications, unordered list items starting with a '\*' or '\+' has wider line height.
### 1.5.3 Nested lists
```
1. First item
2. Here comes the nested list
    - Nested lists must be indented with 4 SPACES or 1 TAB
    * Second item
3. End of list
```
### 1.5.4 Nesting other elements
```
- I'm going to nest other elements into a list item
    > A Blockquote, indent it with 4 SPACES or 1 TAB

    A paragraph, same rule

    ![same thing with images](images/magic.png)

    ```
    <p>And code blocks, too</p>
    ```

    `<h2>And inline code</h2>`
```
Result:
> - I'm going to nest other elements into a list item
>     > A Blockquote, indent it with 4 SPACES or 1 TAB
>
>     A paragraph, same rule
>
>     ![same thing with images](images/magic.png)
>
>     ```
>     <p>And code blocks, too</p>
>     ```
>
>     `<h2>And inline code</h2>`
## 1.6 Code
You can use single tick mark (\`) to *enclose* inline code.

And you can indent text with **4 spaces** or **1 tab** to indicate it's a *code block*.

Or, you can also *enclose* block code with treble tick marks (\`\`\`), *But this is an extended syntax feature which I will explain later*

If you want to escape tick marks in your word or phrase, *enclose* the word or phrase in double tick marks (\`\`).
## 1.7 Horizontal rules
Horizontal rules are horizontal lines split the content of a document.
```
---
To create a horizontal rule.
****
Use 3 OR MORE
_______
"`" (tick marks), "*" (asterisks), or "_" (underscores)
```
## 1.8 Links
```
[Example](https://example.com "The title is enclosed in quotes")
[email](who@example.com "And it appears when you hover you cursor above the link")
<https://example.com>
<who@example.com>
```

Result:
> [Example](https://example.com "The title is enclosed in quotes")
>
> [email](who@example.com "And it appears when you hover you cursor above the link")
>
> <https://example.com>
>
> <who@example.com>

You can also emphasize links:
```
**[Example](./AnotherMarkdown.md "And a title")**
*[Example](./AnotherMarkdown.md "And a another title")*
```

> **[Example](./AnotherMarkdown.md "And a title")**
>
> *[Example](./AnotherMarkdown.md "And a another title")*

### ***Appendix*** | Reference-style links
Reference-style links consists of two parts:
1. The text you put inline which you can see when it renders to HTML
2. The *reference link* at some other place of the Markdown file
```
I will give you a [link][1], and its reference link somewhere else, for example, just below this paragraph

[1]: (https://example.com "example")
```

> I will give you a [link][1], and its reference link somewhere else, for example, just below this paragraph
> 
[1]: (https://example.com "example")

You can also write your reference link this way:
```
[link][a]
[link] [A]
[link][A]

[a]: (https://example.com "example")
[link]: [https://example.com "example"]
```
> [link][a]
>
> [link] [A]
>
> [link][A]

[a]: (https://example.com "example")
[link]: [https://example.com "example"]
## 1.9 Images
```
![alt text, displays when the image dose not exist](./images/sample.jpg "title, text appears when you hover you cursor above the image")
```
## 1.10 Escaping characters
Use a back slash to escape special characters.  
Characters you can escape:
| Character | Name         | Character | Name       |
| :-------: | ------------ | :-------: | --------- |
| \\        | backslash    | \(\)      | parentheses |
| \`        | tickmars     | \#        | hashtag |
| \*        | asterisk     | \+        | plus sign |
| \_        | underscore   | \-        | dash |
| \{\}      | curly braces | \.        | dot |
| \[\]      | brackets     | \!        | exclamaion |

***
# 2 Extended syntax
Note that *extended syntax support* varies from different Markdown applications.
## 2.1 Tables
```
| Table title 1   | Table title 2   |
| --------------- | ---     |
| row 2, column 1 | row 2, column 2 |
| row 3, column 1 | row 3, column 2 |
| Hi! | Bye! |
```
Result:
| Table title 1   | Table title 2   |
| --------------- | ---     |
| row 2, column 1 | row 2, column 2 |
| row 3, column 1 | row 3, column 2 |
| Hi! | Bye! |

**\! Note**: 
- The extra spaces are ignored
- Use at least three hyphens (\-\-\-) to seperate table title and table content  
    The hyphen line is a must when creating a table
- If you want to escape pipe characters (|) in a table, substitute the pip character with `&#124`
### 2.1.1 Alignment
```
| Align to left | Align to center | Align to right |
| :---          | :---:           | ---:           |
| left          | center          | right          |
```
Result:
| Align to left | Align to center | Align to right |
| :---          | :---:           | ---:           |
| left          | center          | right          |
### 2.1.2 Formatting text in tables
You can add **emphasis**, **inline code** (Not code blocks), **links** into your table.

***But*** you can not add **headings**, **lists**, **horizontal rules**, **images**, or **HTML tags**.

## 2.2 Fenced code blocks
As mentioned earlier in this guide, you can enclose your *code blocks* with treble ticks (\`\`\`). In addition, you can also enclose your *code blocks* with treble tilde (~~~).
### Syntax Highlighting
    ```json
    {
        "firstName": "Kilian",
        "lastName": "Weiss",
        "age": 21,
        "married": false
    }
    ```

Result:
```json
{
    "firstName": "Kilian",
    "lastName": "Weiss",
    "age": 21,
    "married": false
}
```
## 2.3 Footnotes
```
Here's a simple footnote[^1], and here's a longer one[^bignote].

[^1]: This is the first footnote.
[^bignote]: Here's one with multiple paragraphs and code, but not be supported on all markdown applications.
    Indent paragraphs to include them in the footnote.
    
    `{ my_ code }`

    Add as many paragraphs as you like.
```
You can put your footnotes anywhere in the file, and the result is that the footnotes will always appears at the end of the document.

Result:
Here's a simple footnote[^1], and here's a longer one[^bignote].

[^1]: This is the first footnote.
[^bignote]: Here's one with multiple paragraphs and code, but not be supported on all markdown applications.
    Indent paragraphs to include them in the footnote.
   
   `{ my_ code }`

   Add as many paragraphs as you like.

## 2.4  Definition lists
~~~
First term
: This is the definition of the first item

Second term
: This is one definition of the second term
: This is another definition of the second term
~~~
First term
: This is the definition of the first item

Second term
: This is one definition of the second term
: This is another definition of the second termesult:
## 2.5 Task lists
```
- [x]First task
- [ ]Second task
- [ ]Third task
```
R
- [x]First task
- [ ]Second task
- [ ]Third taskesult:
## 2.6 Strikethrough
```
The world is ~~flat~~ round.
```
Result:
> The world is ~~flat~~ round.
## 2.7 Automatic URL linking
Many markdown processors automatically turn URLs into links, although didn't enclose them in parentheses. If you don't want your URLs automatically render to links, you can render them as inline code.