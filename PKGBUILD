_gittag='swift-3.0.1-RELEASE'
pkgname='swiftc'
pkgver=3.0.1
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
  'libkqueue'
)

# By default makepkg runs strip on binaries. This seems to cause issues with the Swift REPL.
# Disable it in the PKGBUILD with:
options=(!strip)

# /etc/makepkg.conf has this defined as "-Wl,-01,--sort-common,--as-needed,-z,relro"
# I think this is an issue not sure why though
LDFLAGS=""

source=(
  'fix-lldb-build.patch'
  'build-presets.ini'
  '0001-build-script-Reduce-the-size-of-development-snapshot.patch'
  "swift::git+https://github.com/apple/swift.git#tag=${_gittag}"
  "llvm::git+https://github.com/apple/swift-llvm.git#tag=${_gittag}"
  "clang::git+https://github.com/apple/swift-clang.git#tag=${_gittag}"
  "lldb::git+https://github.com/apple/swift-lldb.git#tag=${_gittag}"
  "cmark::git+https://github.com/apple/swift-cmark.git#tag=${_gittag}"
  "llbuild::git+https://github.com/apple/swift-llbuild.git#tag=${_gittag}"
  "swiftpm::git+https://github.com/apple/swift-package-manager.git#tag=${_gittag}"
  "swift-corelibs-xctest::git+https://github.com/apple/swift-corelibs-xctest.git#tag=${_gittag}"
  "swift-corelibs-foundation::git+https://github.com/apple/swift-corelibs-foundation.git#tag=${_gittag}"
  "swift-corelibs-libdispatch::git+https://github.com/apple/swift-corelibs-libdispatch.git#commit=${_gittag}"
  "swift-integration-tests::git+https://github.com/apple/swift-integration-tests.git#tag=${_gittag}"
)

sha256sums=(
  'b71e2498d47ff977511e85510f251eca964a4a0433b71070c9cdb9ffe92a2153'
  'ef994c98a430cb4a0c8694716d8f9e132f43a9081dbffdbaa44f5f5132d34a26'
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
  'SKIP'
)

prepare() {
  cd "$srcdir/swift-corelibs-libdispatch"
  git submodule update --init --recursive

  cd "$srcdir/swift"
  # Prefer LZMA2 compression for smaller files. Merge request(s) upstream:
  # https://github.com/apple/swift/pull/801
  git apply "$srcdir/0001-build-script-Reduce-the-size-of-development-snapshot.patch"

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
  utils/build-script \
    --preset-file="${srcdir}/build-presets.ini" \
    --preset=buildbot_arch_linux \
    installable_package="${installable_package}" \
    install_destdir="$pkgdir/"
  tar xf "${installable_package}" -C "$pkgdir/"
}
