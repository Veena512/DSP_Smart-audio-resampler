# DSP_Smart-audio-resampler
SMART AUDIO RESAMPLER FOR STREAMING/VOIP QUALITY ADAPTATION

Voice-over-IP (VoIP) and streaming systems must keep audio intelligible while the available bandwidth, jitter and CPU headroom change from moment to moment. Codecs respond by switching between sample-rate tiers, but conventional resamplers use fixed conversion ratios, alias high frequency content into the speech band, and produce clicks and phase jumps when the ratio is changed mid-stream. 

This project designs a lightweight adaptive resampler that changes the sample rate on the fly. A Kaiser-window FIR anti-alias stage with constant group delay is combined with a Farrow cubic-Lagrange decimator that keeps its fractional phase across rate changes, and filter changes are cross-faded. 

A small neural-network controller (3-24-12 MLP with hysteresis) observes bandwidth, jitter and CPU availability and selects the codec tier (8/16/24/48 kHz) and the filter profile (taps and Kaiser beta) for every 20 ms frame. 

The system is evaluated in Python on fully synthetic speech-like audio and a four-state Markov network model. On frames that were delivered, the anti-aliased pipeline reaches 28.7 dB in-band SNR against 14.6 dB for a resampler without an anti-alias filter, and the adaptive system loses 9.5% of frames against 20.3% for a static 16 kHz link and 48.4% for a static 48 kHz link.

The project was further extended into a live communication prototype using WebRTC. A Python-based server was developed to connect two browser clients, allowing communication between two laptops over a private network using Tailscale. 

The existing adaptive controller and streaming resampling components were integrated into the server-side audio processing path. During testing, both laptops were able to access the dashboard, join the same room and establish a WebRTC ICE connection.

