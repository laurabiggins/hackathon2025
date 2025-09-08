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

## Illumina short read data
