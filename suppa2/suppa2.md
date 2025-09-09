# SUPPA2 evaluation

## 1. Installation

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


## 2. Creating transcript events and local alternative splicing events files

First, copy the relevant GTF file:

```
aws s3 cp s3://altos-lab-reference-data/genomes/Homo_sapiens/Ensembl/GRCh38/Annotation/Genes/Homo_sapiens.GRCh38.108.gtf .
```

#### 2.1. Transcript events file (`.ioi`)

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

#### 2.1. Local alternative splicing (AS) files (`.ioe`)

Next, generate the local alternative splicing (AS) events file:

```
python ~/GitHub/SUPPA/suppa.py generateEvents -i Homo_sapiens.GRCh38.108.gtf -o Homo_sapiens.GRCh38.108 -f ioe -e SE SS MX RI FL
```

This generates a number of different files:

```
Homo_sapiens.GRCh38.108_A3_strict.gtf
Homo_sapiens.GRCh38.108_A3_strict.ioe
Homo_sapiens.GRCh38.108_A5_strict.gtf
Homo_sapiens.GRCh38.108_A5_strict.ioe
Homo_sapiens.GRCh38.108_AF_strict.gtf
Homo_sapiens.GRCh38.108_AF_strict.ioe
Homo_sapiens.GRCh38.108_AL_strict.gtf
Homo_sapiens.GRCh38.108_AL_strict.ioe
Homo_sapiens.GRCh38.108_MX_strict.gtf
Homo_sapiens.GRCh38.108_MX_strict.ioe
Homo_sapiens.GRCh38.108_RI_strict.gtf
Homo_sapiens.GRCh38.108_RI_strict.ioe
Homo_sapiens.GRCh38.108_SE_strict.gtf
Homo_sapiens.GRCh38.108_SE_strict.ioe
```

The general structure of the `.ioe` files is the following, as exemplied by the retained intron (`RI`) file:

```
seqname gene_id event_id        alternative_transcripts total_transcripts
1       ENSG00000160072 ENSG00000160072;RI:1:1485782:1485838-1486110:1486235:+  ENST00000474481 ENST00000673477,ENST00000474481,ENST00000308647,ENST00000472194,ENST00000485748
1       ENSG00000232596 ENSG00000232596;RI:1:4593052:4593190-4593700:4594009:+  ENST00000634256,ENST00000659083 ENST00000667354,ENST00000634256,ENST00000659083
1       ENSG00000116786 ENSG00000116786;RI:1:15727014:15727068-15727192:15727832:+      ENST00000375799,ENST00000375793 ENST00000375799,ENST00000375793,ENST00000642363
1       ENSG00000048707 ENSG00000048707;RI:1:12348823:12348973-12349164:12349374:+      ENST00000460333 ENST00000011700,ENST00000620676,ENST00000613099,ENST00000460333,ENST00000646917
1       ENSG00000157191 ENSG00000157191;RI:1:16458842:16459062-16459274:16460077:+      ENST00000443980,ENST00000496239 ENST00000492095,ENST00000443980,ENST00000496239
1       ENSG00000157191 ENSG00000157191;RI:1:16449093:16449201-16451838:16452015:+      ENST00000496239 ENST00000513161,ENST00000337132,ENST00000492095,ENST00000443980,ENST00000496239,ENST00000457722,ENST00000504551
1       ENSG00000204138 ENSG00000204138;RI:1:28496534:28497271-28499420:28500364:+      ENST00000373839 ENST00000632964,ENST00000373839
1       ENSG00000142675 ENSG00000142675;RI:1:26188234:26188307-26188442:26188503:+      ENST00000484874 ENST00000374253,ENST00000482227,ENST00000484874,ENST00000531191,ENST00000361530
1       ENSG00000157978 ENSG00000157978;RI:1:25563661:25563791-25565173:25565207:+      ENST00000488127 ENST00000488127,ENST00000484476,ENST00000374338
```

On top of that, we have `.gtf` files that will aid in visualisation process of the different local alternative splicing (AS) events.


## 3. Generate SUPPA2-compatible transcript matrices

