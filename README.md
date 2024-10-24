# ffmpeg-web

A Web and Native UI for ffmpeg-wasm: convert video, audio and images using the
power of ffmpeg, directly from your web browser or from your computer.

Try it: https://ffmpeg-web.netlify.app/

**Want something faster than your browser? Run ffmpeg-web with the full power on
your device using native ffmpeg and Electron only as a renderer. Read more in
the "Electron" section below.**

![UI of ffmpeg-web](./readme_assets/ffmpegweb-mainpage.png)
[![Netlify Status](https://api.netlify.com/api/v1/badges/54deaa95-e730-4007-8037-0d878109e6da/deploy-status)](https://app.netlify.com/sites/ffmpeg-web/deploys)
[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/Dinoosauro/ffmpeg-web)

## What you can do:

### Convert video and audio

You can convert video and audio files with lots of encoders. You can convert
thanks to this tool basically every:

- Video content to: H264 (MP4), H265 (MP4), VP9 (WebM), VP8* (WebM), Theora*
  (OGG), Windows Media Video* (WMW)
- Audio content to: MP3, AAC (M4A), Vorbis (OGG), Opus (OGG), FLAC, ALAC, WAV,
  Windows Media Audio* (WMA)

Note: * You need to enable "Show less common codec" in Settings

#### Merge video and audio file

You can merge video and audio files by selecting the "Copy video" and "Copy
audio" tag while encoding a media file. This will permit you to merge the video
files (see "Selecting a file" for important notice)

#### Customize media size:

You can change the video/audio bitrate, and other essential settings of that
media file (like FPS, orientation, channels etc.) directly from the interface.

![Video filters](./readme_assets/ffmpegweb-videooptions.png)

#### Add filters

You can add both video and audio filters. The most common (I think) ones have a
GUI, but you can pass any ffmpeg video filter configuration you want.

### Custom command

Do you have a ffmpeg command that you need to execute? Write that in the "Custom
command" section, and ffmpeg-web will execute that.

![Custom command UI](./readme_assets/ffmpegweb-inputoptions.png)

### Merge media

If you have two or more videos/audios and you want to merge them, use this
section. This will avoid re-encoding, so your media will be immediately ready.

![Merge content UI](./readme_assets/ffmpegweb-mergeoptions.png)

### Edit metadata

If you need to quickly edit some metadata, there's a section dedicated to it.
Choose from lots of default metadata keys, or create your own. Add the value and
then click to "Add value". Select the files and, without any re-encoding, the
metadata will be edited.

![Metadata UI](./readme_assets/ffmpegweb-metadataoptions.png)

### Convert images

Just like videos and audios, ffmpeg-web can also convert images to lots of
formats. You can also add some filters, the same as video's ones.

**Note:** you can use this tool also to extract a song's album art!

## File selection

At the top right of the page, you can see a "File selection" tab. Before doing
that, make sure you've set everything you want correctly. Then, if you are
converting your media, you should look at all the ways you can manage multiple
files by clicking on the select below the title:

- You can keep only the first file in the script
- Add all of the files in the output one (`ffmpeg -i file1 -i file2 ... output`)
- Keep the files that have the same name
  - You can choose if keeping only the files that have the same name as the
    first file or do the script for each combination of equal names
- Execute the same command for each selected file

## Trim content

ffmpeg-web permits you to trim content in lots of cases. You can choose to trim
a video:

- Providing the start and the end of the new video
- Writing a lists of timestamps with a divider (ex: useful if you need to trim
  video by chapters)

![Multiple timestamps UI](./readme_assets/ffmpegweb-multipletimestamps.png)

## Settings

You can change some settings in ffmpeg-web:

- Change how the file should be downloaded
  - Use the File System API, the normal downloads or save everything in a ZIP
    file
- Enable hardware acceleration (only on Electron)
- Create and change themes
- Enable a screensaver (and change its content)
- Change the length of alerts
- See the open source licenses

### Screensaver

If you're planning to use ffmpeg-web to convert lots of files, or if you want to
convert a really heavy one, you might want to enable the screensaver option.
It's a nice UI that you can customize with a custom image, custom video or
custom YouTube URL, and you can see the progress and the file that is being
converted (you can hide both of these if you want).

![Screensaver UI](./readme_assets/ffmpegweb-screensaver.jpg)

## Electron

You can run ffmpeg-web with native performance using Electron:

1. Clone this repository (you can
   [download it as a zip](https://github.com/Dinoosauro/ffmpeg-web/archive/refs/heads/main.zip)
   if you don't have git installed)
2. Make sure to have Node.JS installed. Minimum requirement: Node 20 LTS.
3. Type in `npm i` for installing dependancies.
4. Build the dist folder, so that you'll need to download the resources only
   once, by writing `node BuildDist.cjs` in the command line
5. Finally, write `npm run electron` to open the Electron build. From now on,
   you'll need to write only this to run ffmpeg-web.

### Differences between Electron and Web/Docker version:

- The Electron version is way faster, since it relies on native ffmpeg and not
  on a WebAssembly version
- The Electron version offers hardware acceleration features. Note that only
  Apple and Intel hardware acceleration have been tested
- If you want, you can still use FFmpeg WebAssembly, but that would be much
  slower compared to native FFmpeg. Note that, while using the native version,
  you cannot create zip files.

## Dockerfile

You can run ffmpeg-web also in a Docker container. Clone the repository (or
download the zip file) and then build the image:

`docker build -t ffmpeg-web .`

After this, you can start the container. The exposed parts are "80" for normal
HTTP and "443" for HTTPS (see below how to set it up). For example, to open
ffmpeg-web at `http://localhost:3000`:

`docker run -p 127.0.0.1:80:3000 ffmpeg-web`

### Enable HTTPS:

You might want to enable HTTPS with a self-signed certificate. Since the
Dockerfile uses a nginx server for the deploy, you need to do the following:

- In the `Dockerfile`, uncomment the `COPY` commands for the certificate.
  Replace the source path with the one of your certificate file and key
- In `nginx.conf`, uncomment from `listen 443 ssl`. You don't need to replace
  anything.

Build again the image, and HTTPS should be setted up.

## Privacy

Every video is elaborated locally, and nothing is sent to a server. ffmpeg-web
connects to:

- Google Fonts: fetch fonts (no other data sent)
- Unpkg: fetch essential scripts that make ffmpeg-web work
- Netlify: hosting

Note that, if you are using Microsoft Edge, you should disable "Enhance security
from this website" from the HTTPS secure symbol in the status bar. In this way,
ffmpeg will encode the media faster than before. After you've chosen the files,
the conversion will start automatically. You'll be able to see the progress at
the end of the page

## Offline use

You can install ffmpeg-web as a Progressive Web App to use it offline.
