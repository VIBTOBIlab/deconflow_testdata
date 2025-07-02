# Toy data to test DecoNFlow pipeline

In this repository you can find some toy data that can be used to test the pipeline once it is cloned. The data was downloaded from SRA using the [nf-core/fetchngs](https://nf-co.re/fetchngs/1.12.0/) pipeline v1.12.0 and the following SRR ids:

```
SRR1045638
SRR536237
SRR1045706
SRR620007
SRR536234
SRR641696
SRR1045641
SRR536238
SRR1045707
SRR620008
SRR536235
SRR641695
```

Then, the raw fastq files were preprocessed using the [nf-core/methylseq](https://nf-co.re/methylseq/2.6.0/) pipeline v2.6.0. Finally, the bam and cov files produced by the pipeline were subsampled to retain only the first 100K bp of chr1 using respectively [SAMtools](https://www.htslib.org/) v1.18 and awk:

```
# bam
samtools view -b <coordinated_sorted_bam> "chr1:1-100000" > <output_bam>

#cov
zcat <cov_file> | awk '($1 == "chr1" || $1 == "1") && $2 <= 100000' | gzip > <output_cov>
```

Finally, a part of these samples are used to build the reference atlas and the remaining samples are used as bulk samples to be deconvolved:

```
experiment,tissue,donor,sample,usage
SRX388734,Lung cells,STL001,SRR1045638,atlas
SRX388734,Lung cells,STL001,SRR1045641,atlas
SRX175350,Lung cells,STL002,SRR536237,atlas
SRX175350,Lung cells,STL002,SRR536238,bulk
SRX388743,Pancreas cells,STL002,SRR1045708,atlas
SRX388743,Pancreas cells,STL002,SRR1045707,atlas
SRX175354,Pancreas cells,STL003,SRR620007,atlas
SRX175354,Pancreas cells,STL003,SRR620008,bulk
SRX175348,Sigmoid colon cells,STL001,SRR536234,atlas
SRX175348,Sigmoid colon cells,STL001,SRR536235,atlas
SRX190161,Sigmoid colon cells,STL003,SRR641696,atlas
SRX190161,Sigmoid colon cells,STL003,SRR641695,bulk
```