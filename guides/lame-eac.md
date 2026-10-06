# LAME MP3 with Exact Audio Copy

This guide shows how to configure **LAME** as an external MP3 encoder in Exact Audio Copy and provides practical command-line examples for standard LAME and the extended LAME build distributed through EAC Codecs.

For current Windows binaries:

https://www.eaccodecs.com/downloads/

For the expanded EAC Codecs setup guide:

https://www.eaccodecs.com/manual/lame/

## Basic EAC configuration

Open:

**EAC → Compression Options → External Compression**

Enable:

**Use external program for compression**

Recommended basic settings:

| Setting | Value |
|---|---|
| Parameter passing scheme | User Defined Encoder |
| File extension | `.mp3` |
| Program | Path to `lame.exe` |
| Use CRC check | Optional |
| Add ID3 tag | Depends on the chosen tagging setup |
| Delete WAV after compression | Normally enabled |
| Bit rate | Controlled by the command line |

The command examples below are entered in EAC's:

**Additional command-line options**

field.

## Recommended standard VBR command

For high-quality standard LAME VBR:

```text
-V0 "%source%" "%dest%"
```

For a smaller file size with a strong quality/size balance:

```text
-V2 "%source%" "%dest%"
```

Lower `-V` numbers represent higher quality.

Common settings include:

```text
-V0
-V1
-V2
-V3
-V4
```

## Recommended EAC Codecs extended mode

The extended LAME build distributed through EAC Codecs supports additional bitrate-control options.

For the high-quality cVBRb profile:

```text
-V0 -b 192 --vbr-min-strict --bitrate-boost=3 "%source%" "%dest%"
```

This combines:

- the `-V0` quality target;
- a 192 kbps minimum bitrate target;
- strict VBR minimum handling;
- bitrate boosting where the encoder determines that additional bitrate is useful.

These switches are specific to compatible extended LAME builds and should not be assumed to work with older standard LAME releases.

## Standard VBR

Variable bitrate is the normal choice for efficient high-quality MP3 encoding.

### V0

```text
-V0 "%source%" "%dest%"
```

### V1

```text
-V1 "%source%" "%dest%"
```

### V2

```text
-V2 "%source%" "%dest%"
```

### V3

```text
-V3 "%source%" "%dest%"
```

### V4

```text
-V4 "%source%" "%dest%"
```

For most EAC users, `-V0` or `-V2` are sensible starting points.

## Constant bitrate

Use:

```text
-b <kbps>
```

to request constant bitrate MP3 output.

### 128 kbps

```text
-b 128 "%source%" "%dest%"
```

### 192 kbps

```text
-b 192 "%source%" "%dest%"
```

### 256 kbps

```text
-b 256 "%source%" "%dest%"
```

### 320 kbps

```text
-b 320 "%source%" "%dest%"
```

CBR can be useful where predictable bitrate or broad hardware compatibility is more important than coding efficiency.

## Average bitrate

LAME can also operate in average-bitrate mode.

Use:

```text
--abr <kbps>
```

For example:

```text
--abr 128 "%source%" "%dest%"
```

```text
--abr 192 "%source%" "%dest%"
```

```text
--abr 256 "%source%" "%dest%"
```

ABR targets an average bitrate while allowing the instantaneous bitrate to change according to the audio.

## EAC Codecs extended cVBR / cVBRb modes

The EAC Codecs LAME build may provide additional modes beyond the traditional upstream LAME command set.

### cVBR

cVBR combines a VBR quality target with additional bitrate control.

The exact options available depend on the supplied LAME build.

### cVBRb

A high-quality cVBRb example is:

```text
-V0 -b 192 --vbr-min-strict --bitrate-boost=3 "%source%" "%dest%"
```

The command can be adjusted by changing the VBR quality target or bitrate floor where supported by the build.

For example:

```text
-V1 -b 192 --vbr-min-strict --bitrate-boost=3 "%source%" "%dest%"
```

or:

```text
-V0 -b 224 --vbr-min-strict --bitrate-boost=3 "%source%" "%dest%"
```

Use the EAC Codecs build if you want to use these extended switches.

## EAC placeholders

The source WAV created by EAC is passed with:

```text
"%source%"
```

and the requested MP3 output path is passed with:

```text
"%dest%"
```

Keep both placeholders in quotation marks so paths containing spaces are handled correctly.

A complete command therefore ends with:

```text
"%source%" "%dest%"
```

## Tagging

LAME and EAC can both participate in MP3 tagging depending on how the encoder is configured.

If your command line or encoder setup writes the desired tags directly, avoid enabling duplicate tagging in EAC.

If EAC is being used to add the MP3 tags after encoding, configure **Add ID3 tag** accordingly.

After a test rip, verify that artist, album, title, track number and other expected metadata are present only once and are stored correctly.

## Recommended starting points

For a good general-purpose VBR profile:

```text
-V2 "%source%" "%dest%"
```

For higher-quality standard VBR:

```text
-V0 "%source%" "%dest%"
```

For broadly compatible fixed-bitrate MP3:

```text
-b 192 "%source%" "%dest%"
```

For maximum standard CBR bitrate:

```text
-b 320 "%source%" "%dest%"
```

For the EAC Codecs extended high-quality profile:

```text
-V0 -b 192 --vbr-min-strict --bitrate-boost=3 "%source%" "%dest%"
```

## Testing the encoder

Before ripping a complete disc, encode one track and verify that:

1. EAC successfully starts `lame.exe`;
2. an MP3 file is produced;
3. the resulting file plays correctly;
4. the expected VBR, ABR, CBR or extended mode is reported by a media-information tool;
5. metadata is present if tagging has been configured;
6. the source WAV is removed if EAC is configured to remove it.

## Common problems

### EAC cannot start LAME

Check that the configured program path points directly to:

```text
lame.exe
```

rather than only to the directory containing it.

### No output file is created

Check that both EAC placeholders are present:

```text
"%source%" "%dest%"
```

### Paths containing spaces fail

Keep the source and destination placeholders inside quotation marks:

```text
"%source%"
"%dest%"
```

### An extended option is rejected

Options such as:

```text
--vbr-min-strict
--bitrate-boost=3
```

require a compatible extended LAME build.

Use the current EAC Codecs Windows binaries if you want to use the EAC Codecs-specific modes:

https://www.eaccodecs.com/downloads/

### Metadata is missing or duplicated

Check whether tagging is being handled by:

- LAME;
- EAC;
- or both.

Avoid configuring both to write the same metadata unless that behaviour is intentional.

## More information

Full manual setup:

https://www.eaccodecs.com/manual/lame/

Windows binaries:

https://www.eaccodecs.com/downloads/

EAC Codecs:

https://www.eaccodecs.com/
