# FLAC with Exact Audio Copy

This guide shows how to configure the **FLAC reference encoder** as an external lossless encoder in Exact Audio Copy.

For current Windows binaries:

https://www.eaccodecs.com/downloads/

For the expanded EAC Codecs setup guide:

https://www.eaccodecs.com/manual/flac/

## What FLAC does

FLAC provides **lossless audio compression**.

Unlike MP3, AAC or Opus, encoding audio to FLAC does not discard audio information. A correctly decoded FLAC file reproduces the original PCM audio exactly.

The FLAC compression level affects encoding effort and file size, but **not decoded audio quality**.

## Basic EAC configuration

Open:

**EAC → Compression Options → External Compression**

Enable:

**Use external program for compression**

Recommended basic configuration:

| Setting | Value |
|---|---|
| Parameter passing scheme | User Defined Encoder |
| File extension | `.flac` |
| Program | Path to `flac.exe` |
| Use CRC check | Optional |
| Delete WAV after compression | Normally enabled |

## Recommended command

A straightforward high-compression configuration is:

```text
-8 -V -o "%dest%" "%source%"
```

The important options are:

```text
-8
```

Use FLAC compression level 8.

```text
-V
```

Verify the encoded file after compression.

```text
-o "%dest%"
```

Specify the destination file.

```text
"%source%"
```

Specify the WAV file supplied by Exact Audio Copy.

## Compression levels

FLAC supports compression levels from 0 through 8.

For example:

```text
-5 -V -o "%dest%" "%source%"
```

or:

```text
-8 -V -o "%dest%" "%source%"
```

Higher settings generally spend more processing time attempting to reduce file size.

They do **not** improve audio quality.

A FLAC file created at level 0 and one created at level 8 will decode to the same PCM audio when both were encoded correctly from the same source.

## Why use `-V`?

The verify option instructs FLAC to verify the encoded result during compression.

For archival CD ripping this is a sensible additional integrity check:

```text
-V
```

A typical EAC command therefore becomes:

```text
-8 -V -o "%dest%" "%source%"
```

## Faster encoding

If maximum compression is unnecessary, a moderate setting can be used:

```text
-5 -V -o "%dest%" "%source%"
```

The resulting file may be slightly larger, but the decoded audio remains identical.

## Maximum standard compression

For storage-focused use:

```text
-8 -V -o "%dest%" "%source%"
```

This is a good default when encoding speed is not particularly important.

## Metadata

FLAC supports Vorbis Comment metadata.

Metadata can either be supplied through the command line or written by EAC after encoding, depending on the chosen configuration.

A technical command may contain tag options such as:

```text
-T "ARTIST=..."
-T "TITLE=..."
-T "ALBUM=..."
```

However, metadata handling varies with EAC configuration, so the minimal examples in this guide deliberately concentrate on the audio encoding stage.

The fuller EAC Codecs guide covers configuration in more detail:

https://www.eaccodecs.com/manual/flac/

## Testing the encoder

After configuration, rip a single track and check that:

1. EAC successfully launches `flac.exe`;
2. a `.flac` file is produced;
3. FLAC verification completes without error;
4. the file plays normally;
5. metadata is present if tagging is enabled;
6. the temporary WAV is removed if EAC is configured to remove it.

## Common problems

### EAC cannot find FLAC

Ensure the configured program path points directly to:

```text
flac.exe
```

### FLAC reports that the output file already exists

Check the destination configuration and make sure EAC is not attempting to reuse an existing filename.

### Paths with spaces fail

Use quotation marks around the source and destination:

```text
-o "%dest%" "%source%"
```

### Higher compression sounds better

It does not.

FLAC compression levels alter the compression process and resulting file size. All valid FLAC compression levels remain lossless.

### File sizes differ between compression levels

This is normal.

A higher compression setting may find a more compact representation of exactly the same audio data.

## Recommended configuration

For most archival EAC use:

```text
-8 -V -o "%dest%" "%source%"
```

For somewhat faster compression:

```text
-5 -V -o "%dest%" "%source%"
```

## More information

Full manual setup:

https://www.eaccodecs.com/manual/flac/

Windows binaries:

https://www.eaccodecs.com/downloads/

EAC Codecs:

https://www.eaccodecs.com/
