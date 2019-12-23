FROM rexyai/r-minimal

RUN apk update --no-cache && \
    apk add openblas && \
    # replace reference blas by openblas
    rm /usr/local/lib/R/lib/libRblas.so && \
    ln -s /usr/lib/libopenblas.so.3 /usr/local/lib/R/lib/libRblas.so && \
    installr -d -t "zlib-dev gfortran" rsparse
