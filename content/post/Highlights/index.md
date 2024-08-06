---
title: Highlighs
description: Pdf highlights extractor
slug: Highlights 
date: 2024-06-30 00:00:00+0000
image:
categories:
    - Tech
tags:
    - fun
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
draft: True
---

Recently, I've started to read a self-help book. I've highlighted the parts that
I know are useful for future references. Then I had an epiphany. If I have
everything highlighted in one place that would be cool rather than scrolling
through entire pdf. Google books offers the same feature if we buy books from
play store.

## Code

```python

pip install pymupdf

import os
import fitz

def extract_highlights(pdf_path, output_path):
    doc = fitz.open(pdf_path)
    highlights = []

    for page_num in range(len(doc)):
        page = doc.load_page(page_num)
        annot = page.first_annot
        while annot:
            if annot.type[0] == 8:  # Highlight
                highlight_text = ""
                quads = annot.vertices
                for i in range(0, len(quads), 4):
                    quad = quads[i:i + 4]
                    rect = fitz.Quad(quad).rect
                    words = page.get_text("words", clip=rect)
                    for word in words:
                        highlight_text += word[4] + " "
                highlights.append((page_num + 1, highlight_text.strip()))
            annot = annot.next

    with open(output_path, "w") as f:
        for page_num, text in highlights:
            f.write(f"Page {page_num}:\n{text}\n\n")

    print(f"Highlights extracted and saved to {output_path}")

pdf_path ="/Users/venkatasreerampallapothula/Documents/The-Happiness -Equation.pdf"
output_path = os.path.expanduser("~/highlight.txt")

extract_highlights(pdf_path, output_path)
```

![Image 1](hi.png) 

## Code explation

This function takes two arguments: pdf_path (path to the input PDF file) and output_path (path to the output text file).
fitz.open(pdf_path) opens the PDF document.
An empty list highlights is initialized to store the extracted highlights.

The loop iterates through each page in the PDF document.
doc.load_page(page_num) loads the current page.
page.first_annot retrieves the first annotation (if any) on the page.

The while loop iterates through all annotations on the current page.
annot.type[0] == 8 checks if the annotation is a highlight (type 8).
An empty string highlight_text is initialized to store the text within the highlight.
annot.vertices retrieves the vertices (coordinates) of the highlight quadrilateral.

The inner for loop processes each quadrilateral (set of 4 vertices) of the highlight.
fitz.Quad(quad).rect converts the quadrilateral to a rectangle.
page.get_text("words", clip=rect) retrieves all words within the rectangle.
The nested for loop concatenates each word's text (the fifth element in the word tuple) to highlight_text.

The highlight_text along with the page number is appended to the highlights list.
annot = annot.next moves to the next annotation on the page.

The output file is opened in write mode.
Each highlight is written to the file with the page number and the corresponding text.

pdf_path specifies the path to the input PDF file.
output_path specifies the path to the output text file, expanded to the user's home directory.
The extract_highlights function is called with the specified paths.


Before writing each highlight to the file, the script writes the page number.
After each highlight, a blank line is added to separate highlights for better
readability.

Now, all the highlights are extracted to Highlights extracted and saved to
_"/Users/venkatasreerampallapothula/highlight.txt"_ file.
`
