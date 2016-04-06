_gitbranch='master'
pkgname='swiftc'
pkgver='3.0.20160208a.r266.g92fc7db'
pkgver() {
  cd "$srcdir/swift"
  git describe --long --tags | sed -r 's/swift.DEVELOPMENT-SNAPSHOT-([0-9]+)-([0-9]+)-([0-9]+)-([a-z]+)-([0-9]+)/3.0.\1\2\3\4.r\5/g;s/-/./g'
}
pkgrel='1'
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
  'libbsd'
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
  'b71e2498d47ff977511e85510f251eca964a4a0433b71070c9cdb9ffe92a2153'
  '73228947aeffada398366afd9e89ea040ca6bc9334057ab89fa82d799b41add6'
  'c864e35300e8fee8352a4e9b3d1634612e1e2f7dafc052efa12117cbab6fdfc0'
  # SR-1023
  'cb81c24fc7013dbd0bd6f58fc274899b4e73142959242462cd79c09d5e4c3c02'
  '9bf13a5fd2e55c33adcaebc1384552ddc7956744261bf0cd173da820e3515274'
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
