# Text you add to a PDF is extracted in content-stream order, not where you put it

Some things on a page have no text at all, e.g. a checkbox or a black box drawn as a vector shape. To get a marker for them into the extracted text, the obvious idea is to add invisible text (say `[BOX]`) at each shape's position, then extract.

Doesn't work. Extractors like pdftext output characters in **content-stream order**, and new text gets appended to the end of the stream. All the labels pile up at the end of the page.

What works: extract first, then splice the labels in using the geometry.

* find the shapes with PyMuPDF: `page.get_drawings()`, e.g. filled rects of text-line height
* for each shape, find the extracted line whose box overlaps it
* count the characters left of the shape on that line and insert the label there
* insert right-to-left on each line so earlier positions stay valid

Sources:
* https://pymupdf.readthedocs.io/en/latest/page.html#Page.get_drawings
