#------------------------------------------------------------------------------------------------------
# BioRSTUDIO DOCKER FILE v1.1
# [bulid code] docker build -t koheilk/biorstudio:1.1 /Users/nextganken/Documents/bioinfo/docker/BioRSTUDIO/v1.1
#------------------------------------------------------------------------------------------------------
FROM rocker/rstudio:4.0.5
MAINTAINER koheilk

#---------- Dependencies ----------#
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
build-essential \
libxml2 \
libxml2-dev \
libssl-dev \
libbz2-dev \
libffi-dev \
liblzma-dev \
libncurses5-dev \
libncursesw5-dev \
libpq-dev \
libreadline-dev \
libsqlite3-dev \
llvm \
tk-dev \
xz-utils \
zlib1g-dev \
libcurl4-openssl-dev \
libgsl-dev \
libxml2 \
libxml2-dev \
language-pack-ja-base \
language-pack-ja \
libhdf5-dev \
libxt-dev \
libcairo2-dev \
samtools \
glpk-doc \
glpk-utils \
libglpk-dev \
wget \
npm \
libnode-dev \
librsvg2-bin \
libavfilter-dev \
libpoppler-cpp-dev \
libtesseract-dev \
libleptonica-dev \
tesseract-ocr-eng \
libmagick++-dev

#---------- Japanese ----------#
RUN echo "ja_JP.UTF-8 UTF-8" >> /etc/locale.gen
RUN locale-gen ja_JP.UTF-8
RUN /usr/sbin/update-locale LANG=ja_JP.UTF-8

ENV LC_ALL ja_JP.UTF-8
ENV LANG ja_JP.UTF-8

#---------- Make home ----------#
ENV HOME=/home
RUN mkdir /home/bin
#ENV TMPDIR=/tmp

#---------- Python ----------#
RUN apt-get update && apt-get install -y --no-install-recommends \
  python3.9 \
  python3-pip \
  python3-setuptools \
  python3-dev
RUN pip3 install MACS2

#---------- R packages ----------#
RUN install2.r --deps TRUE --ncpus -1 \
devtools \
BiocManager \
remotes \
hexbin \
gam \
survminer \
egg \
data.table \
ggplot2 \
dplyr \
ggsignif \
viridis \
stringr \
uwot \
readr \
statmod \
DT \
mclust

RUN R -e "BiocManager::install(c('BSgenome.Hsapiens.UCSC.hg19','BSgenome.Hsapiens.UCSC.hg38','BSgenome.Mmusculus.UCSC.mm10','TxDb.Hsapiens.UCSC.hg38.knownGene','TxDb.Hsapiens.UCSC.hg19.knownGene','TxDb.Mmusculus.UCSC.mm10.knownGene','org.Hs.eg.db','org.Mm.eg.db'))"
RUN R -e "BiocManager::install(c('Biobase','BiocVersion','SummarizedExperiment','Rsamtools','edgeR','rtracklayer','ComplexHeatmap','ChIPpeakAnno','ChIPseeker','clusterProfiler','limma','slingshot','destiny', 'DiffBind'))"
RUN R -e "BiocManager::install('preprocessCore', configure.args='--disable-threading', force = TRUE)"
RUN R -e "BiocManager::install(c('BioinformaticsFMRP/TCGAbiolinksGUI.data', 'BioinformaticsFMRP/TCGAbiolinks'))"

ENV URL=https://cran.r-project.org/src/contrib/Archive/Cairo/Cairo_1.5-14.tar.gz
RUN cd $HOME/bin && wget $URL && R CMD INSTALL Cairo_1.5-14.tar.gz
ENV URL=https://cran.r-project.org/src/contrib/Archive/rsvd/rsvd_1.0.3.tar.gz
RUN cd $HOME/bin && wget $URL && R CMD INSTALL rsvd_1.0.3.tar.gz

RUN installGithub.r --deps TRUE \
immunogenomics/presto \
GreenleafLab/chromVARmotifs

RUN R -e "devtools::install_github('GreenleafLab/ArchR', ref='release_1.0.2', repos = BiocManager::repositories())"
RUN R -e "ArchR::installExtraPackages()"
RUN R -e "remotes::install_version('spatstat', version = '1.64-1', upgrade = 'never')"

RUN install2.r --deps TRUE --ncpus -1 NMF

#---------- User docker ----------#
WORKDIR $HOME
