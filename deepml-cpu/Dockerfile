# Based on Miniconda and optmized for CPU
# https://github.com/ContinuumIO/docker-images/blob/master/miniconda3/debian/Dockerfile
# https://conda.io/projects/conda/en/latest/commands.html#conda-vs-pip-vs-virtualenv-commands
# 
# docker build --rm=true --no-cache=true -t markpeng/deepml-cpu -f ./Dockerfile .
# docker run --rm -p 8888:8888 -p 6006:6006 --name deepml-cpu -it markpeng/deepml-cpu bash
# 

FROM guitarmind/miniconda:py-3.8.5

LABEL MAINTAINER="Mark Peng"

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
# Disabling SSL verification
ENV SSL_NO_VERIFY=1
ENV PATH=/opt/conda/bin:$PATH

RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    PIP_INSTALL="pip --no-cache-dir install --upgrade --default-timeout=100" && \
    GIT_CLONE="git clone --depth 10" && \
    PYTHONDONTWRITEBYTECODE=true && \

# ==================================================================
# System Packages & Tools
# ------------------------------------------------------------------

    apt-get update --fix-missing && \
    apt-get clean && \

# ==================================================================
# Essesntial Packages
# ------------------------------------------------------------------

    conda install -y --freeze-installed \
        binutils \
        psutil \
        Cython \
        numpy \
        scipy \
        pandas \
        scikit-learn \
        matplotlib \
        seaborn \
        && \

    # Jupyter Notebook
    conda install -c conda-forge -y --freeze-installed \
        jupyter \
        jupyter_contrib_nbextensions \
        jupyter_nbextensions_configurator \
        parso==0.8.1 \
        && \
    jupyter contrib nbextension install --user && \
    jupyter nbextension enable code_prettify/code_prettify && \
    jupyter nbextension enable collapsible_headings/main && \
    jupyter nbextension enable toggle_all_line_numbers/main && \

# ==================================================================
# Tensorflow
# ------------------------------------------------------------------

    # conda install -y --freeze-installed \
    #     tensorflow==2.3.0 \
    #     && \

# ==================================================================
# PyTorch
# ------------------------------------------------------------------

    # conda install -y --freeze-installed \
    #     pytorch==1.7.1 \
    #     torchvision torchaudio cpuonly -c pytorch && \
    # conda install -c conda-forge -y --freeze-installed \
    #     pytorch-lightning \
    #     && \

    # $PIP_INSTALL \
    #     torch_optimizer \
    #     && \

# ==================================================================
# OpenCV
# ------------------------------------------------------------------

    # apt-get install -y build-essential && \
    # conda install -y --freeze-installed cmake==3.14.0 && \
    # $GIT_CLONE --branch 4.3.0 https://github.com/opencv/opencv ~/opencv && \
    # mkdir -p ~/opencv/build && cd ~/opencv/build && \
    # cmake -D CMAKE_BUILD_TYPE=RELEASE \
    #       -D CMAKE_INSTALL_PREFIX=/usr/local \
    #       -D WITH_IPP=OFF \
    #       -D WITH_CUDA=OFF \
    #       -D WITH_OPENCL=OFF \
    #       -D BUILD_TESTS=OFF \
    #       -D BUILD_PERF_TESTS=OFF \
    #       -D PYTHON3_LIBRARY=/opt/conda/lib/libpython3.8.so \
    #       -D PYTHON3_INCLUDE_DIR=/opt/conda/include/python3.8 \
    #       -D PYTHON3_EXECUTABLE=/opt/conda/bin/python \
    #       -D PYTHON3_PACKAGES_PATH=/opt/conda/lib/python3.8/site-packages \
    #       .. && \
    # make -j"$(nproc)" install && \
    # ln -s /usr/local/include/opencv4/opencv2 /usr/local/include/opencv2 && \
    # conda remove -y cmake && \
    # DEBIAN_FRONTEND=noninteractive apt-get remove -y build-essential && \

# ==================================================================
# Additional Packages
# ------------------------------------------------------------------

    # conda install -c conda-forge -y --freeze-installed \
    #     gensim \
    #     transformers \
    #     optuna \
    #     yellowbrick \
    #     plotly \
    #     shap \
    #     gym \
    #     && \

    # $PIP_INSTALL \
    #     zhconv \
    #     iterative-stratification \
    #     && \

    # # LightGBM, Xgboost
    # conda install -c conda-forge -y  --freeze-installed \
    #     lightgbm xgboost \
    #     && \

# ==================================================================
# Config & Cleanup
# https://jcristharif.com/conda-docker-tips.html
# ------------------------------------------------------------------
    ldconfig && \
    apt-get clean -y && \
    apt-get autoremove -y && \
    conda clean --all --force-pkgs-dirs -y && \
    find /opt/conda/ -follow -type f -name '*.a' -delete && \
    find /opt/conda/ -follow -type f -name '*.pyc' -delete && \
    find /opt/conda/ -follow -type f -name '*.js.map' -delete && \
    rm -rf /tmp/* && \
    rm -rf /var/cache/apk/* && \
    find /opt/conda/lib/python3.*/ -name 'tests' -exec rm -r '{}' + && \
    find /opt/conda/lib/python3.*/site-packages/ -name '*.so' -print -exec sh -c 'file "{}" | grep -q "not stripped" && strip -s "{}"' \;

ENV TZ=Asia/Taipei
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

EXPOSE 8888 6006 8090
WORKDIR /workspace