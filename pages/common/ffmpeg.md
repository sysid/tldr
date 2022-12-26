# ffmpeg

> Video conversion tool.
> More information: <https://ffmpeg.org>.

- Extract the sound from a video and save it as MP3:

`ffmpeg -i {{video.mp4}} -vn {{sound}}.mp3`

- Save a video as GIF, scaling the height to 1000px and setting framerate to 15:

`ffmpeg -i {{video.mp4}} -vf 'scale=-1:{{1000}}' -r {{15}} {{output.gif}}`

- Combine numbered images (`frame_1.jpg`, `frame_2.jpg`, etc) into a video or GIF:

`ffmpeg -i {{frame_%d.jpg}} -f image2 {{video.mpg|video.gif}}`

- Quickly extract a single frame from a video at time mm:ss and save it as a 128x128 resolution image:

`ffmpeg -ss {{mm:ss}} -i {{video.mp4}} -frames 1 -s {{128x128}} -f image2 {{image.png}}`

- Trim a video from a given start time mm:ss to an end time mm2:ss2 (omit the -to flag to trim till the end):

`ffmpeg -ss {{mm:ss}} -to {{mm2:ss2}} -i {{video.mp4}} -codec copy {{output.mp4}}`

- Convert AVI video to MP4. AAC Audio @ 128kbit, h264 Video @ CRF 23:

`ffmpeg -i {{input_video}}.avi -codec:audio aac -b:audio 128k -codec:video libx264 -crf 23 {{output_video}}.mp4`

- Remux MKV video to MP4 without re-encoding audio or video streams:

`ffmpeg -i {{input_video}}.mkv -codec copy {{output_video}}.mp4`

- Convert MP4 video to VP9 codec. For the best quality, use a CRF value (recommended range 15-35) and -b:video MUST be 0:

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
