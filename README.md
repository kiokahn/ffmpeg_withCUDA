# FFMPEG with CUDA

original : https://www.gyan.dev/ffmpeg/builds/#release-builds

Tested :  MS WIndows 10 Pro, NVIDIA-SMI 516.94,  Driver Version: 516.94, CUDA Version: 11.7

By Kiok Ahn (kiokahn@gazzi.ai, kiokahn76@gmail.com)

## Check

- CUDA 
```
c:\>nvidia-smi
Fri Aug 19 15:12:44 2022
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 516.94       Driver Version: 516.94       CUDA Version: 11.7     |
|-------------------------------+----------------------+----------------------+
| GPU  Name            TCC/WDDM | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  Quadro M5000       WDDM  | 00000000:01:00.0  On |                  Off |
| 38%   35C    P8    15W / 150W |   2260MiB /  8192MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A      1308    C+G   ...lPanel\SystemSettings.exe    N/A      |
|    0   N/A  N/A      2476    C+G   ...t\Teams\current\Teams.exe    N/A      |
|    0   N/A  N/A      3148    C+G   ...ck\app-4.27.154\slack.exe    N/A      |
|    0   N/A  N/A      5216    C+G   C:\Windows\explorer.exe         N/A      |
|    0   N/A  N/A      7620    C+G   ...artMenuExperienceHost.exe    N/A      |
|    0   N/A  N/A      7936    C+G   ...5n1h2txyewy\SearchApp.exe    N/A      |
|    0   N/A  N/A      8076    C+G   ...me\Application\chrome.exe    N/A      |
|    0   N/A  N/A      9144    C+G   ...t\Teams\current\Teams.exe    N/A      |
|    0   N/A  N/A      9396    C+G   ...2txyewy\TextInputHost.exe    N/A      |
|    0   N/A  N/A     10044    C+G   ...293.54\msedgewebview2.exe    N/A      |
|    0   N/A  N/A     10368    C+G   ...y\ShellExperienceHost.exe    N/A      |
|    0   N/A  N/A     11448    C+G   ...e\PhoneExperienceHost.exe    N/A      |
|    0   N/A  N/A     12232    C+G   ...ty\Common7\IDE\devenv.exe    N/A      |
|    0   N/A  N/A     12704    C+G   ...ub.ThreadedWaitDialog.exe    N/A      |
|    0   N/A  N/A     13000    C+G   ...5n1h2txyewy\SearchApp.exe    N/A      |
|    0   N/A  N/A     16772    C+G   ...qxf38zg5c\Skype\Skype.exe    N/A      |
+-----------------------------------------------------------------------------+

C:\>
```

- ffmpeg
```
C:\>ffmpeg -hwaccels
ffmpeg version 5.1-full_build-www.gyan.dev Copyright (c) 2000-2022 the FFmpeg developers
  built with gcc 12.1.0 (Rev2, Built by MSYS2 project)
  configuration: --enable-gpl --enable-version3 --enable-static --disable-w32threads --disable-autodetect --enable-fontconfig --enable-iconv --enable-gnutls --enable-libxml2 --enable-gmp --enable-bzlib --enable-lzma --enable-libsnappy --enable-zlib --enable-librist --enable-libsrt --enable-libssh --enable-libzmq --enable-avisynth --enable-libbluray --enable-libcaca --enable-sdl2 --enable-libdav1d --enable-libdavs2 --enable-libuavs3d --enable-libzvbi --enable-librav1e --enable-libsvtav1 --enable-libwebp --enable-libx264 --enable-libx265 --enable-libxavs2 --enable-libxvid --enable-libaom --enable-libopenjpeg --enable-libvpx --enable-mediafoundation --enable-libass --enable-frei0r --enable-libfreetype --enable-libfribidi --enable-liblensfun --enable-libvidstab --enable-libvmaf --enable-libzimg --enable-amf --enable-cuda-llvm --enable-cuvid --enable-ffnvcodec --enable-nvdec --enable-nvenc --enable-d3d11va --enable-dxva2 --enable-libmfx --enable-libshaderc --enable-vulkan --enable-libplacebo --enable-opencl --enable-libcdio --enable-libgme --enable-libmodplug --enable-libopenmpt --enable-libopencore-amrwb --enable-libmp3lame --enable-libshine --enable-libtheora --enable-libtwolame --enable-libvo-amrwbenc --enable-libilbc --enable-libgsm --enable-libopencore-amrnb --enable-libopus --enable-libspeex --enable-libvorbis --enable-ladspa --enable-libbs2b --enable-libflite --enable-libmysofa --enable-librubberband --enable-libsoxr --enable-chromaprint
  libavutil      57. 28.100 / 57. 28.100
  libavcodec     59. 37.100 / 59. 37.100
  libavformat    59. 27.100 / 59. 27.100
  libavdevice    59.  7.100 / 59.  7.100
  libavfilter     8. 44.100 /  8. 44.100
  libswscale      6.  7.100 /  6.  7.100
  libswresample   4.  7.100 /  4.  7.100
  libpostproc    56.  6.100 / 56.  6.100
Hardware acceleration methods:
cuda
dxva2
qsv
d3d11va
opencl
vulkan


C:\>
```
    
    
## Encording

### 1. NVIDIA GPU Acceleration

####  - Input Argument


| Codec                     | Argument            |    
|----------------|--------------------------------|
| H.264          | -hwaccel cuvid -c:v h264_cuvid |
|H.265, HEVC     |-hwaccel cuvid -c:v hevc_cuvid  |

#### - Output Argument (Encording codec)

| Codec          | Argument                       |
|----------------|--------------------------------|
| H.264          | -c:v h264_nvenc                |
| H.265, HEVC    |-c:v hevc_nvenc                 |

#### - bitrate
```
-b:v 3600k 
```

#### - framerate (fps)
```
-r 30
```
#### - resorution
```
-s 1920x1080
```

###  2. Example

#### - Video to Image (mp4 to jpeg)
```
C:\ffmpeg_withCUDA>ffmpeg -i file_example_MP4_1920_18MG.mp4 .\input\input_%03d.jpg
```

#### - Jpeg files to mp4 video(h.264) with Audio
```
ffmpeg -framerate 30 -i .\input\input_%03d.jpg -i audio.mp3 -c:v h264_nvenc -b:v 7200k -vf format=yuv420p -c:a aac -shortest -movflags +faststart output.mp4
```

#### - Jpeg files to mp4 video(h.264) without Audio
```
ffmpeg -framerate 30 -i .\input\input_%03d.jpg -c:v h264_nvenc -b:v 7200k -vf format=yuv420p -shortest -movflags +faststart output.mp4
```
- More output quality
```
ffmpeg -framerate 30 -i .\input\input_%03d.jpg -c:v h264_nvenc -preset p6 -profile:v high -tune hq -rc-lookahead 8 -bf 2 -rc vbr -cq 26 -b:v 0 -maxrate 120M -bufsize 240M -shortest -movflags +faststart output.mp4
```

- More more output quality    
```
Change your gpu device
```

- JPEG decode with GPU
```
ffmpeg -framerate 30 -vsync 0 -hwaccel cuvid -c:v mjpeg_cuvid -i .\input\input_%03d.jpg -c:v h264_nvenc -preset p6 -profile:v high -tune hq -rc-lookahead 8 -bf 2 -rc vbr -cq 26 -b:v 0 -maxrate 120M -bufsize 240M -shortest -movflags +faststart output.mp4
```

