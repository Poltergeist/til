# Remove change from last git commit

Sometimes when one added a change to a commit which they did by accident it is tedious to remove the change again from the commit. Infact there is a easy way to remove a file from a commit and put it again into the staged area. then you can change the file and add it to the commit.

```bash
git checkout commit-id <file>
```
