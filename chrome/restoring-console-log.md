# Restore console to the native function in case it was overwritten

on some website console.log is a noop in order not to leak debug info

![selection_044](https://cloud.githubusercontent.com/assets/119011/16340927/3ef06890-3a33-11e6-8dff-f9b5e8258080.png)

to restore that use the following 

```
var i = document.createElement('iframe');
document.body.appendChild(i);
window.console = i.contentWindow.console;
```

What does this do?

it creates a new iframe, takes the pointer to the native console from inside the new iframe and assigns it to the main window console object

![selection_045](https://cloud.githubusercontent.com/assets/119011/16341168/7bdb887e-3a34-11e6-8d75-b1cc1ac68d42.png)
