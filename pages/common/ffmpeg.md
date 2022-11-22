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
```
