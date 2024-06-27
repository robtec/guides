# Cheeky Bash Tricks

```
ls -lt | grep Apr | awk '{print $9}' | while read LINE; do sudo rm -rf "${LINE}/" && printf "Deleted ${LINE}\n"; done
```
