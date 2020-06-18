TexShop pdflatexmk Command
========================

TexShop is a program for Mac OS X which compiles LaTex files (.tex) into pdf files. When it does this, by default it created a .log and .aux file for every source file in your LaTeX document, this gets annoying with large projects.

This repository holds a fork from [texshop-pdflatex](https://github.com/marcuswhybrow/texshop-pdflatex) for `pkflatexmk`, which allows the same concept to be used with bibliographies.

Installing the Script
---------------------

You can put the script anywhere, but the solution I am following suggests, putting it here in `/Library/TeX/Root/bin/`.  You will need to make sure that everyone has executable permissions on the file. If TeXShop cannot execute the file it will complain.  To do this via the command line, simply run
```
cd /Library/TeX/Root/bin/ && \
curl -s https://raw.githubusercontent.com/jakewilliami/texshop-pdflatexmk/master/pdflatexmk.py | sudo tee pdflatexmk.py > /dev/null && \
sudo chmod +x pdflatexmk.py && \
cd - > /dev/null && \
echo -e "\u001b[1;38;5;2mLaTeX compiler \`pdflatexmk.py\` installed successfully.\u001b[0;38m"
```
Next, you will need to ensure TeXShop has a way to access this script.  To do this, you need to create a new engine:
```
cd ~/Library/TeXShop/Engines/ && \
curl -s https://raw.githubusercontent.com/jakewilliami/texshop-pdflatexmk/master/pdflatexmk.py.engine > pdflatexmk.py.engine && \
sudo chmod +x pdflatexmk.py.engine && \
cd - > /dev/null && \
echo -e "\u001b[1;38;5;2mLaTeX compiler \`pdflatexmk.py.engine\` installed successfully.\u001b[0;38m"
```

Now you need to **restart TeXShop** and enter the following [shebang](https://www.wikiwand.com/en/Shebang_(Unix)) at the top of your `.tex` project:
```
% !TEX TS-program = pdflatexmk.py
```

And everything will now work!  


Adjusting Shell Options
-----------------------

The default options for this script to be run with is
```
--file-line-error --synctex=1 --enable-write18 "${1}"
```
To change these you will need to edit the file you just downloaded, at `~/Library/TeXShop/Engines/pdflatexmk.py.engine`.


Setting the Output Directory
----------------------------

By default all generated files will appear in `/tmp/`, you can override this in the standard `pdflatex` command using the `--output-directory=/path/to/location` argument.

All that the new `pdflatexmk.py` script does is call the original `pdflatexmk` command and then move the pdf file back to the directory which the root .tex file is in.

As an example, if you wanted to have all of the output files put into a `build` directory next to the root text file, then just add `--output-directory=build` to argument list in the TexShop preferences for pdflatex.py command.
