# CW-ALAC with Exact Audio Copy

This guide shows how to configure **CW-ALAC 1.0.1.0** as an external Apple Lossless encoder in Exact Audio Copy and provides practical command-line examples for the modes supported by the encoder.

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
| File extension | `.m4a` |
| Program | Path to `cw-alac.exe` |
| Use CRC check | Disabled |
| Add ID3 tag | Disabled |
| Delete WAV after compression | Enabled |
| Bit rate | Not used / lossless |
| Check for external programs return code | Enabled |

## Recommended ALAC command

CW-ALAC produces lossless ALAC audio in an M4A container.

The recommended EAC command is:

```text
--title "%title%" --artist "%artist%" --band "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" -o %dest% %source%
```

This passes the main EAC metadata fields directly to CW-ALAC while encoding the source audio losslessly.

## Full metadata command

For additional disc numbering, lyrics and cover artwork:

```text
--title "%title%" --artist "%artist%" --band "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" --disk "%cdnumber%/%totalcds%" %haslyrics%--lyrics "%lyrics%"%haslyrics% %hascover%--artwork "%coverfile%"%hascover% -o %dest% %source%
```

Track and disc values may be supplied either as a single number or as number/total.

## Basic command-line examples

Encode WAV to ALAC in an M4A container:

```text
cw-alac.exe input.wav output.m4a
```

Encode FLAC input to ALAC:

```text
cw-alac.exe input.flac -o output.m4a
```

Replace an existing output file:

```text
cw-alac.exe input.wav -o output.m4a --overwrite
```

Use the faster ALAC encoding path:

```text
cw-alac.exe --fast input.wav -o output.m4a
```

Both the normal and fast modes remain lossless.

## Lossless encoding

ALAC is a lossless audio codec.

CW-ALAC does not resample audio or intentionally alter the decoded PCM signal.

There is no bitrate or quality setting to select in EAC.

The normal lossless ALAC encoding path is used by default.

A basic conversion is:

```text
cw-alac.exe input.wav output.m4a
```

or:

```text
cw-alac.exe input.wav -o output.m4a
```

## Fast mode

Apple's faster ALAC encoding path can be selected using:

```text
--fast
```

For example:

```text
cw-alac.exe --fast input.wav -o output.m4a
```

The fast path changes encoding speed and compression behaviour, but the decoded audio remains lossless.

## Overwrite existing output

CW-ALAC does not silently replace an existing output file.

To intentionally replace an existing file, use:

```text
--overwrite
```

For example:

```text
cw-alac.exe input.wav -o output.m4a --overwrite
```

## WAV and RF64 input

CW-ALAC supports WAV and RF64 input directly.

Supported input includes integer PCM at ALAC-supported bit depths, with mono, stereo and multichannel audio.

Release validation covers:

```text
16-bit PCM
20-bit PCM
24-bit PCM
32-bit PCM
```

and sample rates up to:

```text
192 kHz
```

Validation also covers mono, stereo and 5.1 audio.

## FLAC input

CW-ALAC can also accept FLAC input through `libFLAC.dll`.

Keep the matching:

```text
libFLAC.dll
```

beside:

```text
cw-alac.exe
```

For example:

```text
cw-alac.exe input.flac -o output.m4a
```

WAV and RF64 encoding remain available without `libFLAC.dll`.

## Multichannel audio

CW-ALAC supports multichannel input.

For multichannel material, conventional WAV/FLAC channel order is converted to the channel order required by ALAC before encoding.

## Metadata

CW-ALAC writes standard M4A/iTunes metadata directly.

Supported metadata includes:

- title;
- artist;
- album;
- album artist;
- genre;
- date/year;
- track numbering;
- disc numbering;
- comment;
- composer;
- lyrics;
- copyright;
- encoder identification;
- JPEG cover artwork;
- PNG cover artwork.

The recommended EAC command includes:

