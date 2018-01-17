# vim


## Add "#" to the beginning of every line
```bash
%s/^/#/
```

## Remove "#" from the end of every line
```bash
%s/#//
```

## Remove first 3 characters no matter what:

```bash
%s/^.\{3}//
```

## Remove first 4 white space characters

```bash
%s/^\s\{4}//
```

## Resize splits windows

```bash
resize 60
```

## Reference
 - [Vim Tips Wiki](http://vim.wikia.com/wiki/Vim_Tips_Wiki)
 - [vim: delete the first 2 spaces for multiple lines](https://stackoverflow.com/questions/6961705/vim-delete-the-first-2-spaces-for-multiple-lines)
