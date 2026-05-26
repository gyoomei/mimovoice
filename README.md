# 🎙️ MiMoVoice

### Real-Time Voice Changer — Powered by Xiaomi MiMo V2.5

A fully client-side voice changer that processes your microphone input in real-time with 9 effects — robot, chipmunk, deep voice, echo, reverb, alien, radio, whisper, and normal. Canvas waveform and frequency visualization, adjustable pitch/volume/reverb, and recording with playback. Zero dependencies, single HTML file.

[![Live Demo](https://img.shields.io/badge/Live-Demo-000?style=for-the-badge&logo=github)](https://gyoomei.github.io/mimovoice/)
[![Try Now](https://img.shields.io/badge/Try-Now-22d3ee?style=for-the-badge&logo=googlechrome)](https://gyoomei.github.io/mimovoice/)
[![AI](https://img.shields.io/badge/AI-Xiaomi%20MiMo%20V2.5-f97316?style=for-the-badge)](https://mimo.xiaomi.com/)
[![Zero API](https://img.shields.io/badge/Zero-API-22c55e?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

---

## 📖 The Problem

Voice changers are either desktop apps (Voicemod, Clownfish), require installation, or are server-based with latency. There's no instant browser-based voice changer that processes audio locally with zero lag — just open, pick an effect, and speak.

## ✨ How it works

```
You click:     🎤 mic button → grant microphone access
You pick:      9 voice effects (robot, chipmunk, deep, echo, reverb, alien, radio, whisper)
You hear:      Real-time processed audio through speakers
You see:       Canvas waveform + frequency bars respond to your voice
You record:    Hit record → speak → play back or save
```

**That's the entire UX** — mic on, pick effect, talk. No accounts, no downloads, no latency.

## 🎯 Core Features

| Capability | Detail |
|---|---|
| Effects | 9 voice effects: Normal, Robot, Chipmunk, Deep, Echo, Reverb, Alien, Radio, Whisper |
| Real-time | Web Audio API processes audio at 44.1kHz with minimal latency |
| Visualizer | Canvas waveform + frequency spectrum bars |
| Controls | Pitch (-12 to +12), Volume, Reverb amount sliders |
| Recording | Record processed voice → play back or download |
| Keyboard | Toggle mic with click |
| Dark/Light | Toggle with persistence |
| Mobile | Touch-friendly, responsive layout |

## 🏗️ Architecture

```
┌──────────────────────────────────────────┐
│  getUserMedia → MediaStreamSource         │
│       │                                    │
│  Audio Graph per Effect                   │
│  Robot: ring modulation (×50Hz osc)       │
│  Chipmunk: highshelf boost +4kHz          │
│  Deep: lowshelf boost 500Hz               │
│  Echo: delay 300ms + feedback loop        │
│  Reverb: ConvolverNode (impulse response) │
│  Alien: ring mod + reverb                 │
│  Radio: bandpass 1500Hz Q=2               │
│  Whisper: noise + lowpass 3kHz            │
│       │                                    │
│  AnalyserNode → Canvas waveform           │
│  MediaStreamDestination → Recording       │
└──────────────────────────────────────────┘
```

## 💡 Architecture Decisions

**Why Web Audio API graph nodes?** Each effect is a unique combination of audio nodes — ring modulation for robot, ConvolverNode for reverb, DelayNode with feedback for echo. The graph is rebuilt dynamically when switching effects, ensuring clean audio with no artifacts.

**Why ConvolverNode for reverb?** Real reverb needs an impulse response — a recording of a room's acoustic properties. We generate one procedurally using exponential decay noise. ConvolverNode applies this mathematically, producing realistic hall effects.

**Why single HTML?** Voice processing needs zero latency. Any network roundtrip would create audible delay. The entire audio graph runs in the browser's audio thread at 44.1kHz with <10ms latency.

## 🧪 Try these examples

| Effect | What it sounds like |
|---|---|
| Robot | Metallic, modulated voice with 50Hz tremolo |
| Chipmunk | High-pitched, bright voice |
| Deep | Low, bass-heavy voice |
| Echo | Voice repeating with 300ms delay |
| Reverb | Spacious, hall-like ambience |
| Alien | Modulated + reverberant, sci-fi voice |
| Radio | Narrow-band, walkie-talkie quality |
| Whisper | Breathy, filtered voice |

## 🛠️ Stack

- **Frontend:** Vanilla JavaScript, single HTML file (~23KB)
- **Audio:** Web Audio API (MediaStreamSource, OscillatorNode, BiquadFilterNode, ConvolverNode, DelayNode, AnalyserNode)
- **Visualization:** Canvas 2D waveform + frequency bars
- **Recording:** MediaRecorder API → WebM export
- **Fonts:** [Space Grotesk](https://fonts.google.com/specimen/Space+Grotesk) + [Outfit](https://fonts.google.com/specimen/Outfit) + [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono)
- **Hosting:** GitHub Pages (zero infra cost)

## 🚀 Quick Start

```bash
git clone https://github.com/gyoomei/mimovoice.git
open mimovoice/index.html
# or
python3 -m http.server 8080 -d mimovoice
```

## 📄 License

MIT — free for personal and commercial use.

**Built with 🧠 [Xiaomi MiMo V2.5](https://mimo.xiaomi.com/) · Submitted to [MiMo 100T program](https://mimo.xiaomi.com/)**
