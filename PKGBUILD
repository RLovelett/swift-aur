_gitbranch='master'
pkgname='swiftc'
pkgver="1.0.0_${_gitbranch}"
pkgrel='1'
arch=('x86_64')
license=('Apache')
pkgdesc='Swift Programming Language'
url='https://swift.org/'

makedepends=(
  'git'
  'cmake'
  'ninja'
  'python'
  'python2'
  'clang'
  'libbsd'
  'icu'
  'libedit'
  'libxml2'
  'sqlite'
  'swig'
  'ncurses'
)

source=(
  'swift-linker.patch'
  '0001-Change-shebangs-to-specify-python2-instead-of-system.patch'
  '0001-Fix-linker-not-finding-pthreads-and-dl.patch'
  '0001-Conform-to-PEP-0394-in-Python-sources.patch'
  '0001-swift-corelibs-foundation-Conform-to-PEP-0394-in-Python-sources.patch'
  'fix-lldb-build.patch'
  '0001-First.patch'
  '0001-swift-llvm-Conform-to-PEP-0394-in-Python-sources.patch'
  '0001-Provide-a-custom-preset-for-Arch-Linux.patch'
  "swift::git+http://github.com/apple/swift.git#branch=${_gitbranch}"
  "llvm::git+http://github.com/apple/swift-llvm.git#branch=stable"
  "clang::git+http://github.com/apple/swift-clang.git#branch=stable"
  "lldb::git+http://github.com/apple/swift-lldb.git#branch=${_gitbranch}"
  "cmark::git+http://github.com/apple/swift-cmark.git#branch=${_gitbranch}"
  "llbuild::git+http://github.com/apple/swift-llbuild.git#branch=${_gitbranch}"
  "swiftpm::git+http://github.com/apple/swift-package-manager.git#branch=${_gitbranch}"
  "swift-corelibs-xctest::git+http://github.com/apple/swift-corelibs-xctest.git#branch=${_gitbranch}"
  "swift-corelibs-foundation::git+http://github.com/apple/swift-corelibs-foundation.git#commit=a4f0b60ab12dfb85bd3f0374f699757db165fadd"
  "swift-integration-tests::git+http://github.com/apple/swift-integration-tests.git#branch=${_gitbranch}"
)

sha256sums=(
  'SKIP'
  'SKIP'
  '462ee79249dc2569fe73fcc22c497a693e033d2dee35e6490407a92b8d74b208'
  '1db1eb7562ab3c730021540e1774a3f7726449f8802221f5f741d53767c01d88'
  '09a2a967259086d877be81731cd5283a36ce860f50a6ea72499ff5f9c9ec9777'
  'c62a1a903a9849be53f5bb9cc6f701f0a2409e188a38b9df5cc565e9b8f3f9ba'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
)

prepare() {
  cd "$srcdir/swift"
  git apply "$srcdir/0001-Provide-a-custom-preset-for-Arch-Linux.patch"
  git apply "$srcdir/0001-Change-shebangs-to-specify-python2-instead-of-system.patch"
  git apply "$srcdir/swift-linker.patch"
  cd "$srcdir/lldb"
  git apply "$srcdir/fix-lldb-build.patch"
  cd "$srcdir/swiftpm"
  git apply "$srcdir/0001-Conform-to-PEP-0394-in-Python-sources.patch"
  cd "$srcdir/swift-corelibs-foundation"
  git apply "$srcdir/0001-swift-corelibs-foundation-Conform-to-PEP-0394-in-Python-sources.patch"
  git apply "$srcdir/0001-First.patch"
  cd "$srcdir/llvm"
  git apply "$srcdir/0001-swift-llvm-Conform-to-PEP-0394-in-Python-sources.patch"
}

build() {
  "$srcdir/swift/utils/build-script" --preset=buildbot_arch_linux prefix=usr destdir="$pkgdir/"
}

package() {
  "$srcdir/swift/utils/build-script" --preset=buildbot_arch_linux prefix=usr destdir="$pkgdir/"
}
