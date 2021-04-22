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
    
      apt install -y jpegoptim pngquant parallel zenity imagemagick
    
* Arch Linux

      pacman -Syu jpegoptim pngquant parallel zenity imagemagick

Run it with
```bash
compressimages [options] [files] [directories]
-0         use NUL as delimiter for stdin rather than newline
-t INT     threads to use (default: 16)
-p PATH    path to save compressed images (default: $HOME/compressed)
-nh        don't ignore directories starting with . or ..
-depth INT limit find's maxdepth to INT
-cc        convert images to JPG and PNG, compress, and keep the smallest file
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
compressimages .
```
will recursively find and optimize all PNG/JPGs in the current directory, saving them in `compressed/` by default

To open the zenity filepicker, just run
```bash
compressimages
```

To bulk convert all images (including webp,svg,etc) to both JPG and PNG, compress them, and keep the smallest file
```bash
compressimages -cc
```

To read filenames/folders/URLs from a .txt file
```bash
compressimages < images.txt
```
---
For directories, it will scan them for more images.

For files, it will check they are PNG/JPG before continuing to optimize them.

For URLs, it will attempt to match http(s)/sft(p)/file links to images, download them using wget, and add the downloaded file to the queue for testing if it's a JPG/PNG image.

# Benchmark
For the benchmark I created 1,000 unique JPG and PNG images of ~2.0MB each. The script successfully compressed all 2,000 images in under a minute, saving 1.7GB of space total.

https://user-images.githubusercontent.com/17060633/115512209-a05b1b80-a279-11eb-9012-b5ead726597e.mp4

Script to create the unique images
```bash
#!/usr/bin/env bash
for ((i=0; i<1000; i++)); do
	convert -size 1000x1000 xc:gray +noise gaussian "image-$i.png"
	convert -size 4000x1000 xc:gray +noise gaussian "image-$i.jpg"
done
```

# Comparison
#### JPG
Screenshot taken with mpv of a 4K BluRay, uncompressed (7.0MB)
![scarface](https://user-images.githubusercontent.com/17060633/115523415-f3869b80-a284-11eb-8584-c344134fdacc.jpg)

Compressed version (889KB)
![scarface](https://user-images.githubusercontent.com/17060633/115523763-4ceeca80-a285-11eb-870b-c7b820a751dc.jpg)

#### PNG
Transparent image of the Rubik's Cube, uncompressed (79KB)
![rubik's](https://user-images.githubusercontent.com/17060633/115523866-6f80e380-a285-11eb-9138-1a45d7a95735.png)

Compressed version (24KB)
![rubix](https://user-images.githubusercontent.com/17060633/115524047-9c34fb00-a285-11eb-905c-c443bc6c0e93.png)

There are very small differences to the human eye in these examples, yet significant amounts of space was saved. Of course this example isn't 100% accurate (different test types on different images should be performed, like in: https://testimages.org/), but this should hopefully provide a rough idea as to what this script can do thanks to [pngquant](https://pngquant.org/) and [jpegoptim](https://github.com/tjko/jpegoptim).

More useful links:
https://tinypng.com/
https://tinyjpg.com/

## Dependencies
- parallel
- jpegoptim
- pngquant
- zenity (optional)
- bash >= 4.0+

## License
MIT
