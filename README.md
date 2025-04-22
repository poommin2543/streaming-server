# Streaming Server

A lightweight Docker-based streaming server setup powered by Alpine, designed for real-time video streaming with WebRTC support using Coturn.

---

## 📁 Project Structure

```
.
├── Dockerfile
├── docker-compose.yaml
├── NoomStreamming_Config.yml
└── runserver
```

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/poommin2543/streaming-server.git
cd streaming-server
```

> 🛠 GitHub: [poommin2543](https://github.com/poommin2543) · Email: poommin2543@gmail.com

---

### 2. Docker Compose Setup

Build and run the streaming server using `docker-compose`:

```bash
docker-compose up --build -d
```

This exposes the following ports:

- RTSP: `8554`
- HTTP: `8001`, `8002`, `8003`
- RTMP: `1935`
- Admin and stream interfaces: `8888`, `8889`, `8189`, `8890`

The `runserver` script is the entry point.

---

### 3. Coturn Setup (WebRTC Support)

Run Coturn in a separate container:

```bash
docker run -d --network=host coturn/coturn \
    -n --log-file=stdout \
    -p 3478:3478 -p 3478:3478/udp \
    -p 5349:5349 -p 5349:5349/udp \
    --lt-cred-mech --fingerprint \
    --no-multicast-peers --no-cli \
    --no-tlsv1 --no-tlsv1_1 \
    --realm=my.realm.org \
    --user='noom:noom' \
    --listening-port=3478
```

You can adjust `--realm` and `--user` to suit your environment.

---

## 🐳 Dockerfile

```dockerfile
FROM alpine:latest
WORKDIR /app
COPY . /app
RUN chmod +x /app/runserver
CMD ["/app/runserver"]
```

---

## 🔧 Configuration

Modify the `NoomStreamming_Config.yml` file to change stream or server parameters.

---

## 📡 Supported Protocols and Codecs

### Ingest (Input)

| Protocol              | Variants                            | Video Codecs                                                        | Audio Codecs                                                                                   |
|-----------------------|-------------------------------------|---------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| **SRT clients**       |                                     | H265, H264, MPEG-4 Video (H263, Xvid), MPEG-1/2 Video               | Opus, MPEG-4 Audio (AAC), MPEG-1/2 Audio (MP3), AC-3                                            |
| **SRT cameras/servers** |                                   | H265, H264, MPEG-4 Video (H263, Xvid), MPEG-1/2 Video               | Opus, MPEG-4 Audio (AAC), MPEG-1/2 Audio (MP3), AC-3                                            |
| **WebRTC clients**   | WHIP                                | AV1, VP9, VP8, H265, H264                                           | Opus, G722, G711 (PCMA, PCMU)                                                                   |
| **WebRTC servers**   | WHEP                                | AV1, VP9, VP8, H265, H264                                           | Opus, G722, G711 (PCMA, PCMU)                                                                   |
| **RTSP clients**     | UDP, TCP, RTSPS                     | AV1, VP9, VP8, H265, H264, MPEG-4 Video, MPEG-1/2 Video, M-JPEG     | Opus, AAC, MP3, AC-3, G726, G722, G711 (PCMA, PCMU), LPCM, RTP-compatible codecs               |
| **RTSP cameras/servers** | UDP, Multicast, TCP, RTSPS      | AV1, VP9, VP8, H265, H264, MPEG-4 Video, MPEG-1/2 Video, M-JPEG     | Opus, AAC, MP3, AC-3, G726, G722, G711 (PCMA, PCMU), LPCM, RTP-compatible codecs               |
| **RTMP clients**     | RTMP, RTMPS, Enhanced RTMP          | AV1, VP9, H265, H264                                               | Opus, AAC, MP3, AC-3, G711 (PCMA, PCMU), LPCM                                                   |
| **RTMP cameras/servers** | RTMP, RTMPS, Enhanced RTMP      | AV1, VP9, H265, H264                                               | Opus, AAC, MP3, AC-3, G711 (PCMA, PCMU), LPCM                                                   |
| **HLS**              | LL-HLS, MP4-HLS, Legacy HLS         | AV1, VP9, H265, H264                                               | Opus, AAC                                                                                       |
| **UDP/MPEG-TS**      | Unicast, Broadcast, Multicast       | H265, H264, MPEG-4 Video, MPEG-1/2 Video                           | Opus, AAC, MP3, AC-3                                                                             |
| **Raspberry Pi Cam** |                                     | H264                                                               | -                                                                                                |

### Output (Read from Server)

| Protocol     | Variants                           | Video Codecs                                                        | Audio Codecs                                                                                   |
|--------------|------------------------------------|---------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| **SRT**      |                                    | H265, H264, MPEG-4 Video, MPEG-1/2 Video                            | Opus, AAC, MP3, AC-3                                                                             |
| **WebRTC**   | WHEP                               | AV1, VP9, VP8, H265, H264                                           | Opus, G722, G711 (PCMA, PCMU)                                                                   |
| **RTSP**     | UDP, Multicast, TCP, RTSPS         | AV1, VP9, VP8, H265, H264, MPEG-4 Video, MPEG-1/2 Video, M-JPEG     | Opus, AAC, MP3, AC-3, G726, G722, G711 (PCMA, PCMU), LPCM, RTP-compatible codecs               |
| **RTMP**     | RTMP, RTMPS, Enhanced RTMP         | H264                                                               | AAC, MP3                                                                                        |
| **HLS**      | LL-HLS, MP4-HLS, Legacy HLS        | AV1, VP9, H265, H264                                               | Opus, AAC                                                                                       |

### Recording & Playback

| Format    | Video Codecs                                                       | Audio Codecs                                                                                   |
|-----------|--------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| **fMP4**  | AV1, VP9, H265, H264, MPEG-4 Video, MPEG-1/2 Video, M-JPEG         | Opus, AAC, MP3, AC-3, G711 (PCMA, PCMU), LPCM                                                  |
| **MPEG-TS** | H265, H264, MPEG-4 Video, MPEG-1/2 Video                         | Opus, AAC, MP3, AC-3                                                                             |

---

## 📋 License

MIT License

---

## 🙏 Credits

Built by Teddy Engineering.

