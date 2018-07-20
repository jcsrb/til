# Quick stress test any curl command


```sh
mycurl() {
    START=$(date +%s)
    curl -o /dev/null -s -w "%{http_code}\n" -F "pdf=@/Users/jakob/Downloads/TC06576.pdf" http://localhost:4040/pdf2txt 
    END=$(date +%s)
    DIFF=$(( $END - $START ))
    echo "It took $DIFF seconds"
}
export -f mycurl

seq 100 | parallel -j0 mycurl
```
