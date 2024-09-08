# cryptSetup 


```bash
cryptsetup luksFormat --type luks2 myblock
cryptsetup luksDump myblock

```

```bash
cryptsetup luksOpen myblock myPart1
ll /dev/mapper/myPart1
mkfs.exfat /dev/mapper/myPart1
mkdir hd
mount /dev/mapper/myPart1 ./hd
cd hd
date > hello.txt

```