We ran the `nf-core/rnaseq` pipeline on 4 samples of our choice (2 replicates of 2 cell lines) using default parameters. Version of the pipeline was 3.18.0. Alignment was done with `STAR` followed by quantification with `salmon`. 

The nf-core pipeline will generate the relevant outputs inside the `star_salmon` folder. `SUPPA2` requires a TPM quantification of the different transcripts, which are available in the `salmon.merged.transcript_tpm.tsv` file. This is how that file looks like:

```
tx      gene_id SGNex_H9_Illumina_replicate2_run1       SGNex_H9_Illumina_replicate3_run1       SGNex_K562_Illumina_replicate3_run1     SGNex_K562_Illumina_replicate4_run1
ENST00000456328 ENSG00000290825 0.023964        0.027742        0.188112        0
ENST00000450305 ENSG00000223972 0       0       0       0
ENST00000488147 ENSG00000227232 6.946638        6.280138        25.225222       29.855285
ENST00000619216 ENSG00000278267 0       0       0       1.12798
ENST00000473358 ENSG00000243485 0       0       0       0.190257
ENST00000469289 ENSG00000243485 0       0.199847        0       0
ENST00000607096 ENSG00000284332 0       0       0       0
ENST00000417324 ENSG00000237613 0       0       0       0
ENST00000461467 ENSG00000237613 0       0       0       0
```

However, the file format for multi-sample matrices needs to be the following:

```
sample1 sample2 sample3 sample4
transcript1 <expression>  <expression>  <expression>  <expression>
transcript2 <expression>  <expression>  <expression>  <expression>
transcript3 <expression>  <expression>  <expression>  <expression>
```

Therefore, we'll need to remove column 2 and remove the header of the first column with this one-liner:

```
cut -f1,3- salmon.merged.transcript_tpm.tsv | sed '1s/tx\t//' > salmon.merged.transcript_tpm.suppa2_compatible.tsv
```

This is the output file:

```
SGNex_H9_Illumina_replicate2_run1       SGNex_H9_Illumina_replicate3_run1       SGNex_K562_Illumina_replicate3_run1     SGNex_K562_Illumina_replicate4_run1
ENST00000456328 0.023964        0.027742        0.188112        0
ENST00000450305 0       0       0       0
ENST00000488147 6.946638        6.280138        25.225222       29.855285
ENST00000619216 0       0       0       1.12798
ENST00000473358 0       0       0       0.190257
ENST00000469289 0       0.199847        0       0
ENST00000607096 0       0       0       0
ENST00000417324 0       0       0       0
ENST00000461467 0       0       0       0
```

Lastly, we need to separate them by experimental conditions, that is, by cell line (H9 vs. K562). I'll run a simple awk command to do that:

```
awk 'BEGIN {OFS="\t"} NR==1 {print "SGNex_H9_Illumina_replicate2_run1", "SGNex_H9_Illumina_replicate3_run1" > "salmon.merged.transcript_tpm.suppa2_compatible.H9_samples.tsv"; print "SGNex_K562_Illumina_replicate3_run1", "SGNex_K562_Illumina_replicate4_run1" > "salmon.merged.transcript_tpm.suppa2_compatible.K562_samples.tsv"} NR>1 {print $1, $2, $3 > "salmon.merged.transcript_tpm.suppa2_compatible.H9_samples.tsv"; print $1, $4, $5 > "salmon.merged.transcript_tpm.suppa2_compatible.K562_samples.tsv"}' salmon.merged.transcript_tpm.suppa2_compatible.tsv
```

Ready for PSI quantifications!

## 4. PSI quantification

#### 4.1. Per isoform

Let's calculate the PSI metrics at the isoform level for both cell lines:

```
for cell_line in H9 K562; do
  echo "---Processing $cell_line...---"
  python ~/GitHub/SUPPA/suppa.py psiPerIsoform -g Homo_sapiens.GRCh38.108.gtf -e salmon.merged.transcript_tpm.suppa2_compatible.${cell_line}_samples.tsv -o salmon_${cell_line}
done
```

This is the structure of the output files, exemplified by the H9 cell line `salmon.psi_H9_isoform.psi` file:

