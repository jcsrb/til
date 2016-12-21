# Create a PDF from command line

```
i=150; convert page1.png page2.png -compress jpeg -quality 70 \
      -resize $((i*827/100))x$((i*1169/100)) -density ${i}x${i} \
      -repage $((i*827/100))x$((i*1169/100)) multipage.pdf
```

where `i` is the density in DPI
values for resize and repage in this example are for A4 pages size

`convert` is part of [imagemagick](http://www.imagemagick.org/)
