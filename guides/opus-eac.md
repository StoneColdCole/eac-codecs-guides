# CW-Opus with Exact Audio Copy

This guide shows how to configure **CW-Opus 1.6.1.0** as an external Opus encoder in Exact Audio Copy and provides practical command-line examples for the modes supported by the encoder.

For current Windows binaries:

https://www.eaccodecs.com/downloads/

For the expanded EAC Codecs setup guide:

https://www.eaccodecs.com/manual/

## Basic EAC configuration

Open:

**EAC → Compression Options → External Compression**

Enable:

**Use external program for compression**

Recommended basic settings:

| Setting | Value |
|---|---|
| Parameter passing scheme | User Defined Encoder |
| File extension | `.opus` |
| Program | Path to `cw-opus.exe` |
| Use CRC check | Disabled |
| Add ID3 tag | Disabled |
| Delete WAV after compression | Enabled |
| Bit rate | Controlled by the command line |
| Check for external programs return code | Disabled |

## Recommended CW protective cVBR

The current recommended EAC profile uses **CW protective cVBR at 256 kbit/s** with maximum encoder complexity.

```text
-cvbr -b 256000 --complexity 10 --title "%title%" --artist "%artist%" --albumartist "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" %source% -o %dest%
```

With CW-Opus, `-cvbr` together with an explicit `-b` value enables CW protective cVBR.

The requested bitrate acts as a minimum bitrate for eligible CELT frames while retaining VBR behaviour above that floor.

Do not interpret this as ordinary constrained VBR with a target bitrate.

## Basic command-line examples

Encode WAV using the default VBR mode:

```text
cw-opus.exe input.wav -o output.opus
```

Encode FLAC using the default VBR mode:

```text
cw-opus.exe input.flac -o output.opus
```

Encode FLAC using a requested bitrate of 192 kbit/s:

```text
cw-opus.exe -b 192000 input.flac -o output.opus
```

Encode FLAC using CW protective cVBR:

```text
cw-opus.exe -cvbr -b 192000 input.flac -o output.opus
```

Encode FLAC using standard constrained VBR:

```text
cw-opus.exe -cvbr input.flac -o output.opus
```

Encode FLAC using constant bitrate:

```text
cw-opus.exe -cbr -b 192000 input.flac -o output.opus
```

Decode an Opus file to 48 kHz PCM16 WAV:

```text
cw-opus.exe -d input.opus -o output.wav
```

## Standard VBR

VBR is the normal Opus mode when neither `-cvbr` nor `-cbr` is specified.

A basic VBR encode is:

```text
cw-opus.exe input.wav -o output.opus
```

To request a bitrate, use:

```text
-b <bits/s>
```

or:

```text
--bitrate <bits/s>
```

For example:

```text
cw-opus.exe -b 192000 input.wav -o output.opus
```

For high-quality EAC VBR at 256 kbit/s:

```text
-b 256000 --complexity 10 --title "%title%" --artist "%artist%" --albumartist "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" %source% -o %dest%
```

The default bitrate is:

```text
128000
```

bits per second.

## CW protective cVBR

CW protective cVBR uses:

```text
-cvbr -b <bits/s>
```

For example:

```text
cw-opus.exe -cvbr -b 192000 input.flac -o output.opus
```

With an explicit bitrate, `-cvbr` enables the CW protective cVBR mode.

The requested bitrate acts as a minimum for eligible CELT frames while allowing the encoder to use more bitrate when required.

For the current recommended EAC profile:

```text
-cvbr -b 256000 --complexity 10 --title "%title%" --artist "%artist%" --albumartist "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" %source% -o %dest%
```

## Standard constrained VBR

Use:

```text
-cvbr
```

without an explicit `-b` value for standard constrained VBR.

For example:

```text
cw-opus.exe -cvbr input.flac -o output.opus
```

For EAC:

```text
-cvbr --complexity 10 --title "%title%" --artist "%artist%" --albumartist "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" %source% -o %dest%
```

## Constant bitrate

Use:

```text
-cbr -b <bits/s>
```

for constant bitrate encoding.

For example:

```text
cw-opus.exe -cbr -b 192000 input.flac -o output.opus
```

For EAC at 256 kbit/s:

```text
-cbr -b 256000 --complexity 10 --title "%title%" --artist "%artist%" --albumartist "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" %source% -o %dest%
```

## Bitrate

CW-Opus accepts:

```text
-b <bits/s>
```

or:

```text
--bitrate <bits/s>
```

Examples:

### 128 kbit/s

```text
-b 128000 %source% -o %dest%
```

### 192 kbit/s

```text
-b 192000 %source% -o %dest%
```

### 256 kbit/s

```text
-b 256000 %source% -o %dest%
```

The default bitrate is 128000 bits per second.

## Complexity

CW-Opus supports encoder complexity values from:

```text
0
```

to:

```text
10
```

using either:

```text
-complexity <0..10>
```

or:

```text
--complexity <0..10>
```

For example:

```text
--complexity 10
```

The default complexity is 10.

The recommended EAC profile also uses complexity 10.

## Frame duration

