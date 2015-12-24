_gitbranch='master'
pkgname='swiftc'
pkgver="2.2.20151222a"
pkgver() {
  cd "$srcdir/swift"
  git describe --long --tags | sed -r 's/swift-([0-9]+\.[0-9]+)-SNAPSHOT-([0-9]+)-([0-9]+)-([0-9]+)-([a-z]+)-([0-9]+)/\1.\2\3\4\5.r\6/g;s/-/./g'
}
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
  "swift-corelibs-foundation::git+http://github.com/apple/swift-corelibs-foundation.git#branch=${_gitbranch}"
  "swift-integration-tests::git+http://github.com/apple/swift-integration-tests.git#branch=${_gitbranch}"
)

sha256sums=(
  '70fd23665a68c1575113d1198b3088b49636bad90b5f068563076af362f63f68'
  'e805293e4a53de148b9bf5bb71a20eb5c8ea75d20eaf8fbce943eaa3c1eb2221'
  '462ee79249dc2569fe73fcc22c497a693e033d2dee35e6490407a92b8d74b208'
  '1db1eb7562ab3c730021540e1774a3f7726449f8802221f5f741d53767c01d88'
  '09a2a967259086d877be81731cd5283a36ce860f50a6ea72499ff5f9c9ec9777'
  'c62a1a903a9849be53f5bb9cc6f701f0a2409e188a38b9df5cc565e9b8f3f9ba'
  '78a900f7b9084c393b3008c851e6d276e3ad6f49c0892eb2d548e564fbd25211'
  '4aa04411d35cf08d093d574b21b755562596c2e6e8d367a75beb9ee7010271a6'
  '3421438f82b9b41d6af3c2edad5b843c965dedf46c2200d0dfc1d1c69c08b286'
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

package() {
  "$srcdir/swift/utils/build-script" --preset=buildbot_arch_linux prefix=usr destdir="$pkgdir/"
}
