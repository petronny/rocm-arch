# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=rocrand
pkgver=2.3.0
pkgrel=1
pkgdesc="RAND library for HIP programming language"
url="https://github.com/ROCmSoftwarePlatform/rocRAND"
arch=(x86_64)
makedepends=("hcc>=2.3.0" git cmake ninja)
source=("https://github.com/ROCmSoftwarePlatform/rocRAND/archive/$pkgver.tar.gz")
sha256sums=("981224d84c5b4883393f01853d0f052cc10562d86cdb0918e04a1e522ad6cefa")

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  # build broken build with stack protection
  export CXXFLAGS=$(echo $CXXFLAGS | sed -e 's/-fstack-protector-strong//')
  export CFLAGS=$(echo $CFLAGS | sed -e 's/-fstack-protector-strong//')
  export CPPFLAGS=$(echo $CPPFLAGS | sed -e 's/-fstack-protector-strong//')

  cmake -DCMAKE_BUILD_TYPE=Release \
        -DBUILD_TEST=OFF \
        -DCMAKE_CXX_COMPILER=/opt/rocm/hcc/bin/hcc \
        "$srcdir/rocRAND-$pkgver"

  make
}

package() {
  cd "$srcdir/build"
  DESTDIR="$pkgdir" make install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocrand.conf
/opt/rocm/hiprand/lib/
/opt/rocm/rocrand/lib/
		EOF
}
