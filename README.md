## About
Bash script for bulk optimizing PNG/JPG files, quickly and reliably (includes download function, and GUI filepicker). It uses lossy compression, with barely noticeable differences except for filesize. Similar to tinypng.com.

## Install
Download and install to `/usr/local/bin/`

```bash
wget https://raw.githubusercontent.com/644/compressimages/master/compressimages
sudo install -m 0755 compressimages /usr/local/bin/
```

Install dependencies

* Ubuntu
    
      apt install -y jpegoptim pngquant parallel zenity
    
* Arch Linux

      pacman -Syu jpegoptim pngquant parallel zenity

Run it with
```bash
compressimages [options] [files] [directories]
-0         use NUL as delimiter for stdin rather than newline
-t INT     threads to use (default: 16)
-p PATH    path to save compressed images (default: $HOME/compressed)
-nh        don't ignore directories starting with . or ..
-depth INT limit find's maxdepth to INT
```

## Examples
Read from stdin
```bash
find . -type f -name '*.png' -print0 | compressimages -0 -p output/
```

It can detect whether it's a directory or file, or even a (s)ftp/http(s) URL
```bash
compressimages -depth 2 Downloads/ someimage.png Pictures/ https://website.com/image.png
```

Running
```bash
compressimages .`
```
will recursively find and optimize all PNG/JPGs in the current directory, saving them in `compressed/` by default

Or to open the zenity filepicker, just run
```bash
compressimages
```

Or to read filenames/folders/URLs from a .txt file
```bash
compressimages < images.txt
```
---
For directories, it will scan them for more images.

For files, it will check they are PNG/JPG before continuing to optimize them.

For URLs, it will attempt to match http(s)/sft(p)/file links to images, download them using wget, and add the downloaded file to the queue for testing if it's a JPG/PNG image.

## Dependencies
- parallel
- jpegoptim
- pngquant
- zenity (optional)
- bash >= 4.0+

## License
MIT
