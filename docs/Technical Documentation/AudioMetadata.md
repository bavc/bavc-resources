---
layout: page
title: Audio Metadata
parent: Technical Documentation
---
When you transfer audio formats with two faces, the transcode script isn’t able to handle uploading the Digital Face 2 details accurately. This issue largely implicates ⅛” audiocassette transfers, but is also applicable to some ¼” audio transfers.

In relation to this article, the transcode engine generally accomplishes the following tasks:
+Extracts metadata from the digital files
+Embeds that information into appropriate Salesforce fields
+Creates derivatives

One major drawback is the inability for the transcode script to distinguish between Face01 and Face02 details when both digital file names share the same BAVC barcode. For example, a typical audiocassette with two faces will produce two .wav files:
+**BAVC1000001**_Face01
+**BAVC1000001**_Face02

The transcode engine will review all information for preservation object BAVC1000001 and mistakenly import Face02 metadata into Digital Face 1 fields. This likely occurs because digital files are organized numerically, with Face02 file details being the last information the transcode engine reads to perform Salesforce import commands.

To fix this issue, you have to manually correct Digital File 1 and Digital File 2 audio metadata details in Salesforce. You can find all the required metadata in the mediainfo.csv file.

#Finding the mediainfo.csv file
You produce a mediainfo.csv file when you run the transcode engine script. The mediainfo.csv file will live in the transcode folder you designated for each item. For example:

PV22_TransferProject → 02_InProgress → 02_Transcode → **SumStereo>Mono** → **mediainfo.csv**

![Alt Text Goes Here](/bavc-resources/assets/images/mediainfocsv-find.png)

##Using *metaHarvest.py*
If you have not located the file, or for any reason no longer have this file available in the SAN, you can run the metaHarvest.py script to regenerate metadata. The script harvests information from chosen digital files to recreate the mediainfo.csv without having to run the transcode engine again.

##(Re)run the Transcode Engine
You can also reproduce a mediainfo.csv file by re-running the transcode engine and creating ‘0’ derivatives. However, running this script again may overwrite details in Salesforce fields and is only suggested in extreme cases, i.e. the above scripts break.

##Using *sfsync.py* - **NOT RECOMMENDED**
Using this script will upload metadata from the mediainfo.csv into Salesforce fields. **It is not recommended to use this script, as Face02 details will be overlooked and/or incorrectly applied in Face01 fields.** Additionally, running this script after using the transcode engine may overwrite important details necessary for the QC process.

#Opening the mediainfo.csv file

Locate the mediainfo.csv file. You can open with the following:

##Numbers
Open with → Numbers. You can set this as your default option.

![Alt Text Goes Here](/bavc-resources/assets/images/numbers-metadata.png)

##Atom
Open Atom to an untitled project. Then, drag the mediainfo.csv file into the window. You will see each digital file’s metadata details displayed in plain text, each field separated by a comma.

![Alt Text Goes Here](/bavc-resources/assets/images/atom-metadata.png)

##TextEdit
Open with → TextEdit. Like Atom, the metadata is separated by commas. You will see in this example that both BAVC1014540_Face01 and BAVC1014540_Face02 each have their own unique details.

![Alt Text Goes Here](/bavc-resources/assets/images/textedit-metadata.png)

With any option above, manually select and enter these metadata fields:
+Digital File Name
+Digital File Duration
+Digital File Size
+Checksum

In Salesforce, Face01 details are filled in the **DIGITAL OBJECT ELEMENTS** section. Face02 details are filled in the **AUDIO FACE 2 FILE DETAILS** section.

# h1 Heading 8-) :->
## h2 Heading
### h3 Heading
#### h4 Heading
##### h5 Heading
###### h6 Heading


## Horizontal Rules

___

---

***


## Typographic replacements

Enable typographer option to see result.

(c) (C) (r) (R) (tm) (TM) (p) (P) +-

test.. test... test..... test?..... test!....

!!!!!! ???? ,,  -- ---

"Smartypants, double quotes" and 'single quotes'


## Emphasis

**This is bold text**

__This is bold text__

*This is italic text*

_This is italic text_

~~Strikethrough~~


## Blockquotes


> Blockquotes can also be nested...
>> ...by using additional greater-than signs right next to each other...
> > > ...or with spaces between arrows.


