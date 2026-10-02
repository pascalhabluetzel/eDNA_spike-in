# eDNA_spike-in
Design of spike-in that can be simulataniously used for both metabarcoding and metagenomics

## Scripts

### `generate_spike_sequences.py`
Generates 5 candidate synDNA spike-in backbone sequences (2000 bp each), one per target GC content (26%, 36%, 46%, 56%, 66%), using [DNA Chisel](https://github.com/Edinburgh-Genome-Foundry/DnaChisel). Each sequence is constrained to: hold GC content within ±0.5% of its target, contain no self-complementary/hairpin stem ≥15 bp, and contain no exact repeated 15-mer. Outputs a FASTA file of the 5 sequences and a CSV summary (target vs. actual GC%, length, constraint pass/fail).

### `run_ncbi_novelty_screen.py`
Screens candidate sequences for novelty against NCBI's `nt` database via remote BLASTN (Biopython's `NCBIWWW.qblast`), flagging any hit with E-value < 0.01 — the same stringency used in Zaramela et al. (2022) to call a synthetic sequence free of meaningful identity to real genomic sequence. Takes a FASTA file as input and writes a pass/flag report per sequence. Requires unrestricted outbound internet access to `blast.ncbi.nlm.nih.gov`.

### `add_primer_pair.py`
Embeds a COI primer pair into each of the 5 backbone sequences at fixed coordinates, producing a 313 bp amplicon between them while keeping the rest of each 2000 bp construct — including the region between the primers — synthetic and GC/repeat-optimized as before. This makes each sequence usable both for direct shotgun-sequencing coverage quantification and for amplicon sequencing. Includes a dependency-free heuristic check for primer-primer dimer risk (complementary-run scan).

### Dependencies
`dnachisel`, `biopython` (`pip install dnachisel biopython`).
