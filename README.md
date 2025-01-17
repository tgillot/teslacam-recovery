# teslacam-recovery

Currently Teslas only leave the last hour of recent footage on their thumbdrives. However, footage from farther back is easily accessible using basic data recovery techniques.

This repo contains a short script that will scan your entire FAT32 or exFAT USB drive for MP4s that have been deleted and copies them to a directory for viewing.

Warning: This is not meant to be a fool-proof guide. This is oriented towards the technically minded with some familiarity with Python and filesystems.

# Prerequisites

* Python 3

# Basics

macOS:
* Plug in your USB drive and unmount it.
* Figure out the device node for your FAT32 or exFAT partition. You can find this in Disk Utility when you click the "Info" button for your volume. It should be something like "disk2s1".
* Create a local directory to store the output (e.g., teslacamrecovery). 
* Run the script, providing the directory path and device node like so: `sudo ./run.py /dev/disk2s1 teslacamrecovery`

This will take a while. The script is not made for space or time efficiency. You'll need a lot of spare disk space (possibly up to twice as much as your drive's capacity).

Your directory should fill up with video files pretty quickly once the script is running.

Linux (Ubuntu):
* Plug in your USB drive.
* Figure out the device by by looking in /dev/disk (/dev/disk/by-label may be most user friendly).
* Create a local directory to store the output (e.g., teslacamrecovery). 
* Run the script, providing the directory path and device node like so: `sudo ./run.py /dev/disk/<directory>/<your device> /<path to teslacamrecovery>`

# Advanced

If you're impatient or low on spare disk space and feeling adventurous, the script includes code for navigating the filesystem, printing out detailed info, and exporting individual files. If you know basic Python, you can probably do a faster, more targeted search for the files you want. My car seems to do a good job at storing files contiguously and in order, so if you can find the cluster for an in-tact file near the time-frame of interest, you can probably get by with scanning a subset of clusters instead of the entire disk. See the comments in run.py.

# Broken MP4s

If your output contains MP4s that don't play, the most likely cause is that the MP4 was fragmented across multiple cluster runs. This will happen if the car needs to write around a previously saved video. The script won't automatically extract those, but with some effort, you can do it yourself. The filename of the MP4 indicates the start cluster of the file. To extract the video, you should try reading all of the clusters starting with that one, skipping over clusters used by active filesystem entries. (This would be pretty easily scriptable; PRs are welcome.)
