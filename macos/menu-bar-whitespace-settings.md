
# Menu Bar Whitespace Settings 


```bash
# Change the whitespace settings value
defaults -currentHost write -globalDomain NSStatusItemSelectionPadding -int 6
defaults -currentHost write -globalDomain NSStatusItemSpacing -int 6

# After running these commands, you need to log out and log back in
```

Reverting to the Original Values

```bash
# Revert to the original values
defaults -currentHost delete -globalDomain NSStatusItemSelectionPadding
defaults -currentHost delete -globalDomain NSStatusItemSpacing

# After running these commands, you need to log out and log back in
```

source
https://flaky.build/native-fix-for-applications-hiding-under-the-macbook-pro-notch
