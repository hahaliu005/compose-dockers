FROM centos:centos7.2.1511

# add yum repository
RUN yum -y install epel-release
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
RUN rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

# update tools
RUN yum -y update

# install development tools
RUN yum -y groupinstall "Development Tools"

# install base tools
RUN yum -y install tar vim wget telnet

# 安装依赖
RUN yum install -y autoconf automake gcc gcc-c++ git libtool make nasm zlib-devel openssl-devel tar cmake perl which bzip2 \
    freetype-devel libass libass-devel

ENV BUILD_DIR /workspace/ffmpeg_build
ENV LD_LIBRARY_PATH ${BUILD_DIR}/lib
ENV PKG_CONFIG_PATH ${BUILD_DIR}/lib/pkgconfig
ENV BIN_DIR /usr/local/bin

WORKDIR /workspace/ffmpeg_sources

# install yasm
RUN git clone --depth 1 git://github.com/yasm/yasm.git && \
    cd yasm && \
    autoreconf -fiv && \
    ./configure --prefix="${BUILD_DIR}" --bindir="${BIN_DIR}" && \
    make -j $(nproc) && \
    make install && \
    make distclean

# install x264
RUN git clone --depth 1 git://git.videolan.org/x264 && \
    cd x264 && \
    PKG_CONFIG_PATH="${BUILD_DIR}/lib/pkgconfig" ./configure --prefix="${BUILD_DIR}" --bindir="${BIN_DIR}" --enable-static && \
    make -j $(nproc) && \
    make install && \
    make distclean

# install libfdk_acc
RUN git clone --depth 1 git://git.code.sf.net/p/opencore-amr/fdk-aac && \
    cd fdk-aac && \
    autoreconf -fiv && \
    ./configure --prefix="${BUILD_DIR}" --bindir="${BIN_DIR}" --disable-shared && \
    make -j $(nproc) && \
    make install && \
    make distclean

# install libmp3lame
RUN curl -L -O http://downloads.sourceforge.net/project/lame/lame/3.99/lame-3.99.5.tar.gz && \
    tar xzvf lame-3.99.5.tar.gz && \
    cd lame-3.99.5 && \
    ./configure --prefix="${BUILD_DIR}" --bindir="${BIN_DIR}" --disable-shared --enable-nasm && \
    make -j $(nproc) && \
    make install && \
    make distclean

# install libopus
RUN git clone http://git.opus-codec.org/opus.git && \
    cd opus && \
    autoreconf -fiv && \
    ./configure --prefix="${BUILD_DIR}" --disable-shared && \
    make -j $(nproc) && \
    make install && \
    make distclean

# install libogg
RUN curl -O http://downloads.xiph.org/releases/ogg/libogg-1.3.2.tar.gz && \
    tar xzvf libogg-1.3.2.tar.gz && \
    cd libogg-1.3.2 && \
    ./configure --prefix="${BUILD_DIR}" --disable-shared && \
    make -j $(nproc) && \
    make install && \
    make distclean

# install libvorbis
RUN curl -O http://downloads.xiph.org/releases/vorbis/libvorbis-1.3.4.tar.gz && \
    tar xzvf libvorbis-1.3.4.tar.gz && \
    cd libvorbis-1.3.4 && \
    LDFLAGS="-L${BUILD_DIR}/lib" CPPFLAGS="-I${BUILD_DIR}/include" ./configure --prefix="${BUILD_DIR}" --with-ogg="${BUILD_DIR}" --disable-shared && \
    make -j $(nproc) && \
    make install && \
    make distclean

# install libvpx
RUN git clone --depth 1 https://chromium.googlesource.com/webm/libvpx.git && \
    cd libvpx && \
    ./configure --prefix="${BUILD_DIR}" --disable-examples && \
    make -j $(nproc) && \
    make install && \
    make distclean

# install libtheora
RUN curl -O http://downloads.xiph.org/releases/theora/libtheora-1.1.1.tar.bz2 && \
    tar jxvf libtheora-1.1.1.tar.bz2 && \
    cd libtheora-1.1.1 && \
    ./configure --prefix="${BUILD_DIR}" --with-ogg="${BUILD_DIR}" --bindir="${BIN_DIR}" --disable-shared && \
    make -j $(nproc) && \
    make install && \
    make distclean

# install faac + http://stackoverflow.com/a/4320377
RUN curl -L -O http://downloads.sourceforge.net/faac/faac-1.28.tar.gz  && \
    tar xzvf faac-1.28.tar.gz  && \
    cd faac-1.28 && \
    sed -i '126d' common/mp4v2/mpeg4ip.h && \
    ./bootstrap && \
    ./configure --prefix="${BUILD_DIR}" --bindir="${BIN_DIR}" && \
    make -j $(nproc) && \
    make install && \
    make distclean

# install xvid
RUN curl -L -O  http://downloads.xvid.org/downloads/xvidcore-1.3.4.tar.gz  && \
    tar xzvf xvidcore-1.3.4.tar.gz && \
    cd xvidcore/build/generic && \
    ./configure --prefix="${BUILD_DIR}" --bindir="${BIN_DIR}" && \
    make -j $(nproc) && \
    make install&& \
    rm -rf ${DIR}

# ffmpeg
RUN curl -O http://ffmpeg.org/releases/ffmpeg-2.8.6.tar.gz && \
    tar xzvf ffmpeg-2.8.6.tar.gz && \
    cd ffmpeg-2.8.6 && \
    ./configure --prefix="${BUILD_DIR}" --extra-cflags="-I${BUILD_DIR}/include" --extra-ldflags="-L${BUILD_DIR}/lib" --bindir="${BIN_DIR}" \
    --extra-libs=-ldl --enable-version3 --enable-libfaac --enable-libmp3lame \
    --enable-libx264 --enable-libxvid --enable-gpl \
    --enable-postproc --enable-nonfree --enable-avresample --enable-libfdk_aac \
    --disable-debug --enable-small --enable-openssl --enable-libtheora \
    --enable-libopus --enable-libvorbis --enable-libvpx \
    --enable-libfreetype --enable-libass && \
    make -j $(nproc) && \
    make install && \
    make distclean && \
    hash -r

# set environment
RUN echo "${BUILD_DIR}/lib" > /etc/ld.so.conf.d/libc.conf

WORKDIR /workspace
