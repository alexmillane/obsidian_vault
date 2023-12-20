
## Disk Usage
Space used by a folder "disk used"
```bash
du -sh .
```
Space free on a device "disk free"
```bash
df -h .
```

## Less
For viewing long files
```bash
less my_long_log.txt
```
> Great thing is that you can use vim bindings to navigate!

You can also pipe things to it
```bash
my_command_producing crap | less
```

## Find

```bash
find . -name "foo*"
```

## Dpkg
Sometimes I have issues with 
```bash
apt --fix-broken install
```