## Lists

Unordered

+ Create a list by starting a line with `+`, `-`, or `*`
+ Sub-lists are made by indenting 2 spaces:
  - Marker character change forces new list start:
    * Ac tristique libero volutpat at
    + Facilisis in pretium nisl aliquet
    - Nulla volutpat aliquam velit
+ Very easy!

Ordered

1. Lorem ipsum dolor sit amet
2. Consectetur adipiscing elit
3. Integer molestie lorem at massa


1. You can use sequential numbers...
1. ...or keep all the numbers as `1.`

Start numbering with offset:

57. foo
1. bar


## Code

Inline `code`

Indented code

    // Some comments
    line 1 of code
    line 2 of code
    line 3 of code


Block code "fences"

```
Sample text here...
```

Syntax highlighting

``` js
var foo = function (bar) {
  return bar++;
};

console.log(foo(5));
```

## Tables

| Option | Description |
| ------ | ----------- |
| data   | path to data files to supply the data that will be passed into templates. |
| engine | engine to be used for processing templates. Handlebars is the default. |
| ext    | extension to be used for dest files. |

Right aligned columns

| Option | Description |
| ------:| -----------:|
| data   | path to data files to supply the data that will be passed into templates. |
| engine | engine to be used for processing templates. Handlebars is the default. |
| ext    | extension to be used for dest files. |


## Links

[link text](http://dev.nodeca.com)

[link with title](http://nodeca.github.io/pica/demo/ "title text!")

[Link To a Page On This Site]({{ site.baseurl}}/docs/TemplateFolder/TemplateArticle.html) _This link just links back to this page, but notice the use of {{site.baseurl}}_

If you want to link to a specific part of an article, hover your mouse over the Header of that section. A link icon should appear. You can the right-click and copy/paste this URL to link directly to the header of an article.

Autoconverted link https://github.com/nodeca/pica (enable linkify to see)


## Images

Make sure to put any images you want in the `/assets/images/` folder. Then you'll want to use the following format to insert images into a page.

`![Alt Text Goes Here]({{site.baseurl}}/assets/images/BAVCLogoOrange.png)`

![Alt Text Goes Here]({{site.baseurl}}/assets/images/BAVCLogoOrange.png)


## Plugins

The killer feature of `markdown-it` is very effective support of
[syntax plugins](https://www.npmjs.org/browse/keyword/markdown-it-plugin).


### [Emojies](https://github.com/markdown-it/markdown-it-emoji)

> Classic markup: :wink: :crush: :cry: :tear: :laughing: :yum:
>
> Shortcuts (emoticons): :-) :-( 8-) ;)

see [how to change output](https://github.com/markdown-it/markdown-it-emoji#change-output) with twemoji.


### [Subscript](https://github.com/markdown-it/markdown-it-sub) / [Superscript](https://github.com/markdown-it/markdown-it-sup)

- 19^th^
- H~2~O


### [\<ins>](https://github.com/markdown-it/markdown-it-ins)

++Inserted text++


### [\<mark>](https://github.com/markdown-it/markdown-it-mark)

==Marked text==


### [Footnotes](https://github.com/markdown-it/markdown-it-footnote)

Footnote 1 link[^first].

Footnote 2 link[^second].

Inline footnote^[Text of inline footnote] definition.

Duplicated footnote reference[^second].

[^first]: Footnote **can have markup**

    and multiple paragraphs.

[^second]: Footnote text.


### [Definition lists](https://github.com/markdown-it/markdown-it-deflist)

Term 1

:   Definition 1
with lazy continuation.

Term 2 with *inline markup*

:   Definition 2

        { some code, part of Definition 2 }

    Third paragraph of definition 2.

_Compact style:_

Term 1
  ~ Definition 1

Term 2
  ~ Definition 2a
  ~ Definition 2b


### [Abbreviations](https://github.com/markdown-it/markdown-it-abbr)

This is HTML abbreviation example.

It converts "HTML", but keep intact partial entries like "xxxHTMLyyy" and so on.

*[HTML]: Hyper Text Markup Language

### [Custom containers](https://github.com/markdown-it/markdown-it-container)

::: warning
*here be dragons*
:::
