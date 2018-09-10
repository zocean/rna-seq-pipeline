HOWTO
========
Here are recipes for running analyses on different platforms.
Before following these instructions, make sure you have completed installation and possible account setup detailed in [installation instructions](installation.md). 

RUNNING THE PIPELINE
=====================

Local with Docker
-------------------
The purpose is to run a Single Ended, unstranded experiment on a local computer. 

1. Get the code:

```bash
  $ git clone https://github.com/ENCODE-DCC/rna-seq-pipeline
  $ cd rna-seq-pipeline
```

2. Get STAR and kallisto index files:

```bash
  $ curl https://storage.googleapis.com/star-rsem-runs/reference-genomes/GRCh38_v24_ERCC_phiX_starIndex_chr19only.tgz -o test_data/GRCh38_v24_ERCC_phiX_starIndex_chr19only.tgz
  $ curl https://storage.googleapis.com/star-rsem-runs/reference-genomes/Homo_sapiens.GRCh38.cdna.all.chr19_ERCC_phix_k31_kallisto.idx -o test_data/Homo_sapiens.GRCh38.cdna.all.chr19_ERCC_phix_k31_kallisto.idx 
``` 

    The other data that is required to complete this recipe is included in the repository within test_data directory.

3. Set up the input:

    Copy the following into input.json, and then open it in your favorite text editor.

```
{
    "rna.endedness" : "single",
    "rna.fastqs_R1" : ["<path-to-repo>/rna-seq-pipeline/test_data/rep1_ENCSR510QZW_chr19only_10000_reads.fastq.gz","<path-to-repo>/rna-seq-pipeline/test_data/rep2_ENCSR510QZW_chr19only_10000_reads.fastq.gz"],
    "rna.aligner" : "star",
    "rna.bamroot" : "SE_unstranded",
    "rna.index" : "<path-to-repo>/rna-seq-pipeline/test_data/GRCh38_v24_ERCC_phiX_starIndex_chr19only.tgz",
    "rna.rsem_index" : "<path-to-repo>/rna-seq-pipeline/test_data/GRCh38_v24_ERCC_phiX_rsemIndex_chr19only.tgz",
    "rna.kallisto.kallisto_index" : "<path-to-repo>/rna-seq-pipeline/test_data/Homo_sapiens.GRCh38.cdna.all.chr19_ERCC_phix_k31_kallisto.idx",
    "rna.strandedness" : "unstranded",
    "rna.strandedness_direction" : "unstranded",
    "rna.chrom_sizes" : "<path-to-repo>/rna-seq-pipeline/test_data/GRCh38_EBV.chrom.sizes",
    "rna.align_ncpus" : 2,
    "rna.align_ramGB" : 4,
    "rna.disks" : "local-disk 20 HDD",
    "rna.kallisto.number_of_threads" : 2,
    "rna.kallisto.ramGB" : 4,
    "rna.kallisto.fragment_length" : 250,
    "rna.kallisto.sd_of_fragment_length" : 10 
}
```

Replace `<path-to-repo>` with the location you cloned the code into.





Local with Singularity
------------------------

Coming soon!


Google Cloud
______________


DNA Nexus
__________


SLURM with Singularity
------------------------

Coming soon!

BUILDING INDEXES
=================