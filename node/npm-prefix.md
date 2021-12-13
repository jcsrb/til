## NPM `--prefix` option

if you use phoenix or other frameworks that have the assets in a subfolder but and you don't want to `cd` into it 
```
cd assets
npm install
cd ..
```

you can use npm's `--prefix` option

```
npm install --prefix ./assets
```
