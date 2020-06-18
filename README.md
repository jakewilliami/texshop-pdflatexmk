TexShop pdflatexmk Command [IN PROGRESS]
========================

TexShop is a program for Mac OS X which compiles LaTex files (.tex) into pdf files. When it does this, by default it created a .log and .aux file for every source file in your LaTeX document, this gets annoying with large projects.

This repository holds a fork from [texshop-pdflatex](https://github.com/marcuswhybrow/texshop-pdflatex) for `pkflatexmk`, which allows the same concept to be used with bibliographies.

Installing the Script
---------------------

You can put the script anywhere, but the solution I am following suggests, putting it here:

    /Library/TeX/Root/bin/pdflatexmk.py

Make sure that everyone has executable permissions on the file, if TexShop cannot execute the file it will complain:

    sudo chmod +x /Library/TeX/Root/bin/pdflatexmk.py

Next change TexShop's preferences to use this script instead, go to **Preferences** and then the **Engine** tab. Under the section entitles **pdfTeX** change the **Latex (default: pdflatex)** box to use the new script.

For example if before it looked like this:

    pdflatex --file-line-error --shell-escape --synctex=1

And afterwards it should look like this:

    /Library/TeX/Root/bin/pdflatexmk.py --file-line-error --shell-escape --synctex=1

Any arguments passed to the new `pdflatex.py` script will be passed on the real `pdflatex` command which is subsequently called.

Setting the Output Directory
----------------------------

By default all generated files will appear in `/tmp/`, you can override this in the standard `pdflatex` command using the `--output-directory=/path/to/location` argument.

All that the new `pdflatexmk.py` script does is call the original `pdflatexmk` command and then move the pdf file back to the directory which the root .tex file is in.

As an example, if you wanted to have all of the output files put into a `build` directory next to the root text file, then just add `--output-directory=build` to argument list in the TexShop preferences for pdflatex.py command.
