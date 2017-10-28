# CLI Tools (misc. Mac OSX) - useful notes and commands
----------------------
# Summary
This document contains notes and commands for a misc. set of CLI tools I find useful on Mac OSX. These are geared toward my own learning and use of these tools. Therefore, they are written for my personal reference (i.e. things I don't want to forget) and may have limited relevance to other users.

Tools for which I have created more extensive notes will likely have their own dedicated .md files. If you don't see a particular tool listed below (e.g. `git` or `psql`), check elsewhere in this repository. If it's a tool I use on a regular basis, it may have it's own dedicated .md file.

# Contents

- [7z](#7z)
- [brew](#brew)
- [dot](#dot)
- [exif](#exif)
- [htop/top](#htop/top)
- [pandoc](#pandoc)
- [sips](#sips)
- [textutil](#textutil)

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

# sips

The sips (scriptable image processing system) command-line tool is used to query or modify raster image files and ColorSync ICC profiles.

File formats include: jpeg, tiff, png, gif, jp2, pict, bmp, qtif, psd, sgi, tga

# textutil

The Text Utility command-line tool can be used to manipulate text files of various formats, using the mechanisms provided by the Cocoa text system. [See Pandoc](#pandoc), a cross-platform alternative, that is capable of doing many more things.
