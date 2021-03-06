FROM guitarmind/r-base
MAINTAINER markpeng@exosite.com

RUN apt-get update && apt-get install -y unzip git htop openssh-server libcurl4-openssl-dev \
    libssl-dev libxml2-dev curl openssl gnupg2 libgit2-dev wget pigz && \
    wget http://ftp.us.debian.org/debian/pool/main/o/openssl1.0/libssl1.0.2_1.0.2u-1~deb9u1_amd64.deb && \
    dpkg -i libssl1.0.2_1.0.2u-1~deb9u1_amd64.deb && \
    rm libssl1.0.2_1.0.2u-1~deb9u1_amd64.deb

# Install required R dependencies and setup R packages
RUN echo "r <- getOption('repos'); r['CRAN'] <- 'http://cran.us.r-project.org'; options(repos = r);" > ~/.Rprofile && \
    Rscript -e "install.packages('bit64')" && \
    Rscript -e "install.packages('https://cran.r-project.org/src/contrib/Archive/Rserve/Rserve_1.7-3.tar.gz', repos = NULL)" && \
    # Rscript -e "install.packages('https://github.com/guitarmind/Rserve-1.7/releases/download/linux/Rserve_1.7-5.tar.gz', repos = NULL)" && \
    Rscript -e "install.packages('zoo')" && \
    Rscript -e "install.packages('xts')" && \
    Rscript -e "install.packages('fpp')" && \
    Rscript -e "install.packages('httr')" && \
    Rscript -e "install.packages('RCurl')" && \
    Rscript -e "install.packages('git2r')" && \
    Rscript -e "install.packages('usethis')" && \
    Rscript -e "install.packages('devtools', dependencies=TRUE)" && \
    Rscript -e "install.packages('jsonlite')" && \
    Rscript -e "devtools::install_version('rjson', version = '0.2.15')" && \
    Rscript -e "install.packages('leaps')" && \
    Rscript -e "install.packages('https://cran.r-project.org/src/contrib/Archive/locfit/locfit_1.5-9.1.tar.gz', repos = NULL)" && \
    Rscript -e "install.packages('https://cran.r-project.org/src/contrib/Archive/TSA/TSA_1.01.tar.gz', repos = NULL)" && \
    Rscript -e "devtools::install_github('twitter/AnomalyDetection')" && \
    Rscript -e "devtools::install_version('data.table', version = '1.11.8')" && \
    # Rscript -e "install.packages('data.table', type = 'source', repos = 'https://Rdatatable.gitlab.io/data.table')" && \
    Rscript -e "install.packages('tidyverse')" && \
    Rscript -e "install.packages('fasttime')"

# Install erlang/elixir dependencies
ENV OTP_VERSION="19.3.6"

RUN echo "deb https://packages.erlang-solutions.com/debian stretch contrib" >> /etc/apt/sources.list && \
    wget https://packages.erlang-solutions.com/debian/erlang_solutions.asc && apt-key add erlang_solutions.asc && \
    apt-get update && apt-get install -y esl-erlang=1:$OTP_VERSION

# Elixir expects utf8.
ENV ELIXIR_VERSION="v1.4.5" \
    LANG=C.UTF-8

RUN set -xe \
    && ELIXIR_DOWNLOAD_URL="https://github.com/elixir-lang/elixir/releases/download/${ELIXIR_VERSION}/Precompiled.zip" \
    && ELIXIR_DOWNLOAD_SHA256="a740e634e3c68b1477e16d75a0fd400237a46c62ceb5d04551dbc46093a03f98"\
    && curl -fSL -o elixir-precompiled.zip $ELIXIR_DOWNLOAD_URL \
    && echo "$ELIXIR_DOWNLOAD_SHA256 elixir-precompiled.zip" | sha256sum -c - \
    && unzip -d /usr/local elixir-precompiled.zip \
    && rm elixir-precompiled.zip \
    && rm -rf /var/lib/apt/lists/*

CMD ["iex"]
