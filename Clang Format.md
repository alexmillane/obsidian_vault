Format a bunch of c++ files in a folder:
```bash
find nvblox/ -iname *.h -o -iname *.cpp | xargs clang-format -i --style=file
```
