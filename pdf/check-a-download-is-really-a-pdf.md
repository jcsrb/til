# Check a downloaded "PDF" really is a PDF

A server can answer a PDF URL with `HTTP 200`, `Content-Type: text/html` and a ~4 KB bot-check page (e.g. Anubis). Save that as `.pdf` and every later step fails in confusing ways.

Check the magic bytes, not the status code or the file extension:

```sh
head -c 5 file.pdf    # must print %PDF-
```

```ruby
File.binread(path, 5) == "%PDF-"
```

Sources:
* https://github.com/TecharoHQ/anubis
