# This is an example PKGBUILD file. Use this as a start to creating your own,
# and remove these comments. For more information, see 'man PKGBUILD'.
# NOTE: Please fill out the license field for your package! If it is unknown,
# then please put 'unknown'.

# Maintainer: Your Name <youremail@domain.com>
pkgname=opensim
pkgver=4.3
pkgrel=1
# epoch=
pkgdesc="SimTK OpenSim graphical user interface and distribution"
arch=("x86_64")
url="https://github.com/opensim-org/opensim-gui"
license=('Apache')
depends=("java-environment-openjdk=8" )
makedepends=("git" "gcc-fortran" "cmake" "swig" "lapack" "liblas" "gconf" "apache-netbeans") # 12.3 Is currently used in the projects CI
provides=("OpenSim")
source=("$pkgname-core-$pkgver.tar.gz::https://github.com/opensim-org/opensim-core/archive/refs/tags/4.3.tar.gz"
    "$pkgname-gui-$pkgver.tar.gz::https://github.com/opensim-org/opensim-gui/archive/refs/tags/4.3.tar.gz")
md5sums=("d063ebde0f2f96b4e27fb47ac7dd25a1" "aaa98e7bed7774e3ea358d44ffe2ee08")

prepare() {
    mkdir -p build_deps
    mkdir -p build_core
    mkdir -p build
}

build() {
    # Building OpenSim dependencies
    cd build_deps
    cmake ../opensim-core-$pkgver/dependencies -DSUPERBUILD_ezc3d:BOOL=on  -DOPENSIM_WITH_CASADI:BOOL=on -DOPENSIM_WITH_TROPTER:BOOL=on -DCMAKE_INSTALL_PREFIX=../opensim_dependencies_install
    cmake . -LAH
    cmake --build . --config Release

    # Building OpenSim core
    cd ../build_core
    cmake ../opensim-core-$pkgver -DOPENSIM_DEPENDENCIES_DIR=../opensim_dependencies_install -DBUILD_JAVA_WRAPPING=on -DBUILD_PYTHON_WRAPPING=on -DOPENSIM_C3D_PARSER=ezc3d -DBUILD_TESTING=off\
        -DCMAKE_INSTALL_PREFIX=../opensim-core-install -DOPENSIM_INSTALL_UNIX_FHS=OFF -DSWIG_DIR=/usr/share/swig -DSWIG_EXECUTABLE=/usr/bin/swig
    cmake . -LAH
    cmake --build . --config Release 
    cmake --install .
	# cd "$pkgname-$pkgver"
	# ./configure --prefix=/usr
	# make
}

# check() {
# 	cd "$pkgname-$pkgver"
# 	make -k check
# }

package() {
	cd "$pkgname-$pkgver"
	# make DESTDIR="$pkgdir/" install
}

# sudo apt-get update && sudo apt-get install --yes liblapack-dev freeglut3-dev libxi-dev libxmu-dev doxygen python3 python3-dev python3-numpy python3-setuptools
# swig
# netbeans-12.3
# libxmu;libxi