```text
--title "%title%"
--artist "%artist%"
--band "%albumartist%"
--album "%albumtitle%"
--genre "%genre%"
--date "%year%"
--track "%tracknr%/%numtracks%"
```

For fuller metadata, add:

```text
--disk "%cdnumber%/%totalcds%"
%haslyrics%--lyrics "%lyrics%"%haslyrics%
%hascover%--artwork "%coverfile%"%hascover%
```

Because CW-ALAC writes metadata directly into the M4A file, EAC's separate **Add ID3 tag** option should remain disabled.

## CW-style metadata aliases

Equivalent aliases include:

```text
--albumartist
--album-artist
--band
```

for album artist,

```text
--disc
--disk
```

for disc numbering, and:

```text
--date
--year
```

for date/year.

Track and disc values may be supplied either as:

```text
3
```

or:

```text
3/12
```

depending on whether a total is also available.

## Output format

CW-ALAC writes ALAC audio in an M4A container.

The current M4A writer uses a 32-bit `mdat` atom.

An encoded media payload above 4 GiB is rejected rather than writing an unvalidated large-file layout.

For normal CD ripping this is unlikely to be relevant, but it can matter for very long or high-resolution material.

## Runtime features

Display the features available in the current runtime environment using:

```text
cw-alac.exe --features
```

This is useful for confirming optional runtime support such as FLAC input.

## Help

Display the complete current command-line option list using:

```text
cw-alac.exe --help
```

## Recommended starting points

For normal EAC use:

```text
--title "%title%" --artist "%artist%" --band "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" -o %dest% %source%
```

For fuller EAC metadata:

```text
--title "%title%" --artist "%artist%" --band "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" --disk "%cdnumber%/%totalcds%" %haslyrics%--lyrics "%lyrics%"%haslyrics% %hascover%--artwork "%coverfile%"%hascover% -o %dest% %source%
```

For a simple WAV conversion outside EAC:

```text
cw-alac.exe input.wav output.m4a
```

For FLAC input:

```text
cw-alac.exe input.flac -o output.m4a
```

For faster lossless encoding:

```text
cw-alac.exe --fast input.wav -o output.m4a
```

To intentionally replace an existing output:

```text
cw-alac.exe input.wav -o output.m4a --overwrite
```

## Testing the encoder

Before ripping a complete disc, encode one track and verify that:

1. an M4A file is produced;
2. the source WAV is removed if EAC is configured to remove it;
3. the resulting file plays correctly;
4. the audio codec is reported as ALAC;
5. metadata is present;
6. cover artwork or lyrics are present where configured;
7. the decoded audio remains lossless.

## Common problems

### EAC cannot start CW-ALAC

Check that the path points directly to:

```text
cw-alac.exe
```

rather than only to the directory containing it.

### No output file is created

Check that both the output and input placeholders are present:

```text
-o %dest% %source%
```

### Metadata is duplicated

Disable EAC's separate **Add ID3 tag** option.

CW-ALAC writes metadata directly into the M4A file.

### FLAC input does not work

Check that the matching:

```text
libFLAC.dll
```

is located beside:

```text
cw-alac.exe
```

WAV and RF64 input do not require `libFLAC.dll`.

### An existing output file is not replaced

Use:

```text
--overwrite
```

when running CW-ALAC manually and intentionally replacing an existing file.

### Fast mode changes audio quality

It does not.

Both the normal and `--fast` encoding paths are lossless.

### Different ALAC file sizes imply different audio quality

They do not.

Lossless compression can produce different file sizes while still decoding to the same PCM audio.

### A very large output is rejected

CW-ALAC currently uses a 32-bit `mdat` atom and rejects an encoded media payload above 4 GiB.

## More information

Full manual setup:

https://www.eaccodecs.com/manual/

Windows binaries:

https://www.eaccodecs.com/downloads/

EAC Codecs:

https://www.eaccodecs.com/
