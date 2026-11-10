# Notes on assets
### Notes on assets

To generate the icon (.ico) file, which is shown in a browser tab to the left of the webpage title, run::

    convert -density 512x512 -background transparent stsci_symbol_pantone279.png -define icon:auto-resize=64,48,32,16 -colors 256 stsci_symbol_pantone279.ico


