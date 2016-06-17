# A5-miseq

[A5-miseq](https://sourceforge.net/projects/ngopt/) 是一个用 perl 开发的针对细菌基因组 de novo assembly 的 pipeline 工具。它本身不参与组装，而是通过组合一套工具来完成工作，工具集包括：

* bwa
* samtools
* SGA
* bowtie
* Trimmomatic
* IDBA-UD
* SSPACE

这些工具都以及集成在 A5-miseq 中，不需要另外安装。为了避免不同版本的 samtools，bowtie 对结果产生的差异，建议采用虚拟环境如 docker 等来隔离运行环境。

## 安装

### 1. 下载安装

```bash
~/tmp$ wget http://downloads.sourceforge.net/project/ngopt/a5_miseq_linux_20150522.tar.gz
~/tmp$ tar zxvf a5_miseq_linux_20150522.tar.gz -C ~/app
~/tmp$ sudo ln -s ~/app/a5_miseq_linux_20150522/bin/a5_pipeline.pl /usr/local/sbin
```

### 2. Docker镜像方式

```bash
~/docker$ mkdir -p a5-miseq && cd a5-miseq
~/docker/a5-miseq$ touch Dockerfile
```

```bash
FROM ubuntu:latest
MAINTAINER Mark Renton <indexofire@gmail.com>

RUN apt-get update -qy
RUN apt-get install -qy openjdk-7-jre-headless file

ADD http://downloads.sourceforge.net/project/ngopt/a5_miseq_linux_20150522.tar.gz /tmp/a5_miseq.tar.gz

RUN mkdir /tmp/a5_miseq
RUN tar xzf /tmp/a5_miseq.tar.gz --directory /tmp/a5_miseq --strip-components=1

ADD run /usr/local/bin/
ADD Procfile /
ENTRYPOINT ["/usr/local/bin/run"]
```

## 使用

```bash
$ perl a5_pipeline.pl SRR955386_1.fastq SRR955386_2.fastq ~/data/a5_output
```

## 文档

```bash
#Usage:

a5_pipeline.pl [--begin=1-5] [--end=1-5] [--preprocessed] <lib_file> <out_base>


```
