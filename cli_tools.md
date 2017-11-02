# CLI Tools (misc. Mac OSX) - useful notes and commands
----------------------
# Summary
This document contains notes and commands for a misc. CLI tools I find useful on Mac OSX. These notes are written for my own learning and use of these tools. Therefore, they are __primarily things I don't want to forget__ and may have limited relevance to other users.

Tools for which I have created more extensive notes will likely have their own dedicated .md files. If you don't see a particular tool listed below (e.g. `git` or `psql`), check elsewhere in this repository a dedicated .md file.

# Contents

- [7z](#7z)
- [brew](#brew)
- [dot](#dot)
- [exif](#exif)
- [htop/top](#htop/top)
- [jekyll](#jekyll)
- [pandoc](#pandoc)
- [shell](#shell)
- [sips](#sips)
- [textutil](#textutil)
- [vagrant](#vagrant)

# 7z
This is the P7Zip Unix command-line version of the 7-Zip  tool for packing and unpacking compressed file archives.
  - Packing / unpacking: 7z, XZ, BZIP2, GZIP, TAR, ZIP and WIM
  - Unpacking only: AR, ARJ, CAB, CHM, CPIO, CramFS, DMG, EXT, FAT, GPT, HFS, IHEX, ISO, LZH, LZMA, MBR, MSI, NSIS, NTFS, QCOW2, RAR, RPM, SquashFS, UDF, UEFI, VDI, VHD, VMDK, WIM, XAR and Z.

Useful `7z` guide from the National Radio Astronomy Observatory: https://info.nrao.edu/computing/guide/file-access-and-archiving/7zip/7z-7za-command-line-guide

# dot
This is the Graphviz command-line tool for converting .dot text files into diagrams and flowcharts

# exif
This is the ExifTool command-line tool for reading, writing and editing image file metadata including formats EXIF, GPS, IPTC, XMP, JFIF, GeoTIFF, ICC Profile, Photoshop IRB, FlashPix, AFCP and ID3.

Instead of using `find` or `awk` with ExifTool, read the `-ext`, `-if`, `-p` and `-tagsFromFile` options documentation: https://www.sno.phy.queensu.ca/~phil/exiftool/exiftool_pod.html

Common ExifTool Mistakes: https://www.sno.phy.queensu.ca/~phil/exiftool/mistakes.html#M3

# htop/top

`htop` is an interactive system-monitor process-viewer and process-manager. It is designed as an alternative to the Unix program `top`

Unlike `top`, `htop` provides a full list of processes running.

# pandoc

Pandoc can convert documents written in one format to a document written in an entirely different format. Examples include:

  - FROM: markdown, reStructuredText, textile, HTML, DocBook, LaTeX, MediaWiki markup, TWiki markup, OPML, Emacs Org-Mode, Txt2Tags, Microsoft Word docx, LibreOffice ODT, EPUB, or Haddock markup

  - INTO: HTML formats, Word processor formats, Ebook formats, Documentation formats, Page layout formats, Outline formats, TeX formats, PDF via LaTeX, Other lightweight markup formats.

# shell
```sh
echo $PATH
# ...returns $PATH currently in use by Shell

# IMPORTANT ROOT DIRECTORIES
/etc/   # configuration files live here
/var/   # "variable files" that grow or change in size over time
        #     - typically system and app logs are found here
/bin/   # "executable binaries" i.e. the applications you run
/sbin/  # binaries only to be used by root user
/lib/   # libraries that support binaries located around system
/usr/   # "user programs" different from bin in that binaries
        #  within bin are required for bootup and system #  
        #  processes, while usr are not required for that
```


# sips

The sips (scriptable image processing system) command-line tool is used to query or modify raster image files and ColorSync ICC profiles.

File formats include: jpeg, tiff, png, gif, jp2, pict, bmp, qtif, psd, sgi, tga

# textutil

The Text Utility command-line tool can be used to manipulate text files of various formats, using the mechanisms provided by the Cocoa text system. [See Pandoc](#pandoc), a cross-platform alternative, that is capable of doing many more things.

# vagrant
```sh
vagrant up
# ...gets VM up and running
vagrant ssh
# ...connect and log into VM after running 'up'
vagrant suspend
# work is saved and the machine is put into “sleep mode”
vagrant halt
# ...work is saved and machine turned off:
#   - think of it as “turning the power off”
vagrant status
# ...shows current status of VM in current directory:
#   - e.g. “default running (virtualbox)”
vagrant global-status
# ...tells state of all active Vagrant envs on system:
#   - does not actively verify the state of machines
#   - is instead based on a cache
#   - it is possible to see stale results
vagrant global-status --prune
# ...removes invalid entries from global index
vagrant destroy
# ...destroys the VM
#   - work is not saved
#   - think of this as reformatting the hard drive
#   - can use 'up' to relaunch, but will be baseline Linux install
```
