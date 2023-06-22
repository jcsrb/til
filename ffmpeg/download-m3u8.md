# Download and merge content of m3u8 files

```bash
ffmpeg -protocol_whitelist file,http,https,tcp,tls -allowed_extensions ALL -i audio.m3u8 -bsf:a aac_adtstoasc -c copy audio.aac
ffmpeg -protocol_whitelist file,http,https,tcp,tls -allowed_extensions ALL -i video.m3u8 -bsf:a aac_adtstoasc -c copy video.mp4

ffmpeg -i video.mp4 -i audio.aac -c:v copy -c:a aac -strict experimental output.mp4
```
