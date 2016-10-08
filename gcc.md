##Gcc

###Install

```
wget http://gcc.parentingamerica.com/releases/gcc-5.4.0/gcc-5.4.0.tar.bz2
tar axf gcc-5.4.0.tar.bz2
yum install -y gmp-devel mfpr-devel libmpc-devel zip
cd gcc-5.4.0
mkdir build
cd build
../configure --disable-multilib
make && make install
```