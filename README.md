# Steganography Lab: Tool Comparison and Forensic Analysis

**Course:** TU863 BSc Digital Forensics & Cybersecurity, TU Dublin
**Date:** November 2026
**Type:** Individual lab project
**Full report:** [Lab05_Steganography_Report.pdf](Lab05_Steganography_Report.pdf)

## Purpose
Hide the same short text message in an image and an audio file using three
open-source steganography tools, then use hashing and hex comparison to
document exactly what each tool changed. This is the same comparison an
investigator makes when a suspect file and a known original are both available.

## Tools and methods
| Tool | Carrier | Method (as observed) | Password |
|---|---|---|---|
| Stegpy 0.2.0 | PNG, WAV | LSB substitution, 2 bits per byte | Yes (payload encrypted) |
| Stegano 3.0.0 | PNG | LSB substitution in colour channels | No |
| OpenStego 0.8.6 | PNG | LSB-based, spread across the image | Yes (AES128) |

Also used: Python 3.14 virtual environment, OpenJDK Temurin 25.0.5,
`shasum -a 256`, `md5`, `cmp`, `diff`, Beyond Compare (Hex Compare), AudioMass,
macOS `afconvert`. Platform: MacBook Air (Apple M4), macOS.

## What I did
- Installed and ran all three tools, and solved setup problems (path handling in
  Stegpy, running OpenStego from the GitHub release zip because Homebrew was not writable).
- Converted the MP3 to uncompressed WAV and trimmed it to produce a usable audio carrier.
- Embedded the same 64-byte payload in each carrier, extracted it, and verified
  each recovery with `diff`.
- Calculated SHA-256 and MD5 for every original and stego file.
- Compared originals and stego files byte by byte with `cmp` and Beyond Compare.
- Answered four analysis questions on investigating hidden data, steganography
  vs encryption, real incidents, and key researchers in the field.

## Key findings
- **Every stego file changed hash.** Both MD5 and SHA-256 detected all modifications,
  and the originals kept their baseline hashes throughout.
- **The WAV kept the same size** (7,942,468 bytes), so file size alone would not
  reveal the change. Only 665 of ~7.9 million bytes differed, in one block starting
  at offset 0x2710.
- **The block size matched the maths:** 220 bytes x 8 bits / 2 bits per byte = 880 bytes.
- **The tools rewrote the PNGs.** Sizes changed by -7.7% (Stegpy), -5.3% (Stegano)
  and +35.2% (OpenStego), so a PNG byte comparison cannot show where the payload sits.
- **Re-saving removed metadata.** The original PNG's C2PA content-credentials chunk
  was missing from the Stegano output.
- **Hiding is not protecting.** Stegano has no password step, so anyone with the tool
  can read the message. Stegpy and OpenStego encrypt the payload first.

## Limitations
- Three of the four tools requested in the brief were completed. OpenPuff has no
  macOS version, so it was not used.
- Some interpretations (for example, Stegpy's 220-byte message size) are inferred
  and marked as such in the report.

## Evidence
The report contains 20 annotated screenshots covering tool installation,
embedding, extraction, verification, hash output and Beyond Compare views.

## Skills demonstrated
Steganography and steganalysis concepts, file hashing and integrity verification,
hex and binary analysis, evidence-handling practice (work on copies, record hashes
first), command-line tooling, technical report writing.

## Note
All test data is lab-only. No real case data or reused passwords are included.
