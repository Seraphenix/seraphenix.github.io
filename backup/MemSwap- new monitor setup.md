# How to deal with a totally new monitor size?
## 1 Confirm your new setup
In **MemSwap_Screen.m** line 28
```matlab
{
    if strcmpi(O.monitorType,'laptop')
        width  = 310;% mm
        height = 174;
        viewdist = 58;              % viewing distance in cm
    elseif strcmpi(O.monitorType,'desktop')
        % Depends on the device
        width  = 530;% mm
        height = 298;
        viewdist = 60;              % viewing distance in cm
        % WARNING: viewing distance not determined
    end
}
```

I have listed all the require measurement in real-life. Please update your new setup with a new `elseif`, e.g.
```matlab
{
    if strcmpi(O.monitorType,'laptop')
        width  = 310;% mm
        height = 174;
        viewdist = 58;              % viewing distance in cm
    elseif strcmpi(O.monitorType,'desktop')
        % Depends on the device
        width  = 530;% mm
        height = 298;
        viewdist = 60;              % viewing distance in cm
        % WARNING: viewing distance not determined
    elseif strcmpi(O.monitorType,'desktop2')
        width  = 610;% mm
        height = 340;
        viewdist = 60;              % viewing distance in cm
    end
}
```

After the change, you could run the following code in the MATLAB to see what the essential parameters are in the new setup:
```matlab
{
    width  = 610;% mm
    height = 340;
    viewdist = 60;              % viewing distance in cm
    cm2deg   = 2*atan(1/(2*viewdist))*180/pi;
    deg2cm   = 1/cm2deg;           % deg by cm
    pix2cm   = width/(widthpx*10); % length of a pix (cm) H size of my screen / H definition of my screen
    cm2pix   = 1/pix2cm;           % pixels in 1 cm (dpc)
    pix2deg  = pix2cm*cm2deg;      % angle of a pix (deg)
    deg2pix  = 1/pix2deg;          % pixels in 1 deg (dpg)
}
```
Get the `deg2pix`, we will use that later.

## 2 Make a new color ring picture
The radius of the color ring is **8 degrees**.
The width of the ring is **2 degrees**.
So calculate the new image size with `deg2pix*8*2` and `deg2pix*2`.

Then open **MemSwap_MakeRing.m** line 10
```matlab
{
    if strcmpi(displaySetup,'desktop')
        % Image size
        % Display-base information
        % Original setup, roughly based on a 24-inch display, 1080p and 60 cm viewing distance
        imSize = 600;
        ringRadOut = imSize/2;
        ringRadInn = imSize/2 - 75;
    elseif strcmpi(displaySetup,'laptop')
        % Laptop setup, 14-inch display, 1080p and 58 cm viewing distance
        imSize = 1000;
        ringRadOut = imSize/2;
        ringRadInn = imSize/2 - 130;
    end
}
```

Again, make a new setup with `elseif`.
I have already calculated `deg2pix*8*2` under the new setup, it's around 610. `deg2pix*2` is 66.

```matlab
{
    if strcmpi(displaySetup,'desktop')
        % Image size
        % Display-base information
        % Original setup, roughly based on a 24-inch display, 1080p and 60 cm viewing distance
        imSize = 600;
        ringRadOut = imSize/2;
        ringRadInn = imSize/2 - 75;
    elseif strcmpi(displaySetup,'laptop')
        % Laptop setup, 14-inch display, 1080p and 58 cm viewing distance
        imSize = 1000;
        ringRadOut = imSize/2;
        ringRadInn = imSize/2 - 130;
    elseif strcmpi(displaySetup,'desktop2')
        imSize = 610;
        ringRadOut = imSize/2;
        ringRadInn = imSize/2 - 66;
    end
}
```
Change the first line to the new setup name:
`displaySetup = 'desktop2';`
Then run the script **MemSwap_MakeRing.m**.

It will generate a new image file under the working directory. **colorRing_LAB_linear-rgb239To305.png**, in my case.

## 3 Switch the ring picture back to the code
Open **MemSwap_TrialSetup.m** line 21
```matlab
{
    if strcmpi(O.monitorType,'laptop')
        ringFile = ['colorRing_',O.colorSpace,'370To500.png'];
    elseif strcmpi(O.monitorType,'desktop')
        % However, for convience, the color ring is a pre-made 500-by-500 PNG image, made by "MakeRing" script
        ringFile = ['colorRing_',O.colorSpace,'225To300.png'];
    end
}
```
Again, add the new setup:
```matlab
{
    if strcmpi(O.monitorType,'laptop')
        ringFile = ['colorRing_',O.colorSpace,'370To500.png'];
    elseif strcmpi(O.monitorType,'desktop')
        % However, for convience, the color ring is a pre-made 500-by-500 PNG image, made by "MakeRing" script
        ringFile = ['colorRing_',O.colorSpace,'225To300.png'];
    elseif strcmpi(O.monitorType,'desktop2')
        ringFile = ['colorRing_',O.colorSpace,'239To305.png'];
    end
}
```
It should be OK to go.

## Future
Perhaps I will automate the image generation steps that we do not need to change it manually?


