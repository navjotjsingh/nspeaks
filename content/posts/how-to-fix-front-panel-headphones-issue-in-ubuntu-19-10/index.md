---
title: "How to fix Front Panel headphones Issue in Ubuntu 19.10/20.04"
date: "2019-11-28"
categories: 
  - "linux"
tags: 
  - "headphone"
  - "sound-issue"
summary: Learn how to fix no sound issue from front headphone port in Ubuntu.
---

**UPDATE: 21 December - Permanent solution added**

Ever since I have switched to Ubuntu as my primary OS, I have constantly faced the issue where my front panel sound ports for headphones don't work. For some reason, Ubuntu likes to keep your front headphones on mute by default. And you won't find any setting to unmute them in your Settings Panel. You will have to fire up your terminal for that.

Enter the following command to launch the Alsamixer tool in Ubuntu.

```shell
$ alsamixer
```

You will see a GUI window inside your Terminal which will look like this.

![AlsaMixer GUI Terminal Window](images/Screenshot-from-2019-11-28-12-26-53.png#center)

You can see the Headphone volume as zero. To adjust it, press the right arrow key to bring the cursor on the Headphone. Press M to unmute it and press the Up arrow key to increase the volume. When you have maxed out the headphone volume, press the Escape key to exit the tool.

You should be able to hear sounds from your front panel connected Headphones again. But hold on. There is an issue. If you reboot, you will again have to repeat this because yes Ubuntu thinks headphones are best left on mute.

We can fix this by creating an autostart entry to bring back the headphone volume back to normal on boot. To do this, first fire up the alsamixer tool and adjust the volume of your headphone and other stuff the way you like it. When you are satisfied with your changes, exit the tool and type the following command to save the settings in `~/.config/asound.state` file.

```shell
$ sudo alsactl --file ~/.config/asound.state store
```

The next step is to create a .desktop file in the `~/.config/autostart/` directory. Use the following command which will create and edit the required desktop file in the Nano editor.

```shell
$ sudo nano ~/.config/autostart/alsarestore.desktop
```

Enter the following lines in your Nano editor.

```bash
[Desktop Entry]
Type=Application
Terminal=false
Name=alsarestore
Exec=alsactl --file ~/.config/asound.state restore
```

Press Ctrl+X to exit the editor and press Y when prompted to save the file.

Our work is done here. Now, your headphone volume will remain maxed even after a reboot.

There is one more issue to be handled here. I am not sure if it happens with everyone but it does happen to me. The headphone keeps getting muted automatically for me every 10-12 minutes. So the above method won't work because it is supposed to restore the volume only during a reboot. To get around this issue, we will have to create a cron job that will run the command to restore the settings after every 10 minutes.

To create a cron job, enter the following command to launch the Crontab Editor.

```
$ crontab -e
```

The **\-e** parameter in the command ensures we are editing only the current user's crontab. If you haven't used the tool before, it will ask you to choose the editor you want to use to edit the crontab. Choose the first option, i.e., the Nano editor which is also the easiest editor to use on the Terminal.

Once you have chosen the editor, a temporary cron file will open. Scroll to its end and enter the following code into it.

```bash
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
*/5 * * * * alsactl --file ~/.config/asound.state restore >/dev/null 2>&1
```

This code will ensure that the alsa settings restoration command will run every 5 minutes every day. Save and exit the editor by pressing Ctrl+X and pressing Y when asked to save the file.

Also, if you are going to use the cronjob, you won't be needing the desktop file.

**Permanent Solution to the Problem**

So all of the above solutions are a temporary fix and require the restore process to run all the time and even at bootup. Even then you will find your headphone muted in between those intervals. Anyway, I found a permanent solution to my problem.

Pulseaudio wrongly assumes that our headphones are off and so it automatically overrides Alsamixer settings and mutes them. There are ways to disable PulseAudio but that also means stopping the Audio from working at all. Fortunately, there is a method to tell Pulseaudio that the headphone is on and it shouldn't mute it.

Open the file `/usr/share/pulseaudio/alsa-mixer/paths/analog-output-lineout.conf` in your favorite editor.

Try to find the following line

```
[Element Headphone]
switch = off
volume = off
```

and change the value of switch from off to on. Do it for `[Element Headphone2]` or any other entry related to Headphone or Front port.

Make the same changes in the file `/usr/share/pulseaudio/alsa-mixer/paths/analog-output-headphones.conf` and `/usr/share/pulseaudio/alsa-mixer/paths/analog-output-headphones-2.conf` as well.

After making all of the above changes, run the following two commands to reload and restart PulseAudio.

```
$ pulseaudio -k
$ pulseaudio --start
```
