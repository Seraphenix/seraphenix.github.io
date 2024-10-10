Recent updates on MemSwap experiment code, including fixing issues, adding a break point and adding a new display setup.

# Update 1: EEG trigger code error fixed
In MemSwap_Display.m line 78 (line numbers may vary):
```matlab
{
    E = MemSwap_EEG(2,20+R.types(trial),E); % Signal: Fixation end
}
```

I made a mistake on the trigger code, now it's fixed. In the new version of MemSwap_Display.m, line 81
```matlab
{
    E = MemSwap_EEG(2,20+R.tarLoca(trial),E); % Signal: Fixation 
}
```
This is the correct version.

# Update 2: New desktop setup
In MemSwap_Option, I have listed two display setup:
```matlab
{
    O.monitorType = 'Laptop';
    % O.monitorType = 'Desktop';
}
```
Please refer to [this article](https://seraphenix.github.io/post/MemSwap-%20new%20monitor%20setup.html) for details about how to add new display setup.
Now a third setup is added into the code, called "DesktopHKU" (letter capitalization does not matter). As its name indicates, it is designed for the monitor in Hong Kong University Shenzhen Hospital. The original "Desktop", is for Shenzhen University General Hospital.

# Update 3: Break the experiment into two sessions
Due to the reports of overtime experiment length among observers, now the experiment will be break into two parts.
In new version of MemSwap_Main.m, line 50:
```matlab
{
    if ii == O.sessionBreak && R.lastTrial~=O.sessionBreak
        D.quitControl = 1;
    end
}
```

Before the change, the experiment will set a break time every 100 trials, letting the observers take a rest. Now, when it reaches 500th (the number could be modifed in Option) trial for the first time, the experiment will automatically quit.
