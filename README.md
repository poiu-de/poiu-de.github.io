poiu-de site
============

This is the source for the poiu-de.github.io site with the documentation of the poiu.de Java libraries.


Publishing
-----------

To publish the site

1. Clone it
   ```
   git clone git@github.com:poiu-de/poiu-de.github.io
   ```

1. Update the hugo theme (as a git submodule)
   ```
   git submodule update --init --recursive
   ```
2. Make the intended changes
3. Push it
   ```
   git push
   ```
4. Github actions will then built the site with hugo and publish it on github pages.

