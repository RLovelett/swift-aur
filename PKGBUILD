_gitbranch='master'
pkgname='swiftc'
pkgver="1.0.0_${_gitbranch}"
pkgrel='1'
arch=('x86_64')
license=('Apache')
pkgdesc='Swift Programming Language'
url='https://swift.org/'
makedepends=('cmake' 'ninja' 'python' 'clang' 'libbsd' 'icu' 'libedit' 'libxml2' 'sqlite' 'swig' 'ncurses')
source=(
  '0001-Miscellaneous-fixes-for-Python-3-compatibility.patch'
  'headerpaths.patch'
  "swift::git+http://github.com/apple/swift.git#branch=${_gitbranch}"
  "llvm::git+http://github.com/apple/swift-llvm.git#branch=stable"
  "clang::git+http://github.com/apple/swift-clang.git#branch=stable"
  "lldb::git+http://github.com/apple/swift-lldb.git#branch=${_gitbranch}"
  "cmark::git+http://github.com/apple/swift-cmark.git#branch=${_gitbranch}"
  "llbuild::git+http://github.com/apple/swift-llbuild.git#branch=${_gitbranch}"
  "swiftpm::git+http://github.com/apple/swift-package-manager.git#branch=${_gitbranch}"
  "swift-corelibs-xctest::git+http://github.com/apple/swift-corelibs-xctest.git#branch=${_gitbranch}"
  "swift-corelibs-foundation::git+http://github.com/apple/swift-corelibs-foundation.git#branch=${_gitbranch}"
)
md5sums=('SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP')

prepare() {
  cd "$srcdir/swift"
  git apply "$srcdir/0001-Miscellaneous-fixes-for-Python-3-compatibility.patch"
  git apply "$srcdir/headerpaths.patch"
}

build() {
  cd "$srcdir/"
  swift/utils/build-script -R -- --verbose-build
}

check() {
  cd "$srcdir/"
  swift/utils/build-script -t
}

package() {
  cd "$srcdir/$pkgname"
  make PREFIX=usr DESTDIR="$pkgdir/" install
}
