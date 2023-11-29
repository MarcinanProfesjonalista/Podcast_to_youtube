## Podcast Conversion Script

This script is designed to assist in converting podcast audio files to video format, allowing for the inclusion of a static image. The provided script uses FFmpeg to create video files (.mp4) from a collection of input audio files (.mp3).

### Usage

1. **Requirements:**
   - [FFmpeg](https://ffmpeg.org/download.html) must be installed on your system and accessible in the system's PATH.

2. **Script Customization:**
   - Adjust the image filename and its path in the script, where `-i Dyrektywa_maszynowa.jpg` specifies the static image to be used.
   - Customize the output filename pattern by modifying the `set "output_file=..."` line. In the provided script, it adds a prefix "Dyrektywa_Maszynowa_" to the original filename.

3. **Execution:**
   - Run the script in a directory containing the target audio files (in this case, *.mp3) and the specified image file.

4. **Progress Display:**
   - The script displays progress information as it processes each audio file, providing a visual indication of the conversion progress.

5. **Output:**
   - The resulting video files are created in the same directory as the script with a ".mp4" extension.

### Example
```bash
@echo off

setlocal enabledelayedexpansion

set "total_files=0"
for %%I in (*.mp3) do (
    set /a "total_files+=1"
)

set "i=0"

for %%I in (*.mp3) do (
    set /a "i+=1"
    
    set "output_file=Dyrektywa_Maszynowa_%%~nI.mkv"

    rem Wywołanie FFmpeg dla konwersji (w tle)
    start "" ffmpeg -loop 1 -framerate 2 -i Dyrektywa_maszynowa.jpg -i "%%I" -c:a libx264 -preset medium -tune stillimage -crf 18 -c:a copy -shortest -pix_fmt yuv420p "!output_file!"

    rem Obliczanie postępu i wyświetlanie paska
    set /a "progress=i * 100 / total_files"
    echo Progress: !progress!%%
)

echo Conversion complete!
