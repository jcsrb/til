# Count the number of pages of all PDFs inside a folder

Just the total count
```sh
exiftool -T -filename -PageCount -s3 -ext pdf . | cut -c 9-11 | paste -sd+ - | bc
```

```sh
exiftool -T -filename -PageCount -s3 -ext pdf .
```

returns pdf name and page nr
```
309.pdf	52
818.pdf	12
378.pdf	22
217.pdf	27
```


Sources:
* https://apple.stackexchange.com/questions/198854/shell-command-to-count-pages-in-a-pdf-other-than-pdftk 
* https://stackoverflow.com/questions/926069/add-up-a-column-of-numbers-at-the-unix-shell
