# 04_TD+Pi+F

## （1）TD

```shell
for i in HY DT BT EBZ GL HN JZ LD LM MY QML Wild TX YL sichuan xizang gansu
do
vcftools --gzvcf vcf.gz --keep ${i}.txt --recode --recode-INFO-all --out ${i}.TD
vcftools --vcf ${i}.TD.recode.vcf --out ${i}_50kb.TajimaD --TajimaD 50000
done
```

## (2) Pi

```shell
for i in HY BT EBZ GL HN JZ LD LM MY QML Wild TX YL xizang sichuan gansu
do
vcftools --gzvcf vcf.gz --keep ${i}.txt --recode --recode-INFO-all --out ${i}.PI
vcftools --vcf ${i}.PI.recode.vcf --out ${i}_50kb.PI --window-pi 50000 --window-pi-step 10000
done
```

## (3) 近交系数F

```shell
vcftools --gzvcf $vcf --plink --out $OUT
plink --allow-extra-chr --chr-set 29 --file $OUT --make-bed --out $OUT
plink --allow-extra-chr --chr-set 29 --bfile $OUT --het --out $OUT
plink --allow-extra-chr --chr-set 29 --bfile $OUT --homozyg-window-snp 50 --homozyg-snp 50 --homozyg-kb 300 --homozyg-density 50 --homozyg-gap 1000 --homozyg-window-missing 5 --homozyg-window-threshold 0.05 --homozyg-window-het 03 --out $OUT
```

