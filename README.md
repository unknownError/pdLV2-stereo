pdLV2-stereo
============

makes simple Puredata (pd) patches useable as LV2 plugins. - linux only at the moment. 

features
========
* stereo in and output
* 5 control inputs

can be used to design simple audio effects or synthesizers which can be controlled by parameters (no midi yet).


workflow
========

create a .pd patch, specify everything important inside that patch, save it.
make: creates an LV2 bundle (directory containing the pd patch a .so file and some extra files necessary for the LV2 standard)
make install: copies the LV2 bundle to ~/.lv2
start your plugin host. (tested Ardour and Qtractor)

some design details
===================
"make" will first parse a pd patch for a few informations like:
* the plugin's name
* the name of the control inputs
* the control inputs lower and upper limits and their default values at plugin initialization
* the plugin's URI
parsing is done in bash using sed, should be replaced with python for platform independent use.

the parsed data is fed into templates for the c source, rdf manifest, etc. Then compiling starts.

Each plugin has its own shared object (.so) file in its bundle containing names of the pd patch and the plugin URI compiled in.
Since the lv2 libraries changed since I made the plugin system this could be circumventen - only one already compiled .so
instead of many different .so files which have to be compiled in advance...

Other ideas to improve next
===========================

Please get your hands dirty. I don't have lots of time for this project.

important:
* make use of the plugin directory to read plugin URI (available in newer lv2 lirary) - makes recompiling unnecessary.
* add midi input
* make it realtime safe (threaded loading of patches)
* use directories to contain everything for one plugin - allows use of 
* create control inputs only when needed, allow unlimited inputs
** dynamically add inputs during runtime
* update modified patches without restarting plugin host (watch for file changes)
* make patches editable (requires the above)


less important:
* get rid of Makefiles (python)
* get rid of bash (python)
