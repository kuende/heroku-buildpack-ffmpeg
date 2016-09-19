## Build script

```bash
#!/usr/bin/env bash

export BUILD_PATH=/tmp
export OUT_PATH=/app/vendor/ffmpeg
export PKG_CONFIG_PATH=$OUT_PATH/lib/pkgconfig:$PKG_CONFIG_PATH
export PATH=$OUT_PATH/bin:$PATH

cd $BUILD_PATH

apt-get update && apt-get install -y --no-install-recommends automake build-essential pkg-config yasm

curl -sLO http://ffmpeg.org/releases/ffmpeg-3.1.3.tar.gz
tar zxf ffmpeg-3.1.3.tar.gz
pushd ffmpeg-3.1.3

./configure                                                 \
  --prefix=$OUT_PATH                                        \
  --disable-network                                         \
  --disable-protocols                                       \
  --enable-protocol=file                                    \
  --disable-swscale                                         \
  --disable-postproc                                        \
  --disable-filters                                         \
  --enable-filter=aresample                                 \
  --disable-muxers                                          \
  --enable-muxer=null                                       \
  --disable-debug                                           \
  --disable-doc

make && make install

popd

cd $OUT_PATH
tar -cvzf output.tar.gz *
```
