# Inplace replace windows line endings with sed

Today I found out how I can replace the windows line endings with unix line endings using sed
```sed -i.bak 's/\r$//' file.txt```
the `-i.bak` will enable in place substitudes and write the old file to `{{filename.bak}}` as a backup.
