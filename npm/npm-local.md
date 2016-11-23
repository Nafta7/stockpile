# How to use package installed locally in node_modules

```bash
alias npm-exec='PATH=$(npm bin):$PATH'
```

then you can:
```
npm-exec <local-module>
```

Taken from: http://stackoverflow.com/questions/9679932/how-to-use-package-installed-locally-in-node-modules
