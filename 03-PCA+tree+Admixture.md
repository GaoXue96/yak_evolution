# 03-PCA+tree+Admixture

## (1)PCA

```shell
source activate jinhua

vcftools --gzvcf vcf.gz --plink --out out
plink --noweb --threads 6 --file out --make-bed --out out

gcta64 --bfile out --make-grm --autosome --out out
gcta64 --grm out --pca 10 --out out.pca

conda deactivate

twstats=/home/gaoxue/software/EIG-6.1.4/bin/twstats
$twstats -t twtable -i out.pca.eigenval -o out.pca.twstats
```

## (2)tree

```shell
source activate jinhua

vcftools --gzvcf vcf.gz --plink --out out
plink --threads 6 --memory 30000 --file out --distance square 1-ibs --out out.plink.ibs

conda deactivate
```



## (3) Admixture

```shell
source activate jinhua

vcftools --gzvcf vcf.gz --plink --out out
plink --noweb --file out --maf 0.05 --hwe 0.0001 --make-bed --out out.filter

for i in {2..10};do admixture --cv out.filter.bed $i|tee out${i};done

conda deactivate
```

