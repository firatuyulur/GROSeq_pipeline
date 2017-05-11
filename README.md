# GROSeq_pipeline

This is a GROSeq local pipeline trial. I will be using the data on HOMER's GROSeq tutorial.

First, the obtained fastqs are aligned using bwa with the following commands.
-bwa needs an indexing step. You can find details of it in its user manual. hg19.fa I am using is the prefix of several hg19 files that are generated after bwa indexing.

As GROSeq readlength I used was <70, I used aln + samse in bwa rather than bwa mem.

`bwa aln hg19.fa GroSeq_rep1.fastq.gz > GroSeq_rep1.sai` 

``bwa samse -f GroSeq_rep1.sam hg19.fa GroSeq_rep1.sai GroSeq_rep1.fastq.gz``

If your readlength is above 70 bps, bwa mem will be a better option.

`bwa mem hg19.fa ABC.fastq.gz > ABC_hg19aligned.sam`

The rest will be the same.

For sam to bam conversion, I used samtools view

`samtools view -bS GroSeq_rep1.sam > GroSeq_rep1.bam`

and sort with

`samtools sort GroSeq_rep1.bam > sorted_GroSeq_rep1.bam`

The next step is bam to bed conversion. This is done by bedtools;

`bedtools bamtobed -i`
