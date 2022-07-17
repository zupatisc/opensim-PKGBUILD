# Maintainer: Patrik Tischmann <patrik.tischmann@stud.hs-ruhrwest.de>
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
source=("$pkgname-core-$pkgver-source.tar.gz::https://github.com/opensim-org/opensim-core/archive/refs/tags/4.3.tar.gz"
    "$pkgname-gui-source::git+https://github.com/opensim-org/opensim-gui")
md5sums=("d063ebde0f2f96b4e27fb47ac7dd25a1" SKIP)

prepare() {
    mv $pkgname-core-4.3 $pkgname-core-source
    cd $pkgname-gui-source
    git submodule update --init --recursive -- opensim-models opensim-visualizer Gui/opensim/threejs

    cd $srcdir
    mkdir -p $pkgname-core-source/dependencies/build
    mkdir -p $pkgname-core-source/build
    mkdir -p $pkgname-gui-source/build
}

build() {
    # Building OpenSim dependencies
    cd $srcdir/$pkgname-core-source/dependencies/build
    cmake $srcdir/$pkgname-core-source/dependencies -DCMAKE_INSTALL_PREFIX=$srcdir/$pkgname-core-dependencies/ -DCMAKE_BUILD_TYPE='Release' -DSUPERBUILD_simbody=ON -DSUPERBUILD_spdlog=ON -DSUPERBUILD_ezc3d=ON\
        -DSUPERBUILD_docopt=ON -DSUPERBUILD_BTK=OFF -DOPENSIM_WITH_CASADI=ON -DOPENSIM_WITH_TROPTER=ON
    make -j8

    # Building OpenSim core
    cd $srcdir/$pkgname-core-source/build
    cmake ../ -DCMAKE_INSTALL_PREFIX=$srcdir/$pkgname-core -DCMAKE_BUILD_TYPE='RelWithDebInfo' -DOPENSIM_DEPENDENCIES_DIR=$srcdir/$pkgname-core-dependencies/ -DOPENSIM_C3D_PARSER=ezc3d\
        -DBUILD_PYTHON_WRAPPING=ON -DBUILD_JAVA_WRAPPING=ON -DWITH_BTK=OFF -DWITH_EZC3D=ON -DBUILD_TESTING=OFF -DOPENSIM_INSTALL_UNIX_FHS=OFF -DOPENSIM_COPY_DEPENDENCIES=ON\
        -DSWIG_DIR=/usr/share/swig -DSWIG_EXECUTABLE=/usr/bin/swig -DOPENSIM_COPY_DEPENDENCIES=ON -DCMAKE_BUILD_TYPE='Release'  -DOPENSIM_WITH_CASADI=ON -DOPENSIM_WITH_TROPTER=ON
    make -j2
    make install

    # Building OpenSim gui
    cd $srcdir/$pkgname-gui-source/build
    cmake ../ -DCMAKE_PREFIX_PATH=$srcdir/$pkgname-core -DAnt_EXECUTABLE="/usr/share/apache-netbeans/extide/ant/bin/ant" -DANT_ARGS="-Dnbplatform.default.netbeans.dest.dir=/usr/share/apache-netbeans;-Dnbplatform.default.harness.dir=/usr/share/apache-netbeans/harness"
    make CopyOpenSimCore
    make PrepareInstaller
}

package() {
	cd "$pkgname-gui-source/Gui/opensim/dist/installer/opensim"
    bash INSTALL
}
