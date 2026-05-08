# Text-to-Video Lip Sync Pipeline

An end-to-end pipeline that generates a lip-synced talking-head video from a text topic.

**Topic → Script → Audio → Lip Sync → Video**

## Tools

- **Gemini 2.5 Flash Lite** – Generates a natural script from a given topic, tone, and duration
- **Kokoro TTS** – Converts script to speech (`af_heart` voice, 24kHz → resampled to 16kHz mono)
- **Wav2Lip (GAN checkpoint)** – Lip syncs audio to a face image or video
- **GFPGAN v1.3** – Post-processing face enhancement to reduce blur around the mouth

> Coqui TTS was originally planned but dropped due to dependency conflicts. Kokoro was used as a replacement.

## Setup

Runs on Google Colab with GPU enabled. Add your Gemini API key as a Colab secret named `scriptgem`.

```bash
pip install -r Wav2Lip/requirements.txt
pip install librosa==0.9.2 google-generativeai kokoro>=0.9.4 soundfile
apt-get install espeak-ng
```

## Usage

Configure these variables in the notebook:

```python
TOPIC    = 'why getting enough sleep makes you smarter'
TONE     = 'warm, friendly, and slightly enthusiastic'
DURATION = 25       # seconds
VOICE    = 'af_heart'
```

To use your own face, set `USE_OWN_FILE = True` and upload your photo/video to `/content/your_face.mp4`.

## Limitations

- Mouth region is often blurry — known limitation of the base Wav2Lip model
- Static image input produces less realistic results than a video base
- Occasional jitter on longer audio clips

## Planned Improvements

- Replace Wav2Lip with LatentSync, SadTalker, or DiffTalk for better quality
- Swap GFPGAN for CodeFormer for sharper face restoration
- Use a short looping video as face input instead of a static image
- Add audio preprocessing (noise reduction + normalization) before lip sync
