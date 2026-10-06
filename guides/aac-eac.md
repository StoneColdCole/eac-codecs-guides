# CW-AAC with Exact Audio Copy

This guide shows how to configure **CW-AAC** as an external AAC encoder in Exact Audio Copy.

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

## Recommended AAC-LC VBR

The recommended CW-AAC profile uses AAC-LC VBR quality 2.

```text
%source% %dest% -q 2 --title "%title%" --artist "%artist%" --band "%albumartist%" --album "%albumtitle%" --genre "%genre%" --date "%year%" --track "%tracknr%/%numtracks%" --disk "%cdnumber%/%totalcds%"
