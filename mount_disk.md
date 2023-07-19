1. Make new directory 

```bash
sudo mkdir /NEW_DIR
```

2. Open `/etc/fstab` file with `root` permissions 

```bash
sudo vim /etc/fstab
```

And add following to the end of the file:

```bash
/dev/DISK    /NEW_DIR    ext4    defaults    0    0
```

3. Mount partition

```bash
sudo mount /NEW_DIR
```

