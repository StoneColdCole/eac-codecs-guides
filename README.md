# EAC Encoder Command Guides

Practical command-line and configuration guides for using external audio encoders with **Exact Audio Copy (EAC)**.

These guides are intended as concise technical references for configuring individual encoders manually in EAC.

## Guides

| Encoder | Format | Guide |
|---|---|---|
| LAME | MP3 | [LAME with Exact Audio Copy](guides/lame-eac.md) |
| FLAC | FLAC | [FLAC with Exact Audio Copy](guides/flac-eac.md) |
| AAC | AAC / M4A | Coming soon |
| ALAC | Apple Lossless | Coming soon |
| Opus | Opus | Coming soon |
| AC-3 | Dolby Digital | Coming soon |
| DTS | DTS | Coming soon |
| DTS-CD | DTS in CD audio | Coming soon |
| HDCD | HDCD decode | Coming soon |

## Windows binaries

Current Windows encoder binaries and the complete EAC Codecs package are available from:

https://www.eaccodecs.com/downloads/

EAC Codecs provides a collection of external audio encoders intended for use with Exact Audio Copy, together with manual setup information and configuration guidance.

## Manual setup documentation

The complete manual setup section is available at:

https://www.eaccodecs.com/manual/

The website contains expanded instructions, troubleshooting information and codec-specific setup pages.

## About these guides

The GitHub guides concentrate on:

- command-line syntax;
- useful encoder modes;
- Exact Audio Copy configuration;
- common setup problems;
- technical notes relevant to each encoder.

They deliberately remain relatively concise. The EAC Codecs website contains the fuller end-user documentation.

## Exact Audio Copy placeholders

When an encoder is configured as a **User Defined Encoder**, EAC can pass the temporary source file and requested destination filename to the external program.

The examples in this repository use:

```text
%source%
%dest%
```

Where appropriate, the placeholders are quoted so paths containing spaces are handled correctly.

## EAC Codecs

EAC Codecs is maintained by **Cole Williams Software Limited**.

Website:

https://www.eaccodecs.com/

Downloads:

https://www.eaccodecs.com/downloads/

## Disclaimer

Exact Audio Copy is a separate third-party application. EAC Codecs and these guides are not part of Exact Audio Copy itself.

Encoder names and trademarks remain the property of their respective owners.
