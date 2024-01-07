# ffmpeg

> Video conversion tool.
> More information: <https://ffmpeg.org>.

- Extract the sound from a video and save it as MP3:

`ffmpeg -i {{path/to/video.mp4}} -vn {{path/to/sound}}.mp3`

- Save a video as GIF, scaling the height to 1000px and setting framerate to 15:

`ffmpeg -i {{path/to/video.mp4}} -vf 'scale=-1:{{1000}}' -r {{15}} {{path/to/output.gif}}`

- Combine numbered images (`frame_1.jpg`, `frame_2.jpg`, etc) into a video or GIF:

`ffmpeg -i {{path/to/frame_%d.jpg}} -f image2 {{video.mpg|video.gif}}`

- Quickly extract a single frame from a video at time mm:ss and save it as a 128x128 resolution image:

`ffmpeg -ss {{mm:ss}} -i {{path/to/video.mp4}} -frames 1 -s {{128x128}} -f image2 {{path/to/image.png}}`

- Trim a video from a given start time mm:ss to an end time mm2:ss2 (omit the -to flag to trim till the end):

`ffmpeg -ss {{mm:ss}} -to {{mm2:ss2}} -i {{path/to/video.mp4}} -codec copy {{path/to/output.mp4}}`

- Convert AVI video to MP4. AAC Audio @ 128kbit, h264 Video @ CRF 23:

`ffmpeg -i {{path/to/input_video}}.avi -codec:a aac -b:a 128k -codec:v libx264 -crf 23 {{path/to/output_video}}.mp4`

- Remux MKV video to MP4 without re-encoding audio or video streams:

`ffmpeg -i {{path/to/input_video}}.mkv -codec copy {{path/to/output_video}}.mp4`

- Convert MP4 video to VP9 codec. For the best quality, use a CRF value (recommended range 15-35) and -b:v MUST be 0:

<<<<<<< HEAD
`ffmpeg -i {{input_video}}.mp4 -codec:video libvpx-vp9 -crf {{30}} -b:video 0 -codec:audio libopus -vbr on -threads {{number_of_threads}} {{output_video}}.webm`


# ffmpeg ...........................................................................................
```bash
# convert .mov to .mp4
ffmpeg -i videoName.mov -vcodec h264 -acodec mp2 videoName.mp4
ffmpeg -i videoName.mov -vcodec h264 -acodec mp3 videoName.mp4
ffmpeg -i input.mov -vcodec h264 -acodec aac output.mp4

# convert .mov to .webm
ffmpeg -i videoName.mov -c:v libvpx -crf 10 -b:v 1M -c:a libvorbis videoName.webm

# convert .mov to .ogg
ffmpeg -i videoName.mov -codec:v libtheora -qscale:v 7 -codec:a libvorbis -qscale:a 5 videoName.ogg


# Convert wav to mp3
ffmpeg -i bbb_audio.wav -ac 2 -ar 44100 -b:a 320k bbb_audio_hqfull.mp3

# Convert wav to m4a (aac)
ffmpeg -i bbb_audio.wav -ac 2 -ar 44100 -b:a 320k bbb_audio_hqfull.m4a

# Convert wav to ogg (vorbis)
ffmpeg -i bbb_audio.wav -ac 2 -ar 44100 -b:a 320k bbb_audio_hqfull.ogg

# resample/resize: https://ottverse.com/change-resolution-resize-scale-video-using-ffmpeg/
ffmpeg -i input.mp4 -vf scale=1280:720 -preset slow -crf 18 output.mp4

# half the mov size
ffmpeg -i input.mov -vf "scale=iw/2:ih/2" output.mov
ffmpeg -i input.mov -vf "scale=iw/2:ih/2:force_original_aspect_ratio=decrease" -c:a copy output.mov
ffmpeg -i input.mov -vf "scale=iw/2:-2" output.mov

```

## Image Resizeing
key to preserving quality while resampling (resizing) is to use high-quality scaling algorithms and to avoid unnecessary re-encoding of the image more than needed. 

`-vf "scale=width:height"`: Applies video filter to scale the image. -1 to keep the aspect ratio
`-q:v 2`: Sets quality for JPEG. The scale is from 2 (best quality) to 31 (worst quality).
```bash
ffmpeg -i input.jpg -vf "scale=width:height" -q:v 2 output.jpg
# resize jpg: quality 2(best)-31
ffmpeg -i IMG_1471.jpg -vf "scale=1200:-1" -q:v 5 x.jpg
```

## Convert OSX heic format
can depend on how FFmpeg was compiled and which libraries were included. Not all builds of FFmpeg support HEIC due to licensing and patent issues 
```bash
ffmpeg -i input.heic output.png
ffmpeg -i input.heic output.jpg  # -q:v 2 for high quality (31: low), no alpha transparency
```

## Record on OSX
```bash
# OSX: list devices
ffmpeg -f avfoundation -list_devices true -i ""

ffmpeg -f avfoundation -i ":1" -t 10 audiocapture.mp3
# -f = "force format". In this case we're forcing the use of AVFoundation
# -i = input source. Typically it's a file, but you can use devices.
#   "0:1" = Record both audio and video from FaceTime camera and built-in mic
#   "0" = Record just video from FaceTime camera
#   ":1" = Record just audio from built-in mic
# -t = time in seconds. If you want it to run indefinitely until you stop it (ControlC) omit this value (not recommended)

# Stream to remote host
ffmpeg -f avfoundation -i ":1" -t 10 -f mpegts "tcp://remote_host_or_IP_:port"  # -f MPEG Transport Stream
# Set remote computer to "listen"
ffplay -i tcp://local_host_or_IP_addr:port?listen -hide_banner
# local_host_or_IP_addr:port is the IP address or hostname and the TCP port of the computer that's listening (not the computer that's streaming).
# ?listen is required to put it into "listen mode" otherwise it will time out if the stream is not there.
```

## Resources
[FFmpeg - Ultimate Guide | IMG.LY Blog](https://img.ly/blog/ultimate-guide-to-ffmpeg/)
=======
`ffmpeg -i {{path/to/input_video}}.mp4 -codec:v libvpx-vp9 -crf {{30}} -b:v 0 -codec:a libopus -vbr on -threads {{number_of_threads}} {{path/to/output_video}}.webm`
>>>>>>> 11ada1bc2471b6cab31280bd03f19ec66dc626c1