CW-Opus supports the following frame durations:

```text
2.5
5
10
20
40
60
```

milliseconds.

Use either:

```text
-framesize <2.5|5|10|20|40|60>
```

or:

```text
--framesize <2.5|5|10|20|40|60>
```

For example:

```text
--framesize 20
```

The default frame duration is 20 ms.

## Sample rate

Normal CD audio from Exact Audio Copy is 44.1 kHz.

CW-Opus automatically resamples 44.1 kHz input to 48 kHz using its 64-tap Blackman-windowed sinc frontend.

No EAC sample-rate conversion is required.

Native Opus API sample rates bypass this frontend resampler.

Supported frontend sample rates are:

```text
8000
12000
16000
24000
44100
48000
```

Hz.

## WAV input

CW-Opus supports WAV input directly.

Supported built-in WAV formats include:

- PCM16
- PCM24
- PCM32
- Float32

Supported channel layouts are:

- mono
- stereo

## FLAC input

FLAC input is optional at runtime.

Place a matching:

```text
libFLAC.dll
```

beside:

```text
cw-opus.exe
```

to enable FLAC input.

WAV input remains available without `libFLAC.dll`.

For example:

```text
cw-opus.exe input.flac -o output.opus
```

or:

```text
cw-opus.exe -b 192000 input.flac -o output.opus
```

## Decode to WAV

CW-Opus can also decode Opus files.

To decode to 48 kHz PCM16 WAV:

```text
cw-opus.exe -d input.opus -o output.wav
```

## Metadata

CW-Opus writes Opus/Vorbis-comment metadata directly.

Supported convenience metadata includes:

- title
- artist
- album artist
- album
- genre
- date
- track numbering
- disc numbering
- comment
- composer
- performer
- ISRC
- description
- organization
- licence
- version tag
- copyright

The recommended EAC command includes:

```text
--title "%title%"
--artist "%artist%"
--albumartist "%albumartist%"
--album "%albumtitle%"
--genre "%genre%"
--date "%year%"
--track "%tracknr%/%numtracks%"
```

Arbitrary tags can also be added using:

```text
--tag NAME=VALUE
```

For example:

```text
--tag SOURCE=CD
```

Because CW-Opus writes OpusTags directly, EAC's separate **Add ID3 tag** option should remain disabled.

## Command reference

Display command-line help:

```text
cw-opus.exe --help
```

Display supported features:

```text
cw-opus.exe --features
```

Display version information:

```text
cw-opus.exe --version
```

## Recommended starting points

For default VBR:

```text
%source% -o %dest%
```

For VBR at 192 kbit/s:

```text
-b 192000 %source% -o %dest%
```

For high-quality VBR at 256 kbit/s with maximum complexity:

```text
-b 256000 --complexity 10 --title "%title%" --artist "%artist%" --albumartist "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" %source% -o %dest%
```

For the current recommended CW protective cVBR profile:

```text
-cvbr -b 256000 --complexity 10 --title "%title%" --artist "%artist%" --albumartist "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" %source% -o %dest%
```

For standard constrained VBR:

```text
-cvbr --complexity 10 --title "%title%" --artist "%artist%" --albumartist "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" %source% -o %dest%
```

For constant bitrate at 256 kbit/s:

```text
-cbr -b 256000 --complexity 10 --title "%title%" --artist "%artist%" --albumartist "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" %source% -o %dest%
```

For FLAC input outside EAC:

```text
cw-opus.exe -b 192000 input.flac -o output.opus
```

For decoding Opus to WAV:

```text
cw-opus.exe -d input.opus -o output.wav
```

## Testing the encoder

Before ripping a complete disc, encode one track and verify that:

1. an Opus file is produced;
2. the source WAV is removed if EAC is configured to remove it;
3. the resulting file plays correctly;
4. the expected bitrate mode is reported by a media-information tool;
5. metadata is present;
6. 44.1 kHz CD input has been handled correctly.

## Common problems

### EAC cannot start CW-Opus

Check that the path points directly to:

```text
cw-opus.exe
```

rather than only to the directory containing it.

### No output file is created

Check that both the input and output arguments are present:

```text
%source% -o %dest%
```

### `-cvbr -b` behaves differently from expected

With CW-Opus:

```text
-cvbr -b <bitrate>
```

enables CW protective cVBR.

For ordinary constrained VBR, use:

```text
-cvbr
```

without an explicit bitrate.

### 44.1 kHz input becomes 48 kHz

This is expected.

CW-Opus automatically resamples 44.1 kHz CD audio to 48 kHz before Opus encoding.

No EAC sample-rate conversion should be enabled.

### FLAC input does not work

Check that:

```text
libFLAC.dll
```

is located beside:

```text
cw-opus.exe
```

WAV input does not require `libFLAC.dll`.

### Metadata is duplicated

Disable EAC's separate **Add ID3 tag** option.

CW-Opus writes OpusTags directly.

## More information

Full manual setup:

https://www.eaccodecs.com/manual/

Windows binaries:

https://www.eaccodecs.com/downloads/

EAC Codecs:

https://www.eaccodecs.com/
