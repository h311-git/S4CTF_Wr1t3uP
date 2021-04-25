# [S4CTF 2021](https://s4ctf.peykar.io):bisquite

**Category:** Miscellaneous

**Description:** May we have some bisquite :)

We are given a file callded `bisquite`

## Solution overview

At first point we don't know what the file type is so for this we use a really great tool called [binwalk](https://github.com/ReFirmLabs/binwalk). ok let's run this command to see what it's made of.

```bash
binwalk bisquite
```
The output is:

```bash
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
1099776       0x10C800        Squashfs filesystem, little endian, version 4.0, compression:gzip, size: 11132553 bytes, 11 inodes, blocksize:  131072 bytes, created: 2021-04-22 11:04:00

```

As we can see there is a squashfs filesystem embedded in the file. there might be many solutions for extracting this but we use the binwalk to automatically extract known file types(`-e` flag) so let's do it:

```bash
binwalk -e bisquite
```

As a result binwalk automatically create a directory for extracted files in this case called:`_bisquite.extracted` and if we explore the directory we can see the flag as a jps file:

![imagefile](flag.jps)

## Flag

So the flag is: `S4CTF{r34LLy_L!k3_bisQu!73}`
