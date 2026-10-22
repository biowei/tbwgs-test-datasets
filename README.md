# tbwgs-test-datasets

Test data for [biowei/tbwgs](https://github.com/biowei/tbwgs).

## Simulated reads (`reads/`)

Paired-end 2 x 150 bp reads simulated from the H37Rv reference in tbdb at commit
[`c4167762`](https://github.com/jodyphelan/tbdb/tree/c4167762ed361c679507e3f4ebe0cd277eae6102)
(`genome.fasta`, sha256 `7846c3926a60fd6a374a45494abbfd9f175696278ef191a410d6c4eba005a240`).
They are not real sequencing data. Use them only to check that the pipeline runs.

| File prefix    | Read pairs | Approximate depth | wgsim seed |
| -------------- | ---------- | ----------------- | ---------- |
| `TB_SIM1_L001` | 4000       | 0.27x             | 11         |
| `TB_SIM1_L002` | 4000       | 0.27x             | 12         |
| `TB_SIM2_L001` | 8000       | 0.54x             | 21         |

`TB_SIM1` has two lanes so that the test covers lane merging.

Command (wgsim 1.0), then `gzip -n`:

```bash
wgsim -N <pairs> -1 150 -2 150 -d 400 -s 50 -e 0.002 -r 0.0005 -R 0.1 -S <seed> \
    genome.fasta <prefix>_R1.fastq <prefix>_R2.fastq
```

## Samplesheets (`samplesheet/`)

- `samplesheet_test.csv`: used by `-profile test`.

## Frozen tbdb (`tbdb/`)

`who_v2plus_c4167762.tar.gz` is the TB-Profiler database built from the `who_v2+` branch of
[tbdb](https://github.com/jodyphelan/tbdb) at commit
[`c4167762`](https://github.com/jodyphelan/tbdb/tree/c4167762ed361c679507e3f4ebe0cd277eae6102).
Its `genome.fasta` and `mask.bed` are byte-identical to the files at that commit. The sha256 is in
`tbdb/SHA256SUMS`.

Command (TB-Profiler 6.7.0), then a reproducible archive of the database directory:

```bash
tb-profiler update_tbdb --branch who_v2+ --commit c4167762ed361c679507e3f4ebe0cd277eae6102 \
    --prefix who_v2plus_c4167762 --db_dir db
tar --sort=name --owner=0 --group=0 --numeric-owner --mtime='2026-10-21 00:00Z' \
    -C db -cf - who_v2plus_c4167762 | gzip -n -9 > who_v2plus_c4167762.tar.gz
```
