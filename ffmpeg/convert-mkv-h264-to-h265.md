
# Convert h264 files to HEVC/h265 

* uses apple m1 native `hevc_videotoolbox`
* copies all audio and subtitle tracks
* targets 2000kb/s bitrate
* optionally removes h264 file if the timecodes match

```sh
#!/bin/zsh

# Enable globbing to ensure patterns expand correctly
setopt +o nomatch

# Loop through all MKV files in the current directory
for file in *.mkv; do
  # Use ffprobe to check if the video codec is H.264
  codec=$(ffprobe -v error -select_streams v:0 -show_entries stream=codec_name -of default=noprint_wrappers=1:nokey=1 "$file")

  if [[ $codec == "h264" ]]; then
    # Construct the new filename by replacing "264" with "265"
    output="${file//264/265}"

    # Convert the video from H.264 to H.265 using VideoToolbox with a target quality/bitrate
    ffmpeg -i "$file" -c:v hevc_videotoolbox -b:v 2000k -map 0 -c:a copy -c:s copy "$output"

    # Compare the duration of the original and new file
    duration_original=$(ffprobe -v error -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 "$file")
    duration_new=$(ffprobe -v error -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 "$output")

    # Calculate the difference in duration (absolute value)
    duration_difference=$(echo "scale=2; if($duration_original > $duration_new) $duration_original - $duration_new else $duration_new - $duration_original" | bc)

    # Check if the difference in duration is negligible (e.g., less than 1 second)
    if (( $(echo "$duration_difference < 1" | bc -l) )); then
      echo "Duration matches, deleting original file: $file"
      # Uncomment the next line to actually delete the file
      # rm "$file"
    else
      echo "Duration mismatch for $file and $output"
    fi
  elif [[ $codec == "hevc" ]]; then
    echo "Skipping already encoded file: $file"
  else
    echo "Unsupported codec for file: $file"
  fi
done

```
