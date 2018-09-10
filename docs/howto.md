HOWTO
========
Here are recipes for running analyses on different platforms.
Before following these instructions, make sure you have completed installation and possible account setup detailed in [installation instructions](installation.md). 

Local with Docker
-------------------
The purpose is to run a Single Ended, unstranded experiment on a local computer. 

1. Get the code:

```bash
  $ git clone https://github.com/ENCODE-DCC/rna-seq-pipeline
  $ cd rna-seq-pipeline
```

2. Get STAR and kallisto index files.

```bash
  $ curl https://storage.googleapis.com/star-rsem-runs/reference-genomes/GRCh38_v24_ERCC_phiX_starIndex_chr19only.tgz -o test_data/GRCh38_v24_ERCC_phiX_starIndex_chr19only.tgz
  $ curl https://storage.googleapis.com/star-rsem-runs/reference-genomes/Homo_sapiens.GRCh38.cdna.all.chr19_ERCC_phix_k31_kallisto.idx -o test_data/Homo_sapiens.GRCh38.cdna.all.chr19_ERCC_phix_k31_kallisto.idx 
``` 

The other data that is required to complete this recipe is included in the repository within test_data directory.

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