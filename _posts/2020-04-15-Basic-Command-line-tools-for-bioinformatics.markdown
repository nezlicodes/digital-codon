---
layout: post
title:  "Basic Command line tools for bioinformatics"
date:   2020-04-15 16:58:59 +0200
categories: bioinfomatics genomic data science
---
So I'm in week 02 of the Coursera course <a href="https://www.coursera.org/learn/genomic-tools" target="_about"> Command Line Tools for Genomic Data Science </a> and I am really having fun.

I feel like I learned a lot already. It's been a dense week that I would like to sum up.

# Sequences representation

Learning bioinformatics implies learning first about a number of file formats. Each file type differ from the other in the way data is represented and it's primordial to know the general structure of the file in order to manipulate it properly.
Here, we will cover:
<ul>
<li> fasta </li>
<li> fastq </li>
<li> BED </li>
<li> gtf/gff </li>
<li> sam/bam </li>
</ul>

#### fasta
A fasta file is a simple way of representing nucleotide and amino-acid sequences. It can store one or many records. Each record contains a one-line header that starts with a greater than sign '>'and contains basic information about the sequence (accession number, ID, name, organism, you name it..) followed by the sequence that can contain one line or more.
It is important to note that any tabulators, spaces, asterisks etc in sequence will be ignored.
You can comment lines i.e ignore them using the ';'
{% highlight ruby %}
>gi|9626243|ref|NC_001416.1| Enterobacteria phage lambda, complete genome
GGGCGGCGACCTCGCGGGTTTTCGCTATTTATGAAAATTTTCCGGTTTAAGGCGTTTCCGTTCTTCTTCG
TCATAACTTAATGTTTTTATTTAAAATACCCTCTGAAAAGAAAGGAAACGACAGGTGCTGAAAGCGAGGC
TTTTTGGCCTCTGTCGTTTCCTTTCTCTGTTTTTGTCCGTGGAATGAACAATGGAAGTCAACAAAAAGCA
##;Ignore me
{% endhighlight %}


#### fastq
With the advent of next generation sequencing technologies (NGS), we started adding more information to the sequences, which becomes important in bioinformatics applications that need to take into account the quality. For this purpose emerged the Fastq Format.
Typically, each record in this file type contains 4 lines:
  - A header that starts with the sign '@'
  - The raw sequence.
  - A line that start with '+' and can contain additional information about the sequence.
  - A line that  encodes the quality value for the sequence in line 2 using ASCII characters.

{% highlight ruby %}
@SRR835775.1 1/1
TAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTCACCCTAACCCTAACCCTAACCGTATCCGTCACCCTAACCCTAAC
+
???B1ADDD8??BB+C?B+:AA883CEE8?C3@DDD3)?D2;DC?8?=BAD=@C@(.6.6=A?=?@##################################
{% endhighlight %}

#### Browser Extensible Data (BED)
BED (Browser Extensible Data) format provides a flexible way to define the data lines that are displayed in an annotation track. BED lines have three required fields (Scaffold: chromose, transcript..., the 0-based start, and the end coordinates of the sequences considered) and nine additional optional fields(name, score, strand...). The columns are separated by tabs or spaces and the number of fields per line must be consistent throughout any single set of data in an annotation track.
Because of their simplicity BED files are widely use, and we will see in another post how to retreive bedfiles and analyze them using bedtools.
{% highlight ruby %}
chr22   10510227        10510528        AluSx1  2021    +
chr22   10514778        10515050        AluSx1  1933    +
chr22   10519673        10519746        AluJo   474     -
chr22   10520950        10521193        AluJo   1258    -
chr22   10522328        10522608        AluSg   2274    +
chr22   10528506        10528771        AluJr   1344    +
chr22   10529446        10529561        AluJo   771     +
{% endhighlight %}

#### Sequence Alignment Map (SAM)
It is a TAB-delimited text format with a:
 - Multi-line header: every line starting with ‘@’. It contains informations about the number of segments, the number of sequences, the program by which it has been generation and if the sequences are sorted or not. Note that the header may be absent.

{% highlight ruby %}
@HD     VN:1.0  GO:none SO: Sorted by
@SQ     SN:Chr1 LN:2992   
@SQ     SN:Chr2 LN:19381 
@RG     ID:H100223_GAII05_0002  PL:SLX  LB:wu_PII       PI:400  DS:wu_0_Genome  SM:wu_0
@RG     ID:Wii_PER01    PL:SLX  LB:wu_phaseI    PI:400  DS:wu_0_Genome  SM:wu_0
@PG     ID:Program name       VN: program version CL: Command line
@CO     TM:Fri, 17 Sep 2010 12:20:13 BST        WD:/lustre/scratch103/sanger/xcg/wu_0/self      HN:bc-16-1-07.internal.sanger.ac.uk     UN:xcg
{% endhighlight %}


 - body: constituted  by the alignment records. Each alignment line has 11 **mandatory** that always come in the same order and contain the information that follows:
 <ol>
   <li>QNAME : Query template NAME. </li>
    <li>FLAG: combination of multiple series of information all of which are represented in binary (bits). </li>
    <li>RNAME:  Reference sequence NAME of the alignment it matches a @SQ field in the header section. </li>
     <li>POS : 1-based leftmost mapping position. </li>
    <li>MAPQ : Mapping quality. </li>
    <li>CIGAR : stands for Concise Idiosyncratic Gapped Alignment Report it describes the alignment (insertions, deletions, matches...). </li>
    <li>RNEXT : Reference sequence name of the primary alignment of the NEXT read in the template</li>
    <li>PNEXT: Position of the next read in the template. (1-based)</li>
    <li>TLEN:	Template length. </li>
    <li>SEQ:	Read Sequence. </li>
    <li>QUAL: Read Quality. </li>
  </ol>

{% highlight ruby %}
@SRR835775.1 1/1
readID43GYAX15:7:1:1202:19894/1    256    contig87    540849    1    65M    *    0    0    CCTGCACGAACGAAATCCGCATGCGTCTGGTCGTTGTACGGAACGGCGGTTGTGTGACGAACGGC    EDDEEDEE=EE?DE??DDDBADEBEFFFDBEFFEBCBC=?BEEEE@=:?::?7?:8-6?7?@??# AS:i:0  XS:i:0 XN:i:0  XM:i:0 XO:i:0  XG:i:0  NM:i:0    MD:Z:65    YT:Z:UU
{% endhighlight %}

You can read more about the SAM format <a href="http://samtools.github.io/hts-specs/SAMv1.pdf" target="_about"> here </a>.

#### Bam (Binary Alignment Map)
BAM files are compressed (binary) SAM files. They contain the same information.Their small size make them ideal to store alignment files. And that's when the command line comes in handy. We will use samtools in order to view the file in the upcoming post.