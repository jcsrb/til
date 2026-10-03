# Text coordinates ignore the page's `/Rotate`

A PDF page can carry `/Rotate 90` (or 180, 270): the viewer turns the page when displaying it, but the content stream stays as it was. Text extractors that give you glyph positions (pdf-reader in Ruby, pdfminer, …) report them in that **unrotated** space.

So on a page with `/Rotate 90`, what reads as a horizontal line on screen runs vertically in those coordinates, and anything that sorts by `y` to rebuild lines and paragraphs produces garbage.

Turn the coordinates yourself, the way the viewer does (Ruby, pdf-reader):

```ruby
def apply_rotation(x, y, rotate)   # rotate = page.rotate
  case rotate
  when 90  then [y, -x]
  when 180 then [-x, -y]
  when 270 then [-y, x]
  else          [x, y]
  end
end
```

The results can be negative; that's fine when you only need the reading order. Add the page width/height back if you need real positions.

Related: [page-size-like-pdfinfo](page-size-like-pdfinfo.md), `pdfinfo` doesn't swap width and height for `/Rotate` either.

Sources:
* PDF 1.7 spec, 7.7.3.3 "Page Objects" (`Rotate`)
* https://github.com/yob/pdf-reader
