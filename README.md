# ESMA MiCA register archive

A weekly, append-only, byte-for-byte archive of the five interim MiCA register files published by
the European Securities and Markets Authority.

ESMA overwrites these files in place and keeps no history. Once a file changes, the previous
version is gone. This repository keeps every version it observes, together with the SHA-256 of
the exact bytes retrieved and the timestamp of retrieval.

**History cannot be reconstructed after the fact at any price.** That is the only reason this
repository exists.

Archive begins **16 August 2026**. Nothing before that date was captured by this project.

---

## What is here

```
data/snapshots/YYYY-MM-DD/
  CASPS.csv            authorized crypto-asset service providers
  NCASP.csv            non-compliant entities, Article 110
  ARTZZ.csv            asset-referenced token issuers
  EMTWP.csv            e-money token issuers
  OTHER.csv            white papers for crypto-assets other than ART and EMT
  register-page.html   the ESMA register page as served that day
  manifest.json        retrieval timestamp, HTTP status, SHA-256, byte length and line count per file
```

Source files, unmodified:

```
https://www.esma.europa.eu/sites/default/files/2024-12/{CASPS,NCASP,ARTZZ,EMTWP,OTHER}.csv
```

A snapshot is committed every Monday at 06:15 UTC and tagged `snapshot-YYYY-MM-DD`. A week in
which no file changed is still recorded, because knowing that the register did not change is
itself data. A file that fails to download is recorded as a state in `manifest.json`, never
skipped and never retried into silence.

---

## What is not here, deliberately

This repository is a mirror with hashes. It contains no analysis of any kind.

- **No classification of any document or URL.** Nothing here says whether an address resolves,
  what it serves, or whether it conforms to anything.
- **No scores, no grades, no tiers, no rankings, no league tables.** Not now and not later.
- **No statements about any named firm.** The files say what ESMA published. This repository adds
  a hash and a timestamp and stops.
- **No natural-person data.** The register files are entity level.

Any measurement built on this archive is published separately, under its own methodology, and
is not part of this repository.

---

## Transformation disclosure

**None.** The CSV files in `data/snapshots/` are the bytes returned by ESMA, stored unchanged.
No parsing, no normalization, no re-encoding, no reformatting, no column changes, no row filtering.
The byte-of-record is what `curl` received.

The only material added by this project is `manifest.json`, which records the retrieval timestamp,
the HTTP status code, the SHA-256, the byte length and the line count of each file as retrieved.

---

## How to verify any file yourself

Every claim this repository makes is one command wide.

```bash
sha256sum data/snapshots/2026-08-16/OTHER.csv
```

Compare that against the `sha256` field for `OTHER` in the same directory's `manifest.json`.
To check the current live file against the most recent snapshot:

```bash
curl -s "https://www.esma.europa.eu/sites/default/files/2024-12/OTHER.csv" | sha256sum
```

If those two digests match, ESMA has not republished the file since the snapshot was taken.
If they differ, the register changed, and both versions are worth reading side by side.

Each snapshot is also an annotated git tag, so `git show snapshot-2026-08-16` pins the tree.

---

## Source, attribution and legal notice

Source data is published by the European Securities and Markets Authority and reproduced here
under the ESMA registers legal notice, which states:

> "Reproduction of all information on this site (REGISTERS information) is authorised except as
> otherwise stated, provided the source is acknowledged."

**ESMA does not endorse this archive and is not associated with it in any way.** ESMA has not
reviewed, approved, or commented on this repository. Any error here is ours.

This archive is maintained by Pharos Production. When citing it, please use:

```
ESMA MiCA register archive, built and maintained by Pharos Production
(https://pharosproduction.com).
Source data: ESMA, reproduced under its registers legal notice. Unmodified.
```

Licensing of each layer is set out in [`LICENSE.md`](LICENSE.md).

---

## Disclaimer

This archive is provided as-is, without warranty of any kind, express or implied.

It is not advice of any kind: not legal, not financial, not investment, not compliance advice.
It is a record of what one set of files served from one set of URLs at a set of points in time,
and it is nothing more than that.

The sole data source is ESMA, and the archive is limited to what that source published on the
dates recorded. It is a point-in-time record. A file that was correct when captured may be wrong
now, and a file absent from a snapshot may exist today.

Corrections are welcome and are handled as additions, never as edits: snapshots are immutable and
a correction arrives as a new entry carrying its own timestamp and reason. Open an issue, or write
to the maintainer address on https://pharosproduction.com.
