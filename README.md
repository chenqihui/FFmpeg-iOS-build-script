
# 官方Mac

* [CompilationGuide/macOS – FFmpeg](https://trac.ffmpeg.org/wiki/CompilationGuide/macOS#Compiling)

# M1 环境编译问题记录

1、--disable-audiotoolbox

* [2023 最新iOS 开发之编译ffmpeg 及报错解析](https://www.jianshu.com/p/f5265b062f54)

2、iOS13

6.x 最低版本 iOS13，5.x 最低版本 iOS

3、bin2c_host.o not match

bin2c_host 一直报错，5.x 和 6.x 新增的，没有找到 configure 设置解决。

暂时直接在删除 ffbuild/common.mak 里面的，需要先下载到源码后才能删。

~~~
$(BIN2CEXE): ffbuild/bin2c_host.o
	$(HOSTLD) $(HOSTLDFLAGS) $(HOSTLD_O) $^ $(HOSTEXTRALIBS)
~~~

根据这句的 git commit log 也顺便禁止 --disable-ptx-compression	
	
~~~
avfilter: compress CUDA PTX code if possible
~~~

??

~~~
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/ranlib: archive member: FFmpegx86_64.a(bin2c_host.o) cputype (16777228) does not match previous archive members cputype (16777223) (all members must match)
fatal error: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/lipo: archive member FFmpegx86_64.a(bin2c_host.o) cputype (16777228) and cpusubtype (0) does not match previous archive members cputype (16777223) and cpusubtype (3) (all members must match)
~~~

4、--disable-metal

metal 配置？

~~~
ar: libavfilter/metal/vf_yadif_videotoolbox.metallib.o: No such file or directory
AR	libavcodec/libavcodec.a
make: *** [libavfilter/libavfilter.a] Error 1
~~~


# FFmpeg iOS build script

See the following repository for Swift package, .xcframeworks and more:

https://github.com/kewlbear/FFmpeg-iOS

[![Build Status](https://travis-ci.org/kewlbear/FFmpeg-iOS-build-script.svg?branch=master)](https://travis-ci.org/kewlbear/FFmpeg-iOS-build-script)

This is a shell script to build FFmpeg libraries for iOS and tvOS apps.

Tested with:

* FFmpeg 4.3.1
* Xcode 12.2

## Requirements

* https://github.com/libav/gas-preprocessor
* yasm 1.2.0

## Usage

Use build-ffmpeg-tvos.sh for tvOS.

* To build everything:

        ./build-ffmpeg.sh

* To build arm64 libraries:

        ./build-ffmpeg.sh arm64

* To build fat libraries for armv7 and x86_64 (64-bit simulator):

        ./build-ffmpeg.sh armv7 x86_64

* To build fat libraries from separately built thin libraries:

        ./build-ffmpeg.sh lipo

## Download

You can download a binary for FFmpeg 4.3.1 release at https://downloads.sourceforge.net/project/ffmpeg-ios/ffmpeg-ios-master.tar.bz2

## External libraries

You should link your app with

* libz.dylib
* libbz2.dylib
* libiconv.dylib
