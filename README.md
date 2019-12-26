# MCscan-Analysis-process
使用MCscan的python版本绘制共线性分析图（python version）
<p align="center">
    <img src="https://github.com/hexin010101/MCscan-Analysis-process/blob/master/K5U6%25N5VI69SKPS%5D%7BG%60%25SFD.png"  width="85%" />
</p>
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
python -m jcvi.compara.catalog ortholog --dbtype prot B73 Y82 --no_strip_names
```
这里会生成 **B73.Y82.anchors**这个比对文件

***
5. 配置文件
 > 创建seqids文件
 


```
1,2,3,4,5,6,7,8,9,10
Y82_chr1,Y82_chr2,Y82_chr3,Y82_chr4,Y82_chr5,Y82_chr6,Y82_chr7,Y82_chr8,Y82_chr9,Y82_chr10
```
> 创建layout文件
```
# y, xstart, xend, rotation, color, label, va,  bed
 .6,     .2,    .9,       0,      , B73,  top, B73.bed
 .4,     .2,    .9,       0,      , Y82,  top, Y82.bed
# edges
e, 0, 1, B73.Y82.anchors.simple
```
6. 从anchors文件生成simple文件
```
python -m jcvi.compara.synteny screen --minspan=30 --simple B73.Y82.anchors B73.Y82.anchors.new
```
7. 画图
```
python -m jcvi.graphics.karyotype seqids layout
```
<center><embed src="https://github.com/hexin010101/MCscan-Analysis-process/blob/master/karyotype(1).pdf" width="850" height="600"></center>
