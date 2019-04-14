FROM eri_python:latest

FROM rocker/rstudio:latest

# update the cran deb repo source
RUN apt-get update && apt-get install -y gnupg2
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9
RUN echo deb http://cran.revolutionanalytics.com/bin/linux/ubuntu xenial/ > /etc/apt/sources.list.d/cran.list

# java stuff
RUN apt-get install -y --no-install-recommends \
	libicu-dev \
	libbz2-dev \
	liblzma-dev \
	libpcre3-dev \
	liblzma-dev \
	zlib1g-dev \
	default-jre \
	default-jdk

# other base linux stuff
RUN apt-get install -y --no-install-recommends \
    r-cran-boot \
    r-cran-class \
    r-cran-cluster \
    r-cran-codetools \
    r-cran-foreign \
    r-cran-kernsmooth \
    r-cran-lattice \
    r-cran-mass \
    r-cran-matrix \
    r-cran-mgcv \
    r-cran-nlme \
    r-cran-nnet \
    r-cran-rjava \
    r-cran-rodbc \
    r-cran-rpart \
    r-cran-spatial \
    r-cran-survival

# stuff needed for the install of the more niche packages in install.R
RUN apt-get install -y --no-install-recommends \
    libcurl4-openssl-dev \
    libssl-dev \
    libxml2-dev

# now manually install a ton of stuff
COPY install.r /
# the make variable before inspired by this:
#  http://www.rexamine.com/2015/07/speeding-up-r-package-installation-process/
# could be doing something terrible.
#  y
#  o
#  l
#  o
RUN MAKE="make -j 8" Rscript /install.r

# install of r-cran-foreign up above installs python2.7 as a dependency, so it
# busts our python3 symlink. some bullshit
RUN rm /usr/bin/python \
   && ln -s /usr/bin/python3 /usr/bin/python

# don't keep sessions between runs of docker image
RUN echo "session-timeout-minutes=0" >> /etc/rstudio/rsession.conf

EXPOSE 8787

