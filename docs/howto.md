HOWTO
========
Here are recipes for running analyses on different platforms.
Before following these instructions, make sure you have completed installation and possible account setup detailed in [installation instructions](installation.md). 

RUNNING THE PIPELINE
=====================

Local with Docker
-------------------
The purpose is to run a Single Ended, non strand specific experiment on a local computer. 

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

3. Set up the `input.json`:

    Copy the following into `input.json` in your favorite text editor.

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


4. Run the pipeline:

```
  $ java -jar -Dconfig.file=backends/backend.conf cromwell-34.jar run rna-seq-pipeline.wdl -i input.json -o workflow_opts/docker.json
```

5. See the outputs in `cromwell-executions/rna/[RUNHASH]`. See [reference](reference.md) for details about the output directory structure.


Google Cloud
--------------
The purpose is to run a Paired End, strand specific experiment on Google Cloud Platform.
Make sure you have completed the steps for installation and Google Cloud setup described in the [installation instructions](installation.md#google-cloud). The following assumes your Google Cloud project is `[YOUR_PROJECT]`, you have created a bucket into `gs://[YOUR_BUCKET_NAME]`, and also directories `inputs`, `output` and `reference` in the bucket.

1. Get the code and move to the repo directory:

```bash
  $ git clone https://github.com/ENCODE-DCC/rna-seq-pipeline
  $ cd rna-seq-pipeline
```

2. Get STAR and kallisto index files:

```bash
  $ curl https://storage.googleapis.com/star-rsem-runs/reference-genomes/GRCh38_v24_ERCC_phiX_starIndex_chr19only.tgz -o test_data/GRCh38_v24_ERCC_phiX_starIndex_chr19only.tgz
  $ curl https://storage.googleapis.com/star-rsem-runs/reference-genomes/Homo_sapiens.GRCh38.cdna.all.chr19_ERCC_phix_k31_kallisto.idx -o test_data/Homo_sapiens.GRCh38.cdna.all.chr19_ERCC_phix_k31_kallisto.idx 
``` 

3. Copy indexes, and input data into the cloud:

```bash
  $ gsutil cp test_data/ENCSR653DFZ* gs://[YOUR_BUCKET_NAME]/inputs/
  $ gsutil cp test_data/GRCh38_v24_ERCC_phiX_starIndex_chr19only.tgz gs://[YOUR_BUCKET_NAME]/reference/
  $ gsutil cp test_data/Homo_sapiens.GRCh38.cdna.all.chr19_ERCC_phix_k31_kallisto.idx gs://[YOUR_BUCKET_NAME]/reference/
  $ gsutil cp test_data/GRCh38_v24_ERCC_phiX_rsemIndex_chr19only.tgz gs://[YOUR_BUCKET_NAME]/reference/
  $ gsutil cp test_data/GRCh38_EBV.chrom.sizes gs://[YOUR_BUCKET_NAME]/reference/ 
```

4. Set up the `input.json`:

    Copy the following into `input.json` in your favorite text editor.

```
{
    "rna.endedness" : "paired",
    "rna.fastqs_R1" : ["gs://[YOUR_BUCKET_NAME]/inputs/ENCSR653DFZ_rep1_chr19_10000reads_R1.fastq.gz", "gs://[YOUR_BUCKET_NAME]/inputs/ENCSR653DFZ_rep2_chr19_10000reads_R1.fastq.gz"],
    "rna.fastqs_R2" : ["gs://[YOUR_BUCKET_NAME]/inputs/ENCSR653DFZ_rep1_chr19_10000reads_R2.fastq.gz", "gs://[YOUR_BUCKET_NAME]/inputs/ENCSR653DFZ_rep2_chr19_10000reads_R2.fastq.gz"],
    "rna.aligner" : "star",
    "rna.index" : "gs://[YOUR_BUCKET_NAME]/reference/GRCh38_v24_ERCC_phiX_starIndex_chr19only.tgz",
    "rna.rsem_index" : "gs://[YOUR_BUCKET_NAME]/reference/GRCh38_v24_ERCC_phiX_rsemIndex_chr19only.tgz",
    "rna.kallisto.kallisto_index" : "gs://[YOUR_BUCKET_NAME]/reference/Homo_sapiens.GRCh38.cdna.all.chr19_ERCC_phix_k31_kallisto.idx",
    "rna.bamroot" : "PE_stranded",
    "rna.strandedness" : "stranded",
    "rna.strandedness_direction" : "reverse",
    "rna.chrom_sizes" : "gs://[YOUR_BUCKET_NAME]/reference/GRCh38_EBV.chrom.sizes",
    "rna.align_ncpus" : 2,
    "rna.align_ramGB" : 4,
    "rna.disks" : "local-disk 20 HDD",
    "rna.kallisto.number_of_threads" : 2,
    "rna.kallisto.ramGB" : 4
}
```

Replace `[YOUR_BUCKET_NAME]` with the name of the actual bucket you created.

5. Run the pipeline:

```
  $ java -jar -Dconfig.file=backends/backend.conf -Dbackend.default=google -Dbackend.providers.google.config.project=[YOUR_PROJECT] -Dbackend.providers.google.config.root=gs://[YOUR_BUCKET_NAME]/output cromwell-34.jar run rna-seq-pipeline.wdl -i input.json -o workflow_opts/docker.json
```

Replace `[YOUR_PROJECT]` with the project id of the project you created, and `[YOUR_BUCKET_NAME]` with the name of the bucket you created.

6. See outputs in `gs://[YOUR_BUCKET_NAME]/outputs/rna/[RUNHASH]`. See [reference](reference.md) for details about the output directory structure.

DNA Nexus
-----------
The purpose is to run a Paired End, non strand specific experiment on DNA Nexus platform. Before starting, make sure you have created a DNA Nexus account, created a new project `[YOUR_PROJECT_NAME]`, installed the [DNA Nexus SDK](https://wiki.dnanexus.com/Downloads#DNAnexus-Platform-SDK), and downloaded dxWDL as detailed in the [installation instructions](installation.md#dna-nexus).

1. Get the code and move to the repo directory:

```bash
  $ git clone https://github.com/ENCODE-DCC/rna-seq-pipeline
  $ cd rna-seq-pipeline
```

2. Get STAR and kallisto index files:

```bash
  $ curl https://storage.googleapis.com/star-rsem-runs/reference-genomes/GRCh38_v24_ERCC_phiX_starIndex_chr19only.tgz -o test_data/GRCh38_v24_ERCC_phiX_starIndex_chr19only.tgz
  $ curl https://storage.googleapis.com/star-rsem-runs/reference-genomes/Homo_sapiens.GRCh38.cdna.all.chr19_ERCC_phix_k31_kallisto.idx -o test_data/Homo_sapiens.GRCh38.cdna.all.chr19_ERCC_phix_k31_kallisto.idx 
``` 

3. Go to [DNA Nexus website](https://www.dnanexus.com) and navigate to `[YOUR_PROJECT_NAME]`. Create a `test_run` directory with subdirectories `inputs`, `output`, `reference` and `workflow` (You can organize the directories any way you want, but this is one way to keep organized).

4. Upload files from `test_data` folder into your DNA Nexus project. Put `ENCSR142YZV_chr19only_10000_reads_R1.fastq.gz` and `ENCSR142YZV_chr19only_10000_reads_R2.fastq.gz` into `inputs` folder. Put `GRCh38_v24_ERCC_phiX_starIndex_chr19only.tgz`, `GRCh38_v24_ERCC_phiX_rsemIndex_chr19only.tgz`, `Homo_sapiens.GRCh38.cdna.all.chr19_ERCC_phix_k31_kallisto.idx` and `GRCh38_EBV.chrom.sizes` into `reference` folder.

5. Setup the `input.json`: 
    Copy the following into `input.json` in your favorite text editor.
```
{
    "rna.endedness" : "paired",
    "rna.fastqs_R1" : ["dx://[YOUR_PROJECT_NAME]:test_run/inputs/ENCSR142YZV_chr19only_10000_reads_R1.fastq.gz"],
    "rna.fastqs_R2" : ["dx://[YOUR_PROJECT_NAME]:test_run/inputs/ENCSR142YZV_chr19only_10000_reads_R2.fastq.gz"],
    "rna.aligner" : "star",
    "rna.index" : "dx://[YOUR_PROJECT_NAME]:test_run/reference/GRCh38_v24_ERCC_phiX_starIndex_chr19only.tgz",
    "rna.rsem_index" : "dx://[YOUR_PROJECT_NAME]:test_run/reference/GRCh38_v24_ERCC_phiX_rsemIndex_chr19only.tgz",
    "kallisto.kallisto_index" : "dx://[YOUR_PROJECT_NAME]:test_run/reference/Homo_sapiens.GRCh38.cdna.all.chr19_ERCC_phix_k31_kallisto.idx",
    "rna.bamroot" : "PE_unstranded",
    "rna.strandedness" : "unstranded",
    "rna.strandedness_direction" : "unstranded",
    "rna.chrom_sizes" : "dx://[YOUR_PROJECT_NAME]:test_run/reference/GRCh38_EBV.chrom.sizes",
    "rna.align_ncpus" : 2,
    "rna.align_ramGB" : 4,
    "rna.disks" : "local-disk 20 HDD",
    "kallisto.number_of_threads" : 2,
    "kallisto.ramGB" : 4    
}
```

Replace `[YOUR_PROJECT_NAME]` with the actual name of the project you created.

6. Compile the workflow:

```bash
  $ java -jar dxWDL-0.75.jar compile rna-seq-pipeline.wdl -project [YOUR_PROJECT_NAME] -f -folder /test_run/workflow -defaults input.json -extras workflow_opts/docker.json
```

7. Go to DNANexus [project page](https://platform.dnanexus.com/projects) and click on your project.

8. Move to the directory `/test_run/workflow`

9. You will find a DNA Nexus workflow called `rna` with all inputs and parameters defined. Click the `rna` workflow, and in the window that opens click `Workflow Actions` button in the upper right corner, and from the dropdown menu choose `Set output folder` and set `/test_run/output` as the output folder.

10. Click the green `Run as Analysis` button to start the pipeline. You will be automatically redirected into the Monitor tab, where you can observe the pipeline run.

11. When the pipeline is completed (15-20min) the outputs will appear in `/test_run/output` folder.


Local with Singularity
------------------------

Coming soon!


SLURM with Singularity
------------------------

Coming soon!

BUILDING INDEXES
=================