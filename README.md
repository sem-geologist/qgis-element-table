# Chemical Element table Widget for QGIS

This plugin adds docking widget with chemical element table to QGIS for easier change of visibility (check state) of the layers which have chemical meaning (contains name or abbreviation of element in the name).
The button which shows the widget is placed to mini toolbar of TOC (Layers widget) of QGIS as this extends TOC tools for chemical layers.

## Why?

The interaction with layers through TOC can be seen robust enough for most of the GIS workflows.
However, imagine if you are working with geochemical layers (i.e. soil chemistry for 40-60 elements) and you only want to glance if there are any visual correlation between geologically/ geochemically associated elements (V with Ti, Hf with Zr, Sr with Ca, Cd with Cu...).
In TOC You would need to find those elements in the list/tree and check/ uncheck, and lists are sorted in a single dimension (alphabetically), which positions entries with layers associated in geochemical processes far apart in the list.
Probably it would be possible to sort list with atomic number, but that would not be practical as similarities of chemical properties works both by group and period (in two dimensions).
Thus the only robust and efficient way to switch between layers with geochemical data is through periodic element table, which this widget implements.

## How does it work?

This plugin uses extensively the small but robust [`qpet`](https://github.com/sem-geologist/qpet) library.
`qpet` gives periodic element table widget and function to recognize chemical elements (names or abbreviation) in a string.
This plugin consist mostly of boilerplate code (made partly with plugin-builder 3), adapters and functions to interconnect the signals from QGIS (such as Project.legendLayerAdded, layerTreeRoot.visibilityChanged...)
to sync the state of element table widget, and other way around ((un-)checking the layers when element button in the widget is (un-)checked).
Thanks to cleverly designed API of QGis, this plugin works out-of the box with layers placed in "Mutually exclusive groups" in TOC.

## How to test it?

When plugin is started and there is no layer which would contain name or abbreviation of chemical element, all buttons of element table is disabled. When project is loaded or if layers are added to current project which contain element information in name, buttons for those elements will be enabled. Only buttons which corresponding element was found in layers are enabled.
When last for given element layer is removed from TOC, the corresponding button in element table is unchecked and disabled.
To see this plugin in action UK Soil geochemical WMS maps can be used. Often it is practical to put such layers into Group and mark as "Mutually exclusive group".

## For bug report and issues
For adding the language support (dictionary of element names and abbreviations in wished language) please open the issue at [`qpet`](https://github.com/sem-geologist/qpet) repository.
For functionality issues of this plugin please open issue or pull request at [`qgis-element-table`](https://github.com/sem-geologist/qgis-element-table) repository.

## Future Ideas:
Currently the plugin works with new layers added/removed to the TOC if they contain the name/abbreviation of element.
This works nice with singleband raster types (WMS, raster files).
The grid type of geochemical information is rare and expensive, more often measurements are done at non uniform grid, or with very varying density.
Such data is much better represented with vector type.
It would be handy that this widget could change which attribute(s) (with geochemical information) should be visualised from the selected vector layer.

## Know bugs and limitations:
If widget width is very small (the same size as other docking widgets on the left), the element table texts (element abbreviations) on the buttons are cropped.
