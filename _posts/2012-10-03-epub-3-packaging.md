---
layout: post
title: "EPUB 3 Packaging"
tagline: "tag"
description: ""
category: 
tags: []
---
{% include JB/setup %}

I was creating an [ebook](https://github.com/fcovas/recipes.epub) without using specific tools, I wanted to edit all the files myself to learn a little more about the EPUB file structure. For reference this is the structure of my work directory:
    
    root
        book
            mimetype
            META-INF
            Content
        deploy
            recipes.epub

 After creating the initial structure I wanted to package it all into one zip file in order to load it on my ebook reader. This post is the result of a series of problems I had:

__mimetype file__  
The *mimetype* file contains the mime type of the compressed epub, in this case *application/epub+zip*. This file needs to be the very first one in the zip package. I'm using the [zip utility](http://amath.colorado.edu/computing/software/man/zip.html) so it's easy to specify the order in wich the files are packaged. 

    zip recipes.epub mimetype META-INF Content

If you are using some graphical interface applications to generate the zips you need to watch out for this.

__Filenames__  
I was calling the zip utility like this:

    zip deploy/recipes.epub book/mimetype book/META-INF book/Content

This generated the names in the zip file having book as parent. You can see the zip filenames using the -vl arguments:

    unzip -vl deploy/recipes.epub

Running the command from the folder where the book is solved that issue.

__Unwanted files__  
When you create the zip all the files in the folders and subfolders will be added. To avoid system files from being added to it you can use the _-X_ flag. If you want to be more specific in what files to exclude you can use the _-x_ flag, this allows you to pass a series of wildcards or the name a file with the list of files to ignore. I'm using both flags:

    zip -X recipes.epub . -x@exclude.lst

The final command used to package the ebook looks like this:

    cd book  
    zip -Xr ../deploy/recipes.epub mimetype META-INF Content -x@../exclude.lst  

__Note__:  
If want to validate your epub file you can go [here](http://validator.idpf.org/).  

__References__  
<http://idpf.org/epub/30/spec/epub30-ocf.html>
<http://amath.colorado.edu/computing/software/man/unzip.html>
<http://amath.colorado.edu/computing/software/man/zip.html>

