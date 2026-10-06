# FLAC with Exact Audio Copy

This guide shows how to configure the **FLAC reference encoder** as an external lossless encoder in Exact Audio Copy and provides practical command-line examples for common FLAC compression levels.

For current Windows binaries:

https://www.eaccodecs.com/downloads/

For the expanded EAC Codecs setup guide:

https://www.eaccodecs.com/manual/flac/

## Basic EAC configuration

Open:

**EAC → Compression Options → External Compression**

Enable:

**Use external program for compression**

Recommended basic settings:

| Setting | Value |
|---|---|
| Parameter passing scheme | User Defined Encoder |
| File extension | `.flac` |
| Program | Path to `flac.exe` |
| Use CRC check | Optional |
| Add ID3 tag | Disabled for normal FLAC use |
| Delete WAV after compression | Normally enabled |
| Bit rate | Not used / lossless |

The command examples below are entered in EAC's:

**Additional command-line options**

field.

## Recommended FLAC command

For a high-compression, verified FLAC encode:

```text
-8 -V -o "%dest%" "%source%"
```

This combines:

- FLAC compression level 8;
- verification of the encoded output;
- the EAC destination path;
- the temporary WAV supplied by EAC.

For most archival EAC use, this is a sensible default.

## Compression levels

FLAC supports compression levels from:

```text
-0
```

through:

```text
-8
```

Higher compression levels generally spend more processing time attempting to reduce file size.

They do **not** improve audio quality.

### Level 5

A balanced general-purpose setting:

```text
-5 -V -o "%dest%" "%source%"
```

### Level 8

Maximum standard compression:

```text
-8 -V -o "%dest%" "%source%"
```

Both commands are lossless.

A file produced at level 5 and a file produced at level 8 will decode to the same PCM audio when both were encoded correctly from the same source.

## Verification

The:

```text
-V
```

option instructs FLAC to verify the encoded data during compression.

For EAC ripping, this is useful because it adds an encoder-side integrity check immediately after the FLAC stream is created.

A verified level-8 command is:

```text
-8 -V -o "%dest%" "%source%"
```

A verified level-5 command is:

```text
-5 -V -o "%dest%" "%source%"
```

## Faster encoding

If encoding speed matters more than obtaining the smallest possible file, use a lower compression level.

For example:

```text
-5 -V -o "%dest%" "%source%"
```

or:

```text
-3 -V -o "%dest%" "%source%"
```

The resulting files may be somewhat larger, but the decoded audio remains identical.

## Maximum standard compression

For storage-focused use:

```text
-8 -V -o "%dest%" "%source%"
```

This is a good default when encoding speed is not particularly important.

## Basic command-line examples

Encode a WAV file at compression level 5:

```text
flac -5 -V input.wav -o output.flac
```

Encode a WAV file at compression level 8:

```text
flac -8 -V input.wav -o output.flac
```

These examples are useful when running FLAC directly outside Exact Audio Copy.

## EAC placeholders

The requested FLAC output path is passed using:

```text
"%dest%"
```

and the temporary WAV supplied by EAC is passed using:

```text
"%source%"
```

The recommended structure is:

```text
-o "%dest%" "%source%"
```

Keep both placeholders in quotation marks so Windows paths containing spaces are handled correctly.

## Metadata

FLAC uses Vorbis Comment metadata rather than ID3 tags.

For that reason, EAC's separate **Add ID3 tag** option should normally remain disabled for FLAC output.

Metadata can be supplied through FLAC command-line options or written by EAC using a FLAC-compatible tagging workflow.

FLAC supports tag options such as:

```text
-T "TITLE=..."
-T "ARTIST=..."
-T "ALBUM=..."
```

For example:

```text
-8 -V -T "TITLE=%title%" -T "ARTIST=%artist%" -T "ALBUM=%albumtitle%" -o "%dest%" "%source%"
```

If you choose to pass metadata directly to FLAC, test one rip first and verify that the expected tags are present.

## Recommended starting points

For balanced compression:

```text
-5 -V -o "%dest%" "%source%"
```

For maximum standard compression:

```text
-8 -V -o "%dest%" "%source%"
```

For somewhat faster encoding:

```text
-3 -V -o "%dest%" "%source%"
```

For level 8 with basic metadata passed directly to FLAC:

```text
-8 -V -T "TITLE=%title%" -T "ARTIST=%artist%" -T "ALBUM=%albumtitle%" -o "%dest%" "%source%"
```

## Testing the encoder

Before ripping a complete disc, encode one track and verify that:

1. EAC successfully starts `flac.exe`;
2. a FLAC file is produced;
3. FLAC verification completes without error;
4. the resulting file plays correctly;
5. the file is reported as lossless FLAC audio;
6. metadata is present if tagging has been configured;
7. the temporary WAV is removed if EAC is configured to remove it.

## Common problems

### EAC cannot start FLAC

Check that the configured program path points directly to:

```text
flac.exe
```

rather than only to the directory containing it.

### No output file is created

Check that both EAC placeholders are present:

```text
-o "%dest%" "%source%"
```

### Paths containing spaces fail

Keep the source and destination placeholders inside quotation marks:

```text
"%source%"
"%dest%"
```

### FLAC reports that the output file already exists

Check the destination configuration and make sure EAC is not attempting to reuse an existing filename.

### Higher compression sounds better

It does not.

FLAC compression levels affect compression effort and resulting file size, not decoded audio quality.

### Metadata is missing

Check whether metadata is being supplied through FLAC command-line options or by EAC.

Do not enable EAC's **Add ID3 tag** option for normal FLAC tagging, because FLAC does not use ID3 as its standard metadata system.

## More information

Full manual setup:

https://www.eaccodecs.com/manual/flac/

Windows binaries:

https://www.eaccodecs.com/downloads/

EAC Codecs:

https://www.eaccodecs.com/
