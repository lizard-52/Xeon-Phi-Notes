# Actually Using x264 on Xeon Phi

Once copied over running the help command gives some useful information

```
[root@mic0 ~]# ./x264  --help
x264 core:164
Syntax: x264 [options] -o outfile infile

Infile can be raw (in which case resolution is required),
  or YUV4MPEG (*.y4m),
  or Avisynth if compiled with support (no).
  or libav* formats if compiled with lavf support (no) or ffms support (no).
Outfile type is selected by filename:
 .264 -> Raw bytestream
 .mkv -> Matroska
 .flv -> Flash Video
 .mp4 -> MP4 if compiled with GPAC or L-SMASH support (no)
Output bit depth: 8/10

...
```
Most importantly it looks like the only supported input is .y4m and .mp4 output is not supported. Unfortunately .y4m is a raw format, so the files are huge. I transcoded a ~5min 360p .mp4 to .y4m with FFMPEG (the irony is not lost on me) and the resulting video was over 2GB! Copying over to the phi with WinSCP took about 8 minutes.

Running x264:
```
./x264 --crf 24 -o output.flv raw.Y4M
```
![](./Images/x264%20running%201.PNG)

That's pretty slow. Time to try more threads!
```
./x264 --crf 24 --threads=240 -o output.flv raw.Y4M
```
![](./Images/x264%20running%202.PNG)
Faster, but it's clearly not using all the threads properly.

Final fps on the 5110p was 10.5fps. Significantly worse than my 3700x which does the same transcode at 104 fps 