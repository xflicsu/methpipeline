# methpipeline
methpipeline based on bwameth & swDMR
Call methylation & Identify DMR from multiple samples

# Steps--Call DNA methylation level
First step---Downlad & install [bwameth](https://github.com/brentp/bwa-meth)
-----
```Shell
    # these 4 lines are only needed if you don't have toolshed installed
    wget https://pypi.python.org/packages/source/t/toolshed/toolshed-0.4.0.tar.gz
    tar xzvf toolshed-0.4.0.tar.gz
    cd toolshed-0.4.0
    sudo python setup.py install

    wget https://github.com/brentp/bwa-meth/archive/v0.10.tar.gz
    tar xzvf v0.10.tar.gz
    cd bwa-meth-0.10/
    sudo python setup.py install
```

Second step---Prepare index
-----
```
bwameth.py index $ref (genome fasta file)
```

Third step---Align
-----
```
bwameth.py --threads 16 --prefix $PREFIX --reference $ref $fq1 $fq2
```
Additionally, you can get SNP from BS-Seq by [BisSNP](https://sourceforge.net/projects/bissnp/) 

Fourth step---call methylation & SNP
-----
```
bwameth.py tabulate --map-q 5 --bissnp $BisSNP --prefix outprefix -t 12 --nome --reference $ref $PREFIX.bam
```
Then, you can get methylation infromation from the outprefix.meth.bed file and SNP information from outprefix.snp.vcf files respectively.


An example: 
-----
list.bak
```
16N
16T
17N
17T
```
shell command of PBS (qsub)
```
indir=/panfs/home/VIP/maofb/lxf/data/Oesophagus/
ref=/panfs/home/VIP/maofb/lxf/DB/genome/hg19/bwameth/hg19.fa
outdir=/panfs/home/VIP/maofb/lxf/Project/Oesophagus/bwa-meth
BisSNP=/panfs/home/VIP/maofb/lxf/soft/BisSNP-0.82.2.jar

for i in `cat list.bak`
do
        echo "#!/bin/bash
#PBS -N bwa-meth
#PBS -l nodes=1:ppn=2
#PBS -j oe
#PBS -q dawningCB60

fq1=$indir/$i.1.fq.gz
fq2=$indir/$i.2.fq.gz
PREFIX=$i

cd $outdir
bwameth.py --threads 16 --prefix \$PREFIX --reference $ref  \$fq1 \$fq2
bwameth.py tabulate --map-q 5 --bissnp $BisSNP --prefix $i -t 12 --nome --reference $ref \$PREFIX.bam
" >$i.sh
        qsub $i.sh
done
```
# Steps--Identify DMR from multiple samples
First---Download & install [swDMR](https://github.com/xflicsu/swdmr) 
-----
```
>cd /path/of/swDMR----- Go to path of swDMR software installed
>tar xzvf swDMR-xxx.tar.gz
>cd swDMR-xxx
>sh install
```
Second---Prepare files for [swDMR](https://github.com/xflicsu/swdmr)
-----
Third----DMR detection with [swDMR](https://github.com/xflicsu/swdmr)
-----
