# qpdf
VCS CUPS repository for :
* https://copr.fedorainfracloud.org/coprs/dkosovic/printing-el8/
* https://copr.fedorainfracloud.org/coprs/dkosovic/printing-el9/

This repository started off as a git clone of the rawhide branch of qpdf from
Fedora Package Sources:
* https://src.fedoraproject.org/rpms/qpdf/tree/rawhide

This repository contains a patch to silence QPDF warnings with
macOS CUPS clients that can become errors, see following for more
details of the issue:
* https://github.com/OpenPrinting/cups/issues/321

### Building on Fedora Copr

Select **Custom** for the source type.

Copy and paste the following script into the custom script text box:

```sh
#! /bin/sh

set -x # verbose output
set -e # fail the whole script if some command fails
                 
git clone https://github.com/eait-cups-printing/qpdf.git
cp -p qpdf/* .

name=`grep Name: qpdf.spec | awk '{ print $2 }'`
version=`grep Version: qpdf.spec | awk '{ print $2 }'`
source0=`grep Source0: qpdf.spec | awk '{print $2}' | sed -e "s/%{name}/$name/g" -e "s/%{version}/$version/g"`
source1=`grep Source1: qpdf.spec | awk '{print $2}' | sed -e "s/%{name}/$name/g" -e "s/%{version}/$version/g"`

curl -OL $source0
curl -OL $source1
```

Copy and paste the following into the build dependencies field:
```
git
gcc
gcc-c++
cmake
zlib-devel
libjpeg-turbo-devel
gnutls-devel
perl-generators
perl-interpreter
perl(Carp)
perl(Config)
perl(constant)
perl(Cwd)
perl(Digest::MD5)
perl(Digest::SHA)
perl(File::Basename)
perl(File::Compare)
perl(File::Copy)
perl(File::Find)
perl(File::Spec)
perl(FileHandle)
perl(IO::Handle)
perl(IO::Select)
perl(IO::Socket)
perl(POSIX)
perl(strict)
unzip
```
