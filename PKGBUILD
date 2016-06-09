_gittag='swift-DEVELOPMENT-SNAPSHOT-2016-06-06-a'
pkgname='swiftc'
pkgver=3.0.20160606a.r0.g9e8266a
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
  'python-pexpect'
  'python2-pexpect'
)

provides=(
  'swift-language'
  'lldb'
  'swift-lldb'
)

conflicts=(
  'lldb'
  'swift-lldb'
  'swift-language-git'
  'swift-git'
  'swift-bin'
  'swift'
)

# By default makepkg runs strip on binaries. This seems to cause issues with the Swift REPL.
# Disable it in the PKGBUILD with:
options=(!strip)

# /etc/makepkg.conf has this defined as "-Wl,-01,--sort-common,--as-needed,-z,relro"
# I think this is an issue not sure why though
LDFLAGS=""

source=(
  '0001-Not-sure-this-is-right-but-it-is-something.patch'
  '0001-Python-3-compat.patch'
  '0002-Add-a-git-configured-user-email.patch'
  'fix-lldb-build.patch'
  '0001-Provide-a-custom-preset-for-Arch-Linux.patch'
  '0001-build-script-Reduce-the-size-of-development-snapshot.patch'
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
  '8ea3928318e32892d08f87f2b01b0bf2cdb1afc370e5e5e581bda09e9d230827'
  'SKIP'
  'SKIP'
  'b71e2498d47ff977511e85510f251eca964a4a0433b71070c9cdb9ffe92a2153'
  '838bcb4381f8547c392706605bd17e59210fb485df3f4607aa41fd1c5f4090b9'
  'ad464f292ca6066a1d56d62921611b3ab7190abeed8dfaf31fb7df508d8a7e10'
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
  git apply "$srcdir/0001-build-script-Reduce-the-size-of-development-snapshot.patch"
  git apply "$srcdir/0001-Provide-a-custom-preset-for-Arch-Linux.patch"

  # Docs don't work right
  git apply "$srcdir/0001-Not-sure-this-is-right-but-it-is-something.patch"

  cd "$srcdir/lldb"
  # This is a proposed patch provided by the community. This patches Python 3 compatibility.
  # https://bugs.swift.org/browse/SR-14
  git apply "$srcdir/fix-lldb-build.patch"

  cd "$srcdir/swift-integration-tests"
  git apply "$srcdir/0001-Python-3-compat.patch"
  git apply "$srcdir/0002-Add-a-git-configured-user-email.patch"
}

package() {
  export LC_CTYPE=en_US.UTF-8
  export LANG=en_US.UTF-8
  installable_package="$(readlink -f ${srcdir}/../swift-${pkgver}.tar.xz)"
  cd "$srcdir/swift"
  utils/build-script --preset=buildbot_arch_linux installable_package="${installable_package}" install_destdir="$pkgdir/"
  tar xf "${installable_package}" -C "$pkgdir/"
}
