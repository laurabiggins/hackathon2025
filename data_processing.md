## ONT cDNA demux data

### Pychopper command for H9 (hES)
```
docker run --rm -it   -v $(pwd):/data   -w /data   quay.io/biocontainers/pychopper:2.7.10--pyhdfd78af_0   pychopper -t 32 -U -r sample_report.pdf -k PCB111 -u SGNex_H9_cDNA_replicate2_run4_unclassified_reads.fastq -w SGNex_H9_cDNA_replicate2_run4_rescued_reads.fastq SGNex_H9_cDNA_replicate2_run4.fastq.gz SGNex_H9_cDNA_replicate2_run4_full_length_reads.fastq
```
### Alignment using minimap2

```
minimap2 -y -t 32 -ax splice -uf -k 14 ${reference} ${input} > ${id}.sam
```

## ONT direct RNA sequencoing data

## Illumina short read data
