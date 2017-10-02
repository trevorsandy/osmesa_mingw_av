# Mesa 3D OSMesa build on AppVeyor CI (MinGW)
[![Build status](https://ci.appveyor.com/api/projects/status/78dsh6h772pdjc5l?svg=true)](https://ci.appveyor.com/project/trevorsandy/osmesa-mingw-av)

Build Mesa/OSMesa on MinGW-w64 using AppVayor CI. The information below guids you through the steps to archive and upload the content you wish to build.

### To create a tar.gz archive:
- Clone and create:
```
$ git clone git://anongit.freedesktop.org/git/mesa/mesa
$ cd mesa
$ git checkout 17.1
$ git archive --format=tar.gz --prefix=mesa-17.1.3.X HEAD > mesa-17.1.3.X.tar.gz
```
- Extract:
```
$ tar -xzvf mesa-17.1.3.X.tar.gz
```
- Extract Specific Dir:
```
$ tar -xzvf mesa-17.1.3.X.tar.gz -C /tmp
```

- Notes:
Replace the 17.1 branch with master or any other branch as desired  
Replace HEAD with tag or commit sha as necessary  
Replace 17.1.3.X with your specified version string  

### To run locally (assuming you have MSYS2/MinGW-w64 set up):
```
$ git clone https://github.com/trevorsandy/osmesa_mingw_av.git
$ cd osmesa_mingw_av
$ mesaversion=17.1.3.X
$ MSYSTEMDIR=msys64
$ APPVEYOR_BUILD_FOLDER=`pwd`
$ if ! test -d ${APPVEYOR_BUILD_FOLDER}/build/osmesa; then mkdir -p ${APPVEYOR_BUILD_FOLDER}/build/osmesa && echo 'folder ${APPVEYOR_BUILD_FOLDER}/build/osmesa created'; fi
$ if test -f ${APPVEYOR_BUILD_FOLDER}/osmesa-install.sh; then chmod +x ${APPVEYOR_BUILD_FOLDER}/osmesa-install.sh && echo 'osmesa-install.sh set to executable'; fi
$ cd ${APPVEYOR_BUILD_FOLDER}/build; BUILD_FOLDER=`pwd`; env IGNORE_DEMO=1 DEMO_MODE=0 DEMO_DRIVER=3 INTERACTIVE=0 SILENT_LOG=0 MANGLED=0 CLEAN=0 GLUT_BUILD=1 USE_SYSTEM_GLUT=0 LLVM_BUILD=0 USE_SYSTEM_LLVM=1 LLVM_PREFIX=/${MSYSTEM} OSMESA_VERSION=${mesaversion} OSMESA_PREFIX=${BUILD_FOLDER}/osmesa ../osmesa-install.sh
```
### To run on AppVeyor CI:
see appveyor.yml

### Command line options:
Command line options or environment variables used by this script:  
- OSMESA_PREFIX: where to install osmesa (must be writeable)  
- OSMESA_VERSION: mesa version (set to the latest version by default)  
- LLVM_PREFIX: where llvm is / should be installed  
- LLVM_VERSION: llvm version (set to the latest version by default)  
- LLVM_BUILD: whether to build LLVM (0/1, default is 0)  
- USE_SYSTEM_LLVM: if using system llvm libs (0/1, default is 0)  
- MESA_BUILD: use when mesa already built but want to build other components (0/1, default is 1)  
- GLU_BUILD: use to skip building glu - e.g. on MinGW builds (0/1, default is 0 for MinGW, otherwise 1)  
- MACOSX_DEPLOYMENT_TARGET: minimum MacOSX SDK version (default is 10.8)  
- OSX_SDKSYSROOT: specify the location or name of OSX SDK (0/<sdk full path>, default is 0)  
- MKJOBS: number of parallel make jobs (4 by default)  
- IGNORE_DEMO: do not download, build and run the MESA demo (0/1, 0 by default)  
- IGNORE_BUILD_DEMO: do not build the demo - e.g. when already built (0/1, default is 0)  
- SILENT_LOG: redirect output and error to log file (0/1, default is 0)  
- USE_SYSTEM_GLUT: if using system glut - e.g. freeglut for MinGW (0/1, default is 0)  
- GLUT_BUILD: use to build glut - if not already built (0/1, default is 1)  
- DEBUG: build debug version (0/1, default is 0)  
- CLEAN: delete compiled source on recompile (0/1, default is 1)  
- INTERACTIVE: manually review and accept options  
- MANGLED: mangle mesa and glu (0/1, default is 1)  
- OSMESA_DRIVER: default dirver :1-classic, 2-softpipe, 3-llvmpipe and 4-swr (1-4, default is 4)  
- DEMO_MODE: disable all download and build logic used to test drivers (0/1, default is 0)  
- DEMO_DRIVER: same option choices as OSMESA_DRIVER (1-4, default is 3)  


- options above can be edited directly in the script or from the command line  
- an example using the command-line "env SILENT_LOG=1 LLVM_BUILD=1 ../osmesa-install.sh"  
- Note: for OSX_SDKSYSROOT, do not include 'isysroot' on the command line - automatically added by the install script  
