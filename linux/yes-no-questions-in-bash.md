# How to ask a yes-no question in bash

```bash
read -p "Do you want to do the thing? " -n 1 -r
echo    # (optional) move to a new line
if [[ $REPLY =~ ^[Yy]$ ]]
then
    echo "You selected to do the thing!"
fi
```
