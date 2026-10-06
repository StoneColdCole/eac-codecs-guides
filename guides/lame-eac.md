# LAME MP3 with Exact Audio Copy

This guide shows how to configure **LAME** as an external MP3 encoder in Exact Audio Copy.

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
| Add ID3 tag | Depends on command/tagging configuration |
| Delete WAV after compression | Normally enabled |

## LAME VBR

Variable bitrate is a good general-purpose choice for high-quality MP3 encoding.

### V0

```text
-V0 "%source%" "%dest%"
```

`-V0` is the highest standard LAME VBR quality setting.

Lower V values represent higher quality:

```text
-V0
-V1
-V2
-V3
-V4
```

A commonly used quality/size balance is:

```text
-V2 "%source%" "%dest%"
```

## Constant bitrate

Use `-b` to request a constant MP3 bitrate.

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

CBR can be useful where predictable bitrate or compatibility is more important than encoding efficiency.

## Average bitrate

LAME can also operate in average-bitrate mode.

Example:

```text
--abr 192 "%source%" "%dest%"
```

This targets an average bitrate of approximately 192 kbps while allowing the instantaneous bitrate to change according to the audio.

## LAME 4 / EAC Codecs extended modes

The LAME build distributed through EAC Codecs may provide additional modes beyond the traditional upstream LAME command set.

### cVBR

cVBR combines a VBR quality target with additional bitrate control.

The exact options available depend on the supplied LAME build.

### cVBRb

For highest-quality use with the EAC Codecs LAME build, an example configuration is:

```text
-V0 -b 192 --vbr-min-strict --bitrate-boost=3 "%source%" "%dest%"
```

This combines:

- the `-V0` quality target;
- a 192 kbps minimum bitrate target;
- strict VBR minimum handling;
- bitrate boosting where the encoder determines that additional bitrate is useful.

These options are specific to compatible LAME builds and should not be assumed to work with older standard LAME releases.

## Recommended starting points

For general use:

```text
-V2 "%source%" "%dest%"
```

For higher-quality VBR:

```text
-V0 "%source%" "%dest%"
```

For broadly compatible fixed-bitrate MP3:

```text
-b 192 "%source%" "%dest%"
```

For the EAC Codecs extended high-quality mode:

```text
-V0 -b 192 --vbr-min-strict --bitrate-boost=3 "%source%" "%dest%"
```

## Testing the encoder

Before ripping a complete disc, encode one track and verify that:

1. an MP3 file is produced;
2. the source WAV is removed if EAC is configured to remove it;
3. the resulting file plays correctly;
4. the expected bitrate/mode is reported by a media-information tool;
5. metadata is present if tagging has been configured.

## Common problems

### EAC cannot start LAME

Check that the path points directly to:

```text
lame.exe
```

rather than only to the directory containing it.

### No output file is created

Check that both EAC placeholders are present:

```text
"%source%" "%dest%"
```

### Command-line option is rejected

Some options, particularly cVBR/cVBRb extensions, require a compatible LAME build.

Use the current EAC Codecs Windows binaries if you want to use the EAC Codecs-specific modes:

https://www.eaccodecs.com/downloads/

### Paths containing spaces fail

Keep the source and destination placeholders inside quotation marks:

```text
"%source%"
"%dest%"
```

## More information

Full manual setup:

https://www.eaccodecs.com/manual/lame/

Windows binaries:

https://www.eaccodecs.com/downloads/

EAC Codecs:

https://www.eaccodecs.com/
