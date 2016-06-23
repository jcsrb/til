# set screen resolution via the command line

to see available resolutions use:

```
$ xrandr

Screen 0: minimum 8 x 8, current 3200 x 1080, maximum 32767 x 32767
eDP1 connected primary 1920x1080+0+0 (normal left inverted right x axis y axis) 345mm x 194mm
   1920x1080     59.91 +  59.93* 
   1680x1050     59.95    59.88  
   1600x1024     60.17  
   1400x1050     59.98  
   1600x900      60.00  
   1280x1024     60.02  
   1440x900      59.89  
   1280x960      60.00  
   1368x768      60.00  
   1360x768      59.80    59.96  
   1152x864      60.00  
   1280x720      60.00  
   1024x768      60.00  
   1024x576      60.00  
   960x540       60.00  
   800x600       60.32    56.25  
   864x486       60.00  
   640x480       59.94  
   720x405       60.00  
   640x360       60.00  
HDMI1 disconnected (normal left inverted right x axis y axis)
VGA1 connected 1280x1024+1920+0 (normal left inverted right x axis y axis) 0mm x 0mm
   1280x1024     60.02*+  60.02* 
   1024x768      60.00  
   800x600       60.32    56.25  
   848x480       60.00  
   640x480       59.94  
VIRTUAL1 disconnected (normal left inverted right x axis y axis)
```

then set one for a specific output
```
$ xrandr --output VGA1 --mode 1280x1024
```
