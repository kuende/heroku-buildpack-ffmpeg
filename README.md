## Build script

```bash
#!/usr/bin/env bash

export BUILD_PATH=/tmp
export OUT_PATH=/app/vendor/ffmpeg
export PKG_CONFIG_PATH=$OUT_PATH/lib/pkgconfig:$PKG_CONFIG_PATH
export PATH=$OUT_PATH/bin:$PATH

cd $BUILD_PATH

apt-get update && apt-get install -y --no-install-recommends automake build-essential pkg-config yasm

curl -sLO http://ffmpeg.org/releases/ffmpeg-3.1.4.tar.gz
tar zxf ffmpeg-3.1.4.tar.gz
pushd ffmpeg-3.1.4

./configure                                                 \
  --prefix=$OUT_PATH                                        \
  --disable-network                                         \
  --disable-protocols                                       \
  --enable-protocol=file                                    \
  --enable-protocol=pipe                                    \
  --disable-postproc                                        \
  --disable-filters                                         \
  --enable-filter=aresample                                 \
  --enable-filter=rotate                                    \
  --enable-filter=metadata                                  \
  --enable-filter=transpose                                 \
  --enable-filter=scale                                     \
  --disable-muxers                                          \
  --enable-muxer=null                                       \
  --enable-muxer=mjpeg                                      \
  --disable-debug                                           \
  --disable-doc

make -j4 && make install

popd

cd $OUT_PATH
tar -cvzf output.tar.gz *
```

## Uploading

```
curl -T output.tar.gz -unvartolomei:<API_KEY> https://api.bintray.com/content/nvartolomei/heroku-buildpack-ffmpeg/ffmpeg/3.1.4/ffmpeg-3.1.4.tar.gz
```
