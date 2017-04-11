# methpipeline
methpipeline based on bwameth

# steps

First step---downlad & install bwameth from [bwameth](https://github.com/brentp/bwa-meth)
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

Second step---prepare index
-----
```
bwameth.py index $REFERENCE
```

Third step---Align
-----

    bwameth.py --threads 16 \
         --reference $REFERENCE \
         $FQ1 $FQ2 > some.sam

Additionally, you can get SNP from BS-Seq by [BisSNP](https://sourceforge.net/projects/bissnp/) 

Fourth step---call methylation & SNP
-----
```
bwameth.py tabulate --map-q 5 --bissnp $BisSNP --prefix outprefix -t 12 --nome --reference $REFERENCE $PREFIX.bam
```
Then, you can get methylation infromation from the bed file and SNP information from snp.vcf files respectively.
