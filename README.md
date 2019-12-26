# MCscan-Analysis-process
使用MCscan的python版本绘制共线性分析图（python version）
1. 下载安装
`pip install jcvi`

2. 数据准备
 需要比对的两个物种的gff文件和蛋白质fasta文件（cds文件也可以）
 以B73和Y82为例
  3. 数据前处理
>  处理gff文件为bed格式
```

python -m jcvi.formats.gff bed --type=mRNA --key=Name B73.gff3.gz -o B73.bed
python -m jcvi.formats.gff bed --type=mRNA --key=Name Y82.gff3.gz -o Y82bed
```
> 处理fasta文件
```

python -m jcvi.formats.fasta format B73.pep.fa.gz B73.pep
python -m jcvi.formats.fasta format Y82.pep.fa.gz Y82.pep

```
4. 比对
```
python -m jcvi.compara.catalog ortholog B73 Y82 --no_strip_names
```
这里会生成 **B73.Y82.anchors**这个比对文件

***
5. 配置文件
 > 创建seqids文件
 $ vim seqids 


```
1,2,3,4,5,6,7,8,9,10
Y82_chr1,Y82_chr2,Y82_chr3,Y82_chr4,Y82_chr5,Y82_chr6,Y82_chr7,Y82_chr8,Y82_chr9,Y82_chr10
```
