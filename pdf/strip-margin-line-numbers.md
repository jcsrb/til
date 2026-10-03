# Strip line numbers from the margin

Transcripts and some legal documents number every 5th line in the left margin:

```
 5   the witness was asked whether
     she had seen the vehicle before
     ...
10   and she said she had not
```

Text extraction puts those numbers into the text (`the witness was asked whether 5`, or a stray `10` paragraph), and tagged PDFs include them as ordinary `P` elements too.

A rule that worked well: collect the bare numbers in the left margin of each page, and drop them only if **all** of them are multiples of 5:

```ruby
side_numbers = items.select { |i| i.text.strip.match?(/\A\d+\z/) && i.x < left_margin }
if side_numbers.all? { |i| (i.text.to_i % 5).zero? }
  side_numbers.each(&:remove)
end
```

The "all of them" check keeps real content safe: a page that also has a `3` or `17` in the margin has numbered paragraphs, not line numbers.

Check with `% 5`, not a hard-coded list: the first version listed `5 10 … 30`, so on longer pages the `35` failed the check and every line number stayed in.
