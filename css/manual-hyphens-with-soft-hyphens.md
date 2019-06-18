# Hyphenation rules differ from language to language

`hyphens: auto` tries its best but there is also `hyphens: manual;`

```html
<style>
p {
  hyphens: auto;
  width: 35px;
  border: 2px solid silver;
}

p.manual {
  hyphens: manual;
}
</style>

<p class="en">
  awesome
</p>
<p lang="it">
  meraviglioso
</p>
<p lang="fr">
  merveilleux
</p>
<p class="it manual">
  me&#173;ra&#173;vi&#173;glio&#173;so
</p>
```


Source:
* https://twitter.com/WebReflection/status/1140706975480733697
* https://codepen.io/WebReflection/pen/BgzpKG
