_gittag='swift-DEVELOPMENT-SNAPSHOT-2016-04-25-a'
pkgname='swiftc'
pkgver=3.0.20160412a.r0.g36739f7
pkgver() {
  cd "$srcdir/swift"
  git describe --long --tags | sed -r 's/swift.DEVELOPMENT-SNAPSHOT-([0-9]+)-([0-9]+)-([0-9]+)-([a-z]+)-([0-9]+)/3.0.\1\2\3\4.r\5/g;s/-/./g'
}
pkgrel=1
arch=('x86_64')
license=('Apache')
pkgdesc='Swift Programming Language'
url='https://swift.org/'

depends=(
  'libxml2'
  'python'
  'python2'
  'libutil-linux'
  'libedit'
  'icu'
)

makedepends=(
  'git'
  'cmake'
  'ninja'
  'clang'
  'sqlite'
  'swig'
  'ncurses'
  'rsync'
)

conflicts=(
  'lldb'
)

# By default makepkg runs strip on binaries. This seems to cause issues with the Swift REPL.
# Disable it in the PKGBUILD with:
options=(!strip)

# /etc/makepkg.conf has this defined as "-Wl,-01,--sort-common,--as-needed,-z,relro"
# I think this is an issue not sure why though
LDFLAGS=""

source=(
  'fix-lldb-build.patch'
  '0001-Provide-a-custom-preset-for-Arch-Linux.patch'
  '0001-Prefer-XZ-compression-over-Gzip-compression.patch'
  # SR-1023
  '0001-Work-around-relocation-R_X86_64_PC32-link-error.patch'
  '0002-Update-the-driver.patch'
  "swift::git+http://github.com/apple/swift.git#tag=${_gittag}"
  "llvm::git+http://github.com/apple/swift-llvm.git#tag=${_gittag}"
  "clang::git+http://github.com/apple/swift-clang.git#tag=${_gittag}"
  "lldb::git+http://github.com/apple/swift-lldb.git#tag=${_gittag}"
  "cmark::git+http://github.com/apple/swift-cmark.git#tag=${_gittag}"
  "llbuild::git+http://github.com/apple/swift-llbuild.git#tag=${_gittag}"
  "swiftpm::git+http://github.com/apple/swift-package-manager.git#tag=${_gittag}"
  "swift-corelibs-xctest::git+http://github.com/apple/swift-corelibs-xctest.git#tag=${_gittag}"
  "swift-corelibs-foundation::git+http://github.com/apple/swift-corelibs-foundation.git#tag=${_gittag}"
  "swift-integration-tests::git+http://github.com/apple/swift-integration-tests.git#tag=${_gittag}"
)

sha256sums=(
  'b71e2498d47ff977511e85510f251eca964a4a0433b71070c9cdb9ffe92a2153'
  'c033189fac6e5693d63ac2548ff945a1f8d4e30a09f15a54033b30b2ee513731'
  'c864e35300e8fee8352a4e9b3d1634612e1e2f7dafc052efa12117cbab6fdfc0'
  # SR-1023
  '1773de10c55880a2337a66b06769c30ee07415cdcf24921beb352c407025d344'
  '7b8418dd7f7ad269285e680a85209924c29b126fed83685f841c6b1bb999c517'
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
  # Support relative relocation against protected symbols in binutils 2.26
  # https://bugs.swift.org/browse/SR-1023
  # https://github.com/apple/swift/pull/2012
  git apply "$srcdir/0001-Work-around-relocation-R_X86_64_PC32-link-error.patch"
  git apply "$srcdir/0002-Update-the-driver.patch"

  cd "$srcdir/lldb"
  # This is a proposed patch provided by the community. This patches Python 3 compatibility.
  # https://bugs.swift.org/browse/SR-14
  git apply "$srcdir/fix-lldb-build.patch"
}

package() {
  export LC_CTYPE=en_US.UTF-8
  export LANG=en_US.UTF-8
  installable_package="$(readlink -f ${srcdir}/../swift-${pkgver}.tar.xz)"
  cd "$srcdir/swift"
  utils/build-script --preset=buildbot_arch_linux installable_package="${installable_package}" install_destdir="$pkgdir/"
  tar xf "${installable_package}" -C "$pkgdir/"
}
