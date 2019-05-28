# Check the status code of a list of urls with paralel calls

```bash
xargs -n1 -P 10 curl -o /dev/null --silent --head --write-out '%{url_effective}: %{http_code}\n' < url.lst
```

or more data

```bash
xargs -n1 -P 10 curl -o /dev/null --silent --head --write-out '%{url_effective};%{http_code};%{time_total};%{time_namelookup};%{time_connect};%{size_download};%{speed_download}\n' < url.lst | tee results.csv
```



Source 
https://stackoverflow.com/questions/6136022/script-to-get-the-http-status-code-of-a-list-of-urls/6136861
