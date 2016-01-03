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
  'rsync'
)

# By default makepkg runs strip on binaries. This seems to cause issues with the Swift REPL.
# Disable it in the PKGBUILD with:
options=(!strip)

source=(
  'swift-linker.patch'
  '0001-Fix-linker-not-finding-pthreads-and-dl.patch'
  '0001-bootstrap-Support-Python-2-and-3-in-the-bootstrap-sc.patch'
  '0001-Make-it-work-on-Python-2-and-3.patch'
  'fix-lldb-build.patch'
  '0001-swift-llvm-Conform-to-PEP-0394-in-Python-sources.patch'
  '0001-Provide-a-custom-preset-for-Arch-Linux.patch'
  '0001-Prefer-XZ-compression-over-Gzip-compression.patch'
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
  '30752850faacaeb5ad2fe7d09f7c6f2801fb1a1fc1a7b9e56224ee774d22eb45'
  '2180c1554b3ac41fb0fc2a679965b78e676e07c48e8adb62200c9422a6235895'
  '6a5920e5a1667a2c0e0398c3ad7083c89d22a956f9036c17a46df300f1f30fd1'
  'c62a1a903a9849be53f5bb9cc6f701f0a2409e188a38b9df5cc565e9b8f3f9ba'
  '4aa04411d35cf08d093d574b21b755562596c2e6e8d367a75beb9ee7010271a6'
  '6c876a071616abe0d79ac64c5520662e3f0a68bf1fb9aac98cad7c9003077331'
  'c864e35300e8fee8352a4e9b3d1634612e1e2f7dafc052efa12117cbab6fdfc0'
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
  # Prefer LZMA2 compression for smaller files. Merge request(s) upstream:
  # https://github.com/apple/swift/pull/801
  git apply "$srcdir/0001-Prefer-XZ-compression-over-Gzip-compression.patch"
  git apply "$srcdir/0001-Provide-a-custom-preset-for-Arch-Linux.patch"
  # Fix linker not finding pthreads or dl. Merge request(s) upstream:
  # https://github.com/apple/swift/pull/435
  git apply "$srcdir/0001-Fix-linker-not-finding-pthreads-and-dl.patch"
  cd "$srcdir/lldb"
  git apply "$srcdir/fix-lldb-build.patch"
  cd "$srcdir/swiftpm"
  git apply "$srcdir/0001-bootstrap-Support-Python-2-and-3-in-the-bootstrap-sc.patch"
  cd "$srcdir/swift-corelibs-foundation"
  git apply "$srcdir/0001-Make-it-work-on-Python-2-and-3.patch"
  cd "$srcdir/llvm"
  git apply "$srcdir/0001-swift-llvm-Conform-to-PEP-0394-in-Python-sources.patch"
}

package() {
  installable_package="$(readlink -f ${srcdir}/../swift-${pkgver}.tar.xz)"
  "$srcdir/swift/utils/build-script" --preset=buildbot_arch_linux installable_package="${installable_package}" install_destdir="$pkgdir/"
  tar xf "${installable_package}" -C "$pkgdir/"
}
