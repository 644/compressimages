## Description
Bash script for bulk optimizing PNG/JPG files. It uses lossy compression, with barely noticeable differences except for filesize. Similar to tinypng.com.

## Installation & Usage
Download and give file executable permissions for user

    wget https://raw.githubusercontent.com/644/compressimages/master/compressimages && chmod u+x compressimages

Copy to $PATH
    
    sudo cp compressimages /usr/local/bin/

Install dependencies

* Ubuntu
    
      apt install -y jpegoptim pngquant parallel
    
* Arch Linux

      pacman -Syu jpegoptim pngquant parallel

Run it with

    compressimages [options] [files] [directories]
    -0            use NUL as delimiter for stdin rather than newline
    -nh           don't ignore directories starting with . or ..
    -p PATH       path for compressed images
    -t INT        threads to use (default: 16)
    -depth INT    limit find's maxdepth to INT

## Examples
    find . -type f -name '*.png' -print0 | compressimages -0 -p output/

It can detect whether it's a directory or file

    find . | compressimages -depth 2 Downloads/ someimage.png Pictures/
    
Or just run `compressimages` to find and compress all PNG/JPGs in the current directory, saving them in ./compressed/
It's a good idea to [add the script to your PATH](https://askubuntu.com/questions/97897/add-bash-script-folder-to-path/97899#97899), so you can run it from anywhere.
    
For directories it will scan them for more images.
For files it will check they are PNG/JPG before continuing to optimize them.

## Dependencies
parallel, jpegoptim, pngquant, bash >= 4.0+

## License
MIT
