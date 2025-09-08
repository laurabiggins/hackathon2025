# 1. Installation

Clone from GitHub repository hosted here: [https://github.com/comprna/SUPPA](https://github.com/comprna/SUPPA)

```
git@github.com:comprna/SUPPA.git
```

I installed it in my home directory under a folder called `GitHub`, i.e., `~/GitHub/SUPPA`, but anything can go here.

Create a conda/mamba/micromamba environment from YAML file:

```
micromamba create -n suppa2_env -f suppa2_env.yaml
```

This is the `suppa2_env.yaml` YAML file that we used:

```
name: suppa2_env
channels:
  - bioconda
  - conda-forge
dependencies:
  - python=3.9
  - pip
  - pip:
    - SUPPA==2.3
```

Note that even though SUPPA should be getting installed with the pip dependency manager, this fails. When running `pip show SUPPA` in the new micromamba environment, it points to a directory where the executable is not found.

Now activate the environment:

```
micromamba activate suppa2_env
```


# 2. Creating transcript events and local alternative splicing events files

First, copy the relevant GTF file:

```
aws s3 cp s3://altos-lab-reference-data/genomes/Homo_sapiens/Ensembl/GRCh38/Annotation/Genes/Homo_sapiens.GRCh38.108.gtf .
```

Then generate the transcript events file first (`.ioi` file):

```
python ~/GitHub/SUPPA/suppa.py generateEvents -i Homo_sapiens.GRCh38.108.gtf -o Homo_sapiens.GRCh38.108 -f ioi
```

This generates a file named `Homo_sapiens.GRCh38.108.ioi` with the following file structure:

```
seqname gene_id isoform_id      inclusion_transcripts   total_transcripts
1       ENSG00000160072 ENSG00000160072;ENST00000673477 ENST00000673477 ENST00000673477,ENST00000308647,ENST00000472194,ENST00000378736,ENST00000485748,ENST00000474481
1       ENSG00000160072 ENSG00000160072;ENST00000472194 ENST00000472194 ENST00000673477,ENST00000308647,ENST00000472194,ENST00000378736,ENST00000485748,ENST00000474481
1       ENSG00000160072 ENSG00000160072;ENST00000378736 ENST00000378736 ENST00000673477,ENST00000308647,ENST00000472194,ENST00000378736,ENST00000485748,ENST00000474481
1       ENSG00000160072 ENSG00000160072;ENST00000485748 ENST00000485748 ENST00000673477,ENST00000308647,ENST00000472194,ENST00000378736,ENST00000485748,ENST00000474481
1       ENSG00000160072 ENSG00000160072;ENST00000474481 ENST00000474481 ENST00000673477,ENST00000308647,ENST00000472194,ENST00000378736,ENST00000485748,ENST00000474481
1       ENSG00000160072 ENSG00000160072;ENST00000308647 ENST00000308647 ENST00000673477,ENST00000308647,ENST00000472194,ENST00000378736,ENST00000485748,ENST00000474481
1       ENSG00000279928 ENSG00000279928;ENST00000624431 ENST00000624431 ENST00000624431
1       ENSG00000228037 ENSG00000228037;ENST00000424215 ENST00000424215 ENST00000424215
1       ENSG00000142611 ENSG00000142611;ENST00000511072 ENST00000511072 ENST00000511072,ENST00000607632,ENST00000378391,ENST00000270722,ENST00000514189,ENST00000512462,ENST00000463591,ENST00000509860,ENST00000378389,ENST00000606170
```

Next, generate the local alternative splicing (AS) events file:

```
python ~/GitHub/SUPPA/suppa.py generateEvents -i Homo_sapiens.GRCh38.108.gtf -o Homo_sapiens.GRCh38.108 -f ioe -e SE SS MX RI FL
```