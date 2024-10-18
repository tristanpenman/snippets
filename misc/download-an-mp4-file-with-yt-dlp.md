# Download an MP4 file with yt-dlp

By default, `yt-dlp` will automatically choose a format to download, and this may not always be the codec or container format that you want (e.g. VP8 / webm).

To download a video as an MP4 file, you can use the following:

```
yt-dlp -S vcodec:h264,fps,res:720,acodec:m4a "<URL>"
```

This will download a 720p video, using H.264 as the preferred video codec, and M4A as the audio codec. These will be combined into an MP4 file.

Note that the URL is contained in quotes, so that characters such as `?` and `&` do not trigger shell-specific behaviours.

## 1080p

It's trivial to alter the command above to choose 1080p instead:

```
yt-dlp -S vcodec:h264,fps,res:1080,acodec:m4a "<URL>"
```

## Alternative Codecs

Alternative codecs can also be used. To list the available codecs / formats / resolutions for a given video:

```
yt-dlp --list-formats "<URL>"
```

The output will be a complete list like this:

```
$ yt-dlp --list-formats "https://www.youtube.com/watch?v=lqoMPzxk7II"
[youtube] Extracting URL: https://www.youtube.com/watch?v=lqoMPzxk7II
[youtube] lqoMPzxk7II: Downloading webpage
[youtube] lqoMPzxk7II: Downloading ios player API JSON
[youtube] lqoMPzxk7II: Downloading mweb player API JSON
[youtube] lqoMPzxk7II: Downloading m3u8 information
[info] Available formats for lqoMPzxk7II:
ID      EXT   RESOLUTION FPS CH │   FILESIZE   TBR PROTO │ VCODEC          VBR ACODEC      ABR ASR MORE INFO
─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
sb3     mhtml 48x27        1    │                  mhtml │ images                                  storyboard
sb2     mhtml 80x45        1    │                  mhtml │ images                                  storyboard
sb1     mhtml 160x90       1    │                  mhtml │ images                                  storyboard
sb0     mhtml 320x180      1    │                  mhtml │ images                                  storyboard
233     mp4   audio only        │                  m3u8  │ audio only          unknown             Default
234     mp4   audio only        │                  m3u8  │ audio only          unknown             Default
599     m4a   audio only      2 │  651.01KiB   31k https │ audio only          mp4a.40.5   31k 22k ultralow, m4a_dash
600     webm  audio only      2 │  625.88KiB   30k https │ audio only          opus        30k 48k ultralow, webm_dash
139-drc m4a   audio only      2 │    1.01MiB   49k https │ audio only          mp4a.40.5   49k 22k low, DRC, m4a_dash
249-drc webm  audio only      2 │  919.71KiB   44k https │ audio only          opus        44k 48k low, DRC, webm_dash
250-drc webm  audio only      2 │    1.18MiB   57k https │ audio only          opus        57k 48k low, DRC, webm_dash
139     m4a   audio only      2 │    1.01MiB   49k https │ audio only          mp4a.40.5   49k 22k low, m4a_dash
249     webm  audio only      2 │  914.34KiB   43k https │ audio only          opus        43k 48k low, webm_dash
250     webm  audio only      2 │    1.17MiB   57k https │ audio only          opus        57k 48k low, webm_dash
140-drc m4a   audio only      2 │    2.67MiB  130k https │ audio only          mp4a.40.2  130k 44k medium, DRC, m4a_dash
251-drc webm  audio only      2 │    2.31MiB  112k https │ audio only          opus       112k 48k medium, DRC, webm_dash
140     m4a   audio only      2 │    2.67MiB  130k https │ audio only          mp4a.40.2  130k 44k medium, m4a_dash
251     webm  audio only      2 │    2.30MiB  112k https │ audio only          opus       112k 48k medium, webm_dash
597     mp4   256x144     12    │  650.35KiB   31k https │ avc1.4d400b     31k video only          144p, mp4_dash
602     mp4   256x144     12    │ ~  1.71MiB   83k m3u8  │ vp09.00.10.08   83k video only
598     webm  256x144     12    │  504.62KiB   24k https │ vp9             24k video only          144p, webm_dash
394     mp4   256x144     24    │    1.06MiB   51k https │ av01.0.00M.08   51k video only          144p, mp4_dash
269     mp4   256x144     24    │ ~  3.41MiB  165k m3u8  │ avc1.4D400C    165k video only
160     mp4   256x144     24    │    1.14MiB   55k https │ avc1.4D400C     55k video only          144p, mp4_dash
603     mp4   256x144     24    │ ~  3.18MiB  154k m3u8  │ vp09.00.11.08  154k video only
278     webm  256x144     24    │    1.71MiB   83k https │ vp09.00.11.08   83k video only          144p, webm_dash
395     mp4   426x240     24    │    1.73MiB   84k https │ av01.0.00M.08   84k video only          240p, mp4_dash
229     mp4   426x240     24    │ ~  5.57MiB  270k m3u8  │ avc1.4D4015    270k video only
133     mp4   426x240     24    │    1.87MiB   91k https │ avc1.4D4015     91k video only          240p, mp4_dash
604     mp4   426x240     24    │ ~  5.83MiB  282k m3u8  │ vp09.00.20.08  282k video only
242     webm  426x240     24    │    2.37MiB  115k https │ vp09.00.20.08  115k video only          240p, webm_dash
396     mp4   640x360     24    │    3.08MiB  150k https │ av01.0.01M.08  150k video only          360p, mp4_dash
230     mp4   640x360     24    │ ~ 10.70MiB  519k m3u8  │ avc1.4D401E    519k video only
134     mp4   640x360     24    │    3.24MiB  157k https │ avc1.4D401E    157k video only          360p, mp4_dash
18      mp4   640x360     24  2 │    9.84MiB  477k https │ avc1.42001E         mp4a.40.2       44k 360p
605     mp4   640x360     24    │ ~ 11.63MiB  564k m3u8  │ vp09.00.21.08  564k video only
243     webm  640x360     24    │    4.10MiB  199k https │ vp09.00.21.08  199k video only          360p, webm_dash
397     mp4   854x480     24    │    5.20MiB  252k https │ av01.0.04M.08  252k video only          480p, mp4_dash
231     mp4   854x480     24    │ ~ 14.67MiB  711k m3u8  │ avc1.4D401E    711k video only
135     mp4   854x480     24    │    4.94MiB  239k https │ avc1.4D401E    239k video only          480p, mp4_dash
606     mp4   854x480     24    │ ~ 15.84MiB  768k m3u8  │ vp09.00.30.08  768k video only
244     webm  854x480     24    │    5.91MiB  287k https │ vp09.00.30.08  287k video only          480p, webm_dash
398     mp4   1280x720    24    │    9.18MiB  446k https │ av01.0.05M.08  446k video only          720p, mp4_dash
232     mp4   1280x720    24    │ ~ 23.83MiB 1155k m3u8  │ avc1.4D401F   1155k video only
136     mp4   1280x720    24    │    8.90MiB  432k https │ avc1.4D401F    432k video only          720p, mp4_dash
609     mp4   1280x720    24    │ ~ 23.43MiB 1136k m3u8  │ vp09.00.31.08 1136k video only
247     webm  1280x720    24    │    9.26MiB  449k https │ vp09.00.31.08  449k video only          720p, webm_dash
399     mp4   1920x1080   24    │   15.84MiB  768k https │ av01.0.08M.08  768k video only          1080p, mp4_dash
270     mp4   1920x1080   24    │ ~ 70.18MiB 3403k m3u8  │ avc1.640028   3403k video only
137     mp4   1920x1080   24    │   31.33MiB 1520k https │ avc1.640028   1520k video only          1080p, mp4_dash
614     mp4   1920x1080   24    │ ~ 51.22MiB 2484k m3u8  │ vp09.00.40.08 2484k video only
248     webm  1920x1080   24    │   24.15MiB 1172k https │ vp09.00.40.08 1172k video only          1080p, webm_dash
616     mp4   1920x1080   24    │ ~ 89.23MiB 4327k m3u8  │ vp09.00.40.08 4327k video only          Premium
```