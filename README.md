# MakeOpenBlasStandaloneDLL
OpenBlas without libgcc_x.dll , libgortran_X.dll and libgomp.dll and so on


# build in msys2

e.g.  mingw-w64-x86_64-gcc and mingw-w64-x86_64-gfortran

cd ~
cp ../../mingw64/bin/ar.exe ../../mingw64/bin/x86_64-w64-mingw32-ar.exe
cp ../../mingw64/bin/ranlib.exe ../../mingw64/bin/x86_64-w64-mingw32-ranlib.exe

cd /you/path/openblas-x.x.x
make BINARY=64 HOSTCC=gcc CC=x86_64-w64-mingw32-gcc FC=x86_64-w64-mingw32-gfortran \
CFLAGS='-static-libgcc -static-libstdc++ -static -ggdb' FFLAGS='-static' \
NO_AFFINITY=1 USE_OPENMP=1 DYNAMIC_ARCH=1

# ERROR
if you see following error:
libgfortran.a(write.o):(.text$get_precision+0x156): undefined reference to `quadmath_snprintf'
libgfortran.a(write.o):(.text$get_float_string+0x170): undefined reference to `quadmath_snprintf'
libgfortran.a(write.o):(.text$get_float_string+0xa62): undefined reference to `quadmath_snprintf'
libgfortran.a(write.o):(.text$get_float_string+0x165c): undefined reference to `quadmath_snprintf'
libgfortran.a(write.o):(.text$get_float_string+0x173a): undefined reference to `quadmath_snprintf'
collect2.exe: error: ld returned 1 exit status

cd /you/path/openblas-x.x.x/export


-defaultlib:advapi32 -lgfortran -lquadmath -defaultlib:advapi32 -lgfortran -lgomp

like this
x86_64-w64-mingw32-gcc -static-libgcc -lgomp -static-libstdc++ -static -ggdb -lquadmath -O2 \
-DMS_ABI -DMAX_STACK_ALLOC=2048 -fopenmp -Wall -m64 -DF_INTERFACE_GFORT -DNO_AVX512 -DSMP_SERVER \
-DUSE_OPENMP -DNO_WARMUP -DMAX_CPU_NUMBER=4 -DMAX_PARALLEL_NUMBER=1 -DVERSION=\"0.3.6\" -DASMNAME= \
-DASMFNAME=_ -DNAME=_ -DCNAME= -DCHAR_NAME=\"_\" -DCHAR_CNAME=\"\" -DNO_AFFINITY \
-I..  libopenblas.def dllinit.obj -shared -o ../libopenblas.dll -Wl,--out-implib,../libopenblas.dll.a \
-Wl,--whole-archive ../libopenblas_zenp-r0.3.6.a -Wl,--no-whole-archive  -defaultlib:advapi32 -lgfortran -lquadmath \
-defaultlib:advapi32 -lgfortran -lgomp
