#Exlude a path from apache proxypass

```
ProxyPass /my/excluded/path !
ProxyPass / http://localhost:900/
```
`!` as the proxy target excludes the route from further ProxtPass directives

