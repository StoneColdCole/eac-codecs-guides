# CW-AAC with Exact Audio Copy

This guide shows how to configure **CW-AAC 1.0.1.0** as an external AAC encoder in Exact Audio Copy and provides practical command-line examples for the modes supported by the encoder.

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
| Program | Path to `cw-aac.exe` |
| Use CRC check | Disabled |
| Add ID3 tag | Disabled |
| Delete WAV after compression | Enabled |
| Bit rate | Controlled by the command line |

## Important positional argument rule

CW-AAC 1.0.1.0 expects the source and destination paths to be the **first two positional arguments**.

Keep:

```text
%source% %dest%
```

at the beginning of the command line.

For example:

```text
%source% %dest% -q 2
```

Do not place metadata or mode options before `%source% %dest%`.

## Recommended AAC-LC VBR

The recommended CW-AAC profile uses AAC-LC VBR quality 2.

```text
%source% %dest% -q 2 --title "%title%" --artist "%artist%" --band "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" --disk "%cdnumber%/%totalcds%"
```

This uses quality-based AAC-LC VBR while passing the main EAC metadata fields directly to CW-AAC.

## Basic command-line examples

Encode WAV to M4A using the default mode:

```text
cw-aac.exe input.wav output.m4a
```

Encode FLAC input using VBR quality 2:

```text
cw-aac.exe input.flac output.m4a -q 2
```

Encode WAV using protective VBR quality 2:

```text
cw-aac.exe input.wav output.m4a --pvbr -q 2
```

Encode WAV using bitrate mode at 256 kbps:

```text
cw-aac.exe input.wav output.m4a -b 256k
```

## VBR quality mode

CW-AAC supports quality-based VBR using:

```text
-q <0.1..10>
```

Examples:

```text
%source% %dest% -q 1
```

```text
%source% %dest% -q 2
```

```text
%source% %dest% -q 3
```

The recommended EAC profile uses:

```text
%source% %dest% -q 2
```

## Protective VBR

CW-AAC also supports protective VBR.

Syntax:

```text
--pvbr -q <1..5>
```

Examples:

```text
%source% %dest% --pvbr -q 1
```

```text
%source% %dest% --pvbr -q 2
```

```text
%source% %dest% --pvbr -q 3
```

```text
%source% %dest% --pvbr -q 4
```

```text
%source% %dest% --pvbr -q 5
```

The recommended pVBR EAC command with metadata is:

```text
%source% %dest% --pvbr -q 2 --title "%title%" --artist "%artist%" --band "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" --disk "%cdnumber%/%totalcds%"
```

The older:

```text
--cvbr
```

switch remains accepted as a legacy alias for:

```text
--pvbr
```

For example:

```text
%source% %dest% --cvbr -q 2
```

## Bitrate mode

CW-AAC can also operate in explicit bitrate mode.

Use:

```text
-b <bitrate>
```

Examples:

### 128 kbps

```text
%source% %dest% -b 128k
```

### 192 kbps

```text
%source% %dest% -b 192k
```

### 256 kbps

```text
%source% %dest% -b 256k
```

### 320 kbps

```text
%source% %dest% -b 320k
```

Bitrate mode can be useful where a predictable target bitrate is preferred over quality-based VBR.

## FLAC input

CW-AAC can accept FLAC input in addition to WAV.

For example:

```text
cw-aac.exe input.flac output.m4a -q 2
```

This can be useful for converting existing lossless files outside Exact Audio Copy.

## Copy existing metadata

CW-AAC can copy tags from a supported input file using:

```text
--copy-tags
```

For example:

```text
cw-aac.exe input.flac output.m4a -q 2 --copy-tags
```

This is particularly useful when converting an already-tagged FLAC file outside EAC.

## Metadata

CW-AAC writes M4A metadata directly.

Supported metadata switches include:

```text
--title <text>
--artist <text>
--album <text>
--album-artist <text>
--albumartist <text>
--band <text>
--genre <text>
--date <text>
--year <text>
--track <n[/total]>
--tracktotal <n>
--disc <n[/total]>
--disk <n[/total]>
--disctotal <n>
--comment <text>
--composer <text>
--copyright <text>
--encoder <text>
```

The recommended EAC command includes:

```text
--title "%title%"
--artist "%artist%"
--band "%albumartist%"
--album "%albumtitle%"
--genre "%genre%"
--date "%year%"
--track "%tracknr%/%numtracks%"
--disk "%cdnumber%/%totalcds%"
```

Useful aliases include:

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

Because CW-AAC writes M4A metadata itself, EAC's separate **Add ID3 tag** option should remain disabled.

## Statistics

Encoding statistics can be displayed using:

```text
--stats
```

For example:

```text
cw-aac.exe input.wav output.m4a -q 2 --stats
```

## Version information

Display the encoder version using:

```text
--version
```

or:

```text
-V
```

## Help

Display command-line help using:

```text
--help
```

or:

```text
-h
```

## Recommended starting points

For normal AAC-LC VBR:

```text
%source% %dest% -q 2
```

For AAC-LC VBR with full EAC metadata:

```text
%source% %dest% -q 2 --title "%title%" --artist "%artist%" --band "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" --disk "%cdnumber%/%totalcds%"
```

For protective VBR:

```text
%source% %dest% --pvbr -q 2
```

For protective VBR with full EAC metadata:

```text
%source% %dest% --pvbr -q 2 --title "%title%" --artist "%artist%" --band "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" --disk "%cdnumber%/%totalcds%"
```

For explicit 256 kbps bitrate mode:

```text
%source% %dest% -b 256k
```

For tagged FLAC conversion outside EAC:

```text
cw-aac.exe input.flac output.m4a -q 2 --copy-tags
```

## Testing the encoder

Before ripping a complete disc, encode one track and verify that:

1. an M4A file is produced;
2. the source WAV is removed if EAC is configured to remove it;
3. the resulting file plays correctly;
4. the expected AAC mode is reported by a media-information tool;
5. metadata is present;
6. the file reports AAC-LC audio.

## Common problems

### EAC cannot start CW-AAC

Check that the path points directly to:

```text
cw-aac.exe
```

rather than only to the directory containing it.

### No output file is created

Check that the source and destination placeholders are present:

```text
%source% %dest%
```

and that they appear at the beginning of the command line.

### Metadata options cause the encoder to fail

CW-AAC expects:

```text
%source% %dest%
```

before metadata and mode options.

Do not move metadata options ahead of the input and output paths.

### The selected quality value is rejected

Normal VBR uses:

```text
-q <0.1..10>
```

Protective VBR uses:

```text
--pvbr -q <1..5>
```

Use a quality value valid for the selected mode.

### Duplicate or incorrect metadata appears

Disable EAC's separate **Add ID3 tag** option.

CW-AAC writes M4A metadata directly.

### A legacy cVBR command is being used

The older:

```text
--cvbr
```

switch is accepted as an alias for:

```text
--pvbr
```

For new configurations, use `--pvbr`.

## More information

Full manual setup:

https://www.eaccodecs.com/manual/

Windows binaries:

https://www.eaccodecs.com/downloads/

EAC Codecs:

https://www.eaccodecs.com/
