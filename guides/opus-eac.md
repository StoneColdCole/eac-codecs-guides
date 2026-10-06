# Opus with Exact Audio Copy

This guide shows how to configure an **Opus encoder** as an external encoder in Exact Audio Copy.

For current Windows binaries:

https://www.eaccodecs.com/downloads/

For the expanded EAC Codecs setup documentation:

https://www.eaccodecs.com/manual/

## What Opus is

Opus is a modern lossy audio codec designed for high efficiency across a wide range of bitrates.

For music encoding, Opus is particularly useful where good quality at relatively low bitrates is important.

Unlike MP3, Opus is not intended for universal legacy-device compatibility, so check playback support before choosing it for long-term use.

## Basic EAC configuration

Open:

**EAC → Compression Options → External Compression**

Enable:

**Use external program for compression**

Recommended basic configuration:

| Setting | Value |
|---|---|
| Parameter passing scheme | User Defined Encoder |
| File extension | `.opus` |
| Program | Path to the Opus encoder executable |
| Use CRC check | Optional |
| Delete WAV after compression | Normally enabled |

## Basic command

A typical Opus command uses a target bitrate.

Example:

```text
--bitrate 128 "%source%" "%dest%"
