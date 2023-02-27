# 01-SNP过滤

##备注：公司提供的vcf文件为每个染色体分别为一个文件

1. 按染色体合并vcf

   ```shell
   #软件版本 gatk4-4.1.1.0   bcftools-1.10.2
   cd /GS01/project/wudd_group/wudd20t2/wangsheng/Gao/data/yak_jianhua2/vcf
   
   source activate gatk4
   gatk GatherVcfs \
   -I HIC_ASM_1.vcf.gz -I HIC_ASM_2.vcf.gz -I HIC_ASM_3.vcf.gz -I HIC_ASM_4.vcf.gz -I HIC_ASM_5.vcf.gz \
   -I HIC_ASM_6.vcf.gz -I HIC_ASM_7.vcf.gz -I HIC_ASM_8.vcf.gz -I HIC_ASM_9.vcf.gz -I HIC_ASM_10.vcf.gz \
   -I HIC_ASM_11.vcf.gz -I HIC_ASM_12.vcf.gz -I HIC_ASM_13.vcf.gz -I HIC_ASM_14.vcf.gz -I HIC_ASM_15.vcf.gz -I HIC_ASM_16.vcf.gz -I HIC_ASM_17.vcf.gz -I HIC_ASM_18.vcf.gz -I HIC_ASM_19.vcf.gz -I HIC_ASM_20.vcf.gz -I HIC_ASM_21.vcf.gz -I HICruanjian_ASM_22.vcf.gz -I HIC_ASM_23.vcf.gz -I HIC_ASM_24.vcf.gz -I HIC_ASM_25.vcf.gz -I HIC_ASM_26.vcf.gz -I HIC_ASM_27.vcf.gz -I HIC_ASM_28.vcf.gz -I HIC_ASM_29.vcf.gz \
   -O ../yak.vcf.gz
   
   bcftools index --threads 6 --tbi yak.1.vcf.gz
   
   conda deactivate
   ```

2. SNP过滤

```shell
cd /GS01/project/wudd_group/wudd20t2/wangsheng/Gao/data/yak_jianhua2/snp
#####step1:提取SNP和INDEL
source activate gatk4
REF_FASTA=/GS01/project/wudd_group/wudd20t2/wangsheng/Gao/data/nohair_yak/reference/WLYAK.fa
gatk SelectVariants -R $REF_FASTA -V yak.vcf.gz -O yak.SNP.vcf.gz -select-type SNP
gatk SelectVariants -R $REF_FASTA -V yak.vcf.gz -O yak.INDEL.vcf.gz -select-type INDEL
conda deactivate
######step2:使用GATK和vcftools过滤SNP
source activate gatk4
gatk VariantFiltration --variant yak.SNP.vcf.gz -R $REF_FASTA \
--filter-expression "QD < 2.0 || MQ < 40.0 || FS > 60.0 || SOR > 3.0 || MQRankSum < -12.5 || ReadPosRankSum < -8.0" --filter-name "Filter" -O yak.SNP.filter_gatk.vcf.gz
conda deactivate
#####软件版本    vcftools-v0.1.16
vcftools --gzvcf yak.SNP.filter_gatk.vcf.gz --recode --recode-INFO-all --max-missing 0.8 --mac 3 --minQ 30 --out yak.SNP.filter_gatk.g8mac3.recode.vcf.gz
######step3:使用GATK和vcftools过滤INDEL

```