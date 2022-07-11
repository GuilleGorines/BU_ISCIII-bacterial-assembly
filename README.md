# ![BU-ISCIII Bacterial Assembly](docs/images/nf-core-assemblybacterias_logo.png)

**Nextflow pipeline for assembling bacterias**.

## Introduction

<!-- TODO nf-core: Write a 1-2 sentence summary of what data the pipeline is for and what it does -->
**BU-ISCIII Bacterial Assembly** is a bioinformatics best-practise analysis pipeline for

The pipeline is built using [Nextflow](https://www.nextflow.io), a workflow tool to run tasks across multiple compute infrastructures in a very portable manner. It comes with docker containers making installation trivial and results highly reproducible.

## Quick Start

1. Install [`nextflow`](https://nf-co.re/usage/installation) (`>=20.04.0`)

2. Install any of [`Docker`](https://docs.docker.com/engine/installation/), [`Singularity`](https://www.sylabs.io/guides/3.0/user-guide/), [`Podman`](https://podman.io/), [`Shifter`](https://nersc.gitlab.io/development/shifter/how-to-use/) or [`Charliecloud`](https://hpc.github.io/charliecloud/) for full pipeline reproducibility _(please only use [`Conda`](https://conda.io/miniconda.html) as a last resort; see [docs](https://nf-co.re/usage/configuration#basic-configuration-profiles))_

3. Download the pipeline and test it on a minimal dataset with a single command:

    ```bash
    nextflow run BU_ISCIII-bacterial-assembly/main.nf -profile test,<docker/singularity/podman/shifter/charliecloud/conda/institute>
    ```

    > Please check [nf-core/configs](https://github.com/nf-core/configs#documentation) to see if a custom config file to run nf-core pipelines already exists for your Institute. If so, you can simply use `-profile <institute>` in your command. This will enable either `docker` or `singularity` and set the appropriate execution settings for your local compute environment.

4. Start running your own analysis!

    ```bash
    nextflow run BU-ISCIII Bacterial Assembly -profile <docker/singularity/podman/shifter/charliecloud/conda/institute> --input '*_R{1,2}.fastq.gz'
    ```

## Pipeline Summary

By default, the pipeline currently performs the following steps:

* Sequencing quality control (`FastQC, version 0.11.9`)
* Sequence trimming (`FastP, version 0.23.2`)
* Identification of the bacterial organism (`KmerFinder, version 3.0`)
* Download of most abundant kmerfinder reference from the NCBI (`ad-hoc Python 3 scripts`)
* Assembly of reads (`UniCycler, version 0.4.8`)
* Assesment of assembly using the downloaded reference (`Quast, version 5.0.2`)
* Mapping of the reads against the reference (`Minimap 2, version 2.24`)
* Generating the bam from the alignment of the reference (`Samtools, version 1.14`)
* Overall pipeline run summaries (`MultiQC, version 1.11`)

## Credits

BU-ISCIII Bacterial Assembly was originally written by Guillermo Gorines. 


## Disclaimer
This pipeline, despite not being mantained by the nf-core community, follows its good-practise protocols, and has been created under usage of their nf-core tools.

## Citations

<!-- TODO nf-core: Add citation for pipeline after first release. Uncomment lines below and update Zenodo doi. -->
<!-- If you use  BU-ISCIII Bacterial Assembly for your analysis, please cite it using the following doi: [10.5281/zenodo.XXXXXX](https://doi.org/10.5281/zenodo.XXXXXX) -->

You can cite the `nf-core` publication as follows:

> **The nf-core framework for community-curated bioinformatics pipelines.**
>
> Philip Ewels, Alexander Peltzer, Sven Fillinger, Harshil Patel, Johannes Alneberg, Andreas Wilm, Maxime Ulysse Garcia, Paolo Di Tommaso & Sven Nahnsen.
>
> _Nat Biotechnol._ 2020 Feb 13. doi: [10.1038/s41587-020-0439-x](https://dx.doi.org/10.1038/s41587-020-0439-x).

In addition, references of tools and data used in this pipeline are as follows:

<!-- TODO nf-core: Add bibliography of tools and data used in your pipeline -->
