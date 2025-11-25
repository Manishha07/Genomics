#De novo Genome Assembly of Escherichia coli (SRR35831031)

#Aim
#To perform a quick de novo genome assembly of an E. coli isolate using Illumina paired-end reads and evaluate the quality using standard bioinformatics tools.

#Tools & Commands Used
#1. SRA Toolkit — Download & Convert Data
   #Download SRA file
    > prefetch SRR35831031
   #Convert SRA → FASTQ
    > fasterq-dump --split-files SRR35831031.sra

#2. SPAdes — Genome Assembly
    > spades.py -1 SRR35831031_1.fastq -2 SRR35831031_2.fastq -o spades_output/

#3. QUAST — Assembly Quality Check
    > quast.py spades_output/contigs.fasta -o quast_results/

#4. BUSCO — Genome Completeness Analysis
    > busco -i spades_output/contigs.fasta -l bacteria_odb10 -o busco_output -m genome

#5. BWA + Samtools — Read Alignment to Assembly
    #Index assembly
      > bwa index spades_output/contigs.fasta
    #Align reads
      > bwa mem spades_output/contigs.fasta SRR35831031_1.fastq SRR35831031_2.fastq > aln.sam
    #Convert SAM → sorted BAM
      > samtools view -Sb aln.sam | samtools sort -o aln.sorted.bam
      > samtools index aln.sorted.bam


#Summary
  #1. Illumina paired-end data was downloaded and converted using SRA Toolkit
  #2. Genome assembled using SPAdes
  #3. Assembly quality evaluated using QUAST
  #4. Genome completeness assessed using BUSCO
  #5. Reads mapped back to assembly using BWA and samtools
  #6. The assembly was validated and found to be high-quality

#Key Results
  #1. Genome size: ~4.9 Mb
  #2. Total contigs: 122
  #3. N50: ~127 kb
  #4. GC content: ~50.6%
  #5. BUSCO completeness: 100% (124/124 genes)
  #6. IGV visualization: Clean alignment & uniform coverage

 
      




   