```
SGNex_H9_Illumina_replicate2_run1       SGNex_H9_Illumina_replicate3_run1
ENSG00000160072;ENST00000673477 0.0556391486741741      0.058875536887111043
ENSG00000160072;ENST00000308647 0.36907428557689037     0.404610619197373
ENSG00000160072;ENST00000472194 0.061046658147855815    0.0635133669560327
ENSG00000160072;ENST00000378736 0.4245191659340324      0.36274415757212275
ENSG00000160072;ENST00000485748 0.05599366712261248     0.07328193228536424
ENSG00000160072;ENST00000474481 0.0337270745444348      0.03697438710199621
ENSG00000279928;ENST00000624431 nan     nan
ENSG00000228037;ENST00000424215 nan     nan
ENSG00000142611;ENST00000511072 0.0     0.0
```

#### 4.2. Per local alternative splicing (AS) event

Now let's calculate those metrics for all the different types of local AS events for which we have generated `.ioe` files beforehand, for both cell lines:

```
for cell_line in H9 K562; do
  echo "---Processing $cell_line...---"
  for event_type in A3 A5 AF AL MX RI SE; do
      echo "Processing $event_type events..."
      python ~/GitHub/SUPPA/suppa.py psiPerEvent \
          --ioe-file Homo_sapiens.GRCh38.108_${event_type}_strict.ioe \
          --expression-file salmon.merged.transcript_tpm.suppa2_compatible.${cell_line}_samples.tsv \
          -o salmon_${cell_line}_${event_type}
  done
done
```

This is the structure of one of the output files, as exemplified by the retained intron (`RI`) file for the H9 cell line:

```
SGNex_H9_Illumina_replicate2_run1       SGNex_H9_Illumina_replicate3_run1
ENSG00000000419;RI:20:50945737:50945762-50945847:50945923:-     0.01234349198627487     0.007421823408974324
ENSG00000000971;RI:1:196740619:196740720-196741875:196742051:+  nan     nan
ENSG00000000971;RI:1:196740619:196740792-196741875:196742051:+  nan     nan
ENSG00000000971;RI:1:196740619:196740792-196741881:196742051:+  nan     nan
ENSG00000000971;RI:1:196740619:196741554-196741875:196742051:+  nan     nan
ENSG00000001497;RI:X:65517987:65518073-65520702:65522285:-      nan     nan
ENSG00000001497;RI:X:65517987:65518465-65520702:65522285:-      0.0     0.0
ENSG00000001497;RI:X:65520702:65520817-65521148:65522285:-      0.4059796411872284      0.2821375830450722
ENSG00000001497;RI:X:65520702:65520817-65523560:65523707:-      0.5833369574446494      0.5211951059755299
```

## 5. Differential splicing and $\Delta$PSI calculation

#### 5.1. $\Delta$PSI per isoform 

This gets done once only:

```
python ~/GitHub/SUPPA/suppa.py diffSplice -m empirical -p salmon_H9_isoform.psi salmon_K562_isoform.psi -e salmon.merged.transcript_tpm.suppa2_compatible.H9_samples.tsv salmon.merged.transcript_tpm.suppa2_compatible.K562_samples.tsv -i Homo_sapiens.GRCh38.108.ioi -gc -s --lower-bound 0.025 -o salmon.differential_splicing_isoform
```

This will save the average TPM
#### 5.2. $\Delta$PSI per local alternative splicing (AS) event

This gets done once per each type of alternative splicing (AS) event:

```
for event_type in A3 A5 AF AL MX RI SE; do
  echo "Processing $event_type events..."
  python ~/GitHub/SUPPA/suppa.py diffSplice -m empirical -p salmon_H9_${event_type}.psi salmon_K562_${event_type}.psi -e salmon.merged.transcript_tpm.suppa2_compatible.H9_samples.tsv salmon.merged.transcript_tpm.suppa2_compatible.K562_samples.tsv -i Homo_sapiens.GRCh38.108_${event_type}_strict.ioe -gc -s --lower-bound 0.025 -o salmon.differential_splicing_${event_type}
done
```
