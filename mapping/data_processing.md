The following data was picked for our evaluation:

```
Sample	Sequencing platform	Sequencing flowcell	Barcoding kit	Sequencing kit	
SGNex_H9_cDNA_replicate2_run4	promethion	FLO-PRO002	SQK-PCB109	SQK-PCB109	cDNA
SGNex_H9_cDNA_replicate3_run4	promethion	FLO-PRO002	SQK-PCB109	SQK-PCB109	cDNA
SGNex_K562_cDNA_replicate1_run3	minion	FLO-MIN106	NA	SQK-PCS108	cDNA
SGNex_K562_cDNA_replicate3_run4	gridion	FLO-MIN106D	NA	SQK-PCS109	cDNA
					
SGNex_H9_Illumina_replicate2_run1					short
SGNex_H9_Illumina_replicate3_run1					short
SGNex_K562_Illumina_replicate3_run1					short
SGNex_K562_Illumina_replicate4_run1					short
					
SGNex_H9_directRNA_replicate2_run1					
GNex_H9_directRNA_replicate3_run1					
SGNex_K562_directRNA_replicate1_run1					
SGNex_K562_directRNA_replicate4_run1					
```
**Note below:** PCB109 and PCB111 share same adapter sequences.

## ONT cDNA demux data

### Pychopper command for H9 (hES)
```
docker run --rm -it   -v $(pwd):/data   -w /data   quay.io/biocontainers/pychopper:2.7.10--pyhdfd78af_0
pychopper -t 32 -U -r sample_report.pdf -k PCB111 -u SGNex_H9_cDNA_replicate2_run4_unclassified_reads.fastq
-w SGNex_H9_cDNA_replicate2_run4_rescued_reads.fastq SGNex_H9_cDNA_replicate2_run4.fastq.gz SGNex_H9_cDNA_replicate2_run4_full_length_reads.fastq
```

```
docker run --rm -it   -v $(pwd):/data   -w /data   quay.io/biocontainers/pychopper:2.7.10--pyhdfd78af_0
pychopper -t 32 -U -r sample_report.pdf -k PCS109 -u SGNex_K562_cDNA_replicate3_run4_unclassified_reads.fastq
-w SGNex_K562_cDNA_replicate3_run4_rescued_reads.fastq SGNex_K562_cDNA_replicate3_run4.fastq.gz SGNex_K562_cDNA_replicate3_run4_full_length_reads.fastq 
```

**Output:**
```
-----------------------------------
Reads with two primers: 86.54% (with UMI 0.09%)
Rescued reads:          1.24%
Unusable reads:         12.21%
-----------------------------------
```

### Alignment using minimap2

For the time being I am using the same mapping command for both direct RNA and cDNA:

```
for i in *fastq.gz; do docker run --rm -it   -v $(pwd):/data   -w /data   quay.io/biocontainers/minimap2:2.30--h577a1d6_0
minimap2 -y -t 32 -ax splice -uf -k 14 Homo_sapiens.GRCh38.dna.primary_assembly.fa  $i > $i.sam ; done
```

## ONT direct RNA sequencing data

For the time being I am using the same mapping command for both direct RNA and cDNA (no chopping at this point)

```
for i in *fastq.gz; do docker run --rm -it   -v $(pwd):/data   -w /data   quay.io/biocontainers/minimap2:2.30--h577a1d6_0
minimap2 -y -t 32 -ax splice -uf -k 14 Homo_sapiens.GRCh38.dna.primary_assembly.fa  $i > $i.sam ; done
```

## Running IsoQuant

Isoquant can be installed via `mamba`. This is the command I am runnning as a first test: 

 ```
isoquant.py --threads 16 -d nanopore --bam SGNex_K562_directRNA_replicate1_run1.sorted.bam  --reference /weka/projects/references/sync_structure/genomes/Homo_sapiens/Ensembl/GRCh38/Sequence/WholeGenomeFasta/Homo_sapiens.GRCh38.dna.primary_assembly.fa --genedb /weka/projects/references/sync_structure/genomes/Homo_sapiens/Ensembl/GRCh38/Annotation/Genes/Homo_sapiens.GRCh38.108.gtf --complete_genedb  --output K562_directRNA_replicate1_run1
```

## Illumina short read data
