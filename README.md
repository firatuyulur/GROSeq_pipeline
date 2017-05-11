# GROSeq_pipeline

This is a GROSeq local pipeline trial. 

First, the obtained fastqs are aligned using bwa with the following commands.
-bwa needs an indexing step. You can find details of it in its user manual. hg19.fa I am using is the prefix of several hg19 files that are generated after bwa indexing.

As GROSeq readlength I used was <70, I used aln + samse in bwa rather than bwa mem.

`bwa aln hg19.fa data1.fastq.gz > data1.sai` 

``bwa samse -f data1.sam hg19.fa data1.sai data1.fastq.gz``

If your readlength is above 70 bps, bwa mem will be a better option.

`bwa mem hg19.fa ABC.fastq.gz > ABC_hg19aligned.sam`

The rest will be the same.

For sam to bam conversion, I used samtools view

`samtools view -bS data1.sam > data1.bam`

For mapping quality threshold, you can check samtools view options. Say you want your threshold to be >= 10, then you add a q parameter as 

`samtools view -bSq 10 data1.sam > data1.bam`

and sort with

`samtools sort data1.bam > sorted_data1.bam`

The next step is bam to bed conversion. This is done by bedtools;

`bedtools bamtobed -i sorted_GroSeq_rep1.bam > sorted_GroSeq_rep1.bed`

Also a bed12 format is well-used for this case;

`bedtools bamtobed -bed12 -i sorted_data1.bam > sorted_data1.bed`

Next step is generating the bedGraph files. For this purpose there are multiple options. HOMER or bedtools genomecov are good options.  will be used. Before using genomecov, it advises a sorting as in the following;

`sort -k 1,1 sorted_data1.bed > re_sorted_data1.bed`

To use genomecov, use something like 

`mysql --user=genome --host=genome-mysql.cse.ucsc.edu -A -e \
	"select chrom, size from hg19.chromInfo" > hg19.genome ` 
  
  to generate genome file. and do the following;
  
`bedtools genomecov -bg -i re_sorted_data1.bed -strand + -g hg19.genome > plusStrand_re_sorted_data1.bedGraph`
