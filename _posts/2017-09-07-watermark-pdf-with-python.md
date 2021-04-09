---
layout: post
title:  "Add watermark to your PDF file with Python "
author: Ananta
date: "2017-02-07"
categories: [ mini project, tutorial, Python Tutorial ]
image: "assets/images/coding.jpg"
comments: false
---
## Add watermark to your PDF file with Python

Today I will show you how you can simply add a watermark to all pages within a PDF file.
I'll use PyPDF2

A Pure-Python library built as a PDF toolkit. It is capable of:

* extracting document information (title, author, …)
* splitting documents page by page
* merging documents page by page
* cropping pages
* merging multiple pages into a single page
* encrypting and decrypting PDF files
* and more!

By being Pure-Python, it should run on any Python platform without any dependencies on external libraries. It can also work entirely on StringIO objects rather than file streams, allowing for PDF manipulation in memory. It is therefore a useful tool for websites that manage or manipulate PDFs.

Today we will use it's watermarking capabilities.
Let's jump to the code.

```python
    from PyPDF2 import PdfFileMerger, PdfFileReader, PdfFileWriter

    pdf_file = r'/test1.pdf'
    watermark = r'/logopdf.pdf'
    merged = r'/merged.pdf'

    with open(pdf_file, "rb") as input_file, open(watermark, "rb") as watermark_file:
        input_pdf = PdfFileReader(input_file)
        watermark_pdf = PdfFileReader(watermark_file)
        watermark_page = watermark_pdf.getPage(0)

        output = PdfFileWriter()

        for i in range(input_pdf.getNumPages()):
            pdf_page = input_pdf.getPage(i)
            pdf_page.mergePage(watermark_page)
            output.addPage(pdf_page)

        with open(merged, "wb") as merged_file:
            output.write(merged_file)
```

Though this method works you must not forget this is a mini project and there are a lot of ways to watermark a pdf and most of them are way more advance than this and they might even work better but this is a great exercise to polish your python skills and know more about it.

for more mini projects head over to [Mini Projects](https://gowoogle.com/categories#mini-project)
