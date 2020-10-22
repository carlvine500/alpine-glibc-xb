# Maintainer: SpectX DevOps <devops@spectx.com>

pkgname="glibc"
pkgver="${GLIBC_VERSION}"
pkgrel="0"
pkgdesc="GNU C Library compatibility layer"
arch="${TARGETARCH}"
url="https://github.com/lauri-nomme/alpine-glibc-xb"
license="LGPL"
source="glibc-bin-${GLIBC_VERSION}.tar.gz
nsswitch.conf
ld.so.conf"
subpackages="$pkgname-bin $pkgname-dev $pkgname-i18n"
triggers="$pkgname-bin.trigger=/lib:/usr/lib:/usr/glibc-compat/lib"

package() {
  mkdir -p "$pkgdir/lib" "$pkgdir/lib64" "$pkgdir/usr/glibc-compat/lib/locale" "$pkgdir/usr/glibc-compat/lib64" "$pkgdir/etc"
  cp -a "$srcdir"/usr "$pkgdir"
  cp "$srcdir"/nsswitch.conf "$pkgdir"/etc/nsswitch.conf
  cp "$srcdir"/ld.so.conf "$pkgdir"/usr/glibc-compat/etc/ld.so.conf
  rm "$pkgdir"/usr/glibc-compat/etc/rpc
  rm -rf "$pkgdir"/usr/glibc-compat/bin
  rm -rf "$pkgdir"/usr/glibc-compat/sbin
  rm -rf "$pkgdir"/usr/glibc-compat/lib/gconv
  rm -rf "$pkgdir"/usr/glibc-compat/lib/getconf
  rm -rf "$pkgdir"/usr/glibc-compat/lib/audit
  rm -rf "$pkgdir"/usr/glibc-compat/share
  rm -rf "$pkgdir"/usr/glibc-compat/var
  LINKER_NAME=$(basename "$pkgdir"/usr/glibc-compat/lib/ld-linux*)
  echo "Detected linker name: $LINKER_NAME"
  ln -s /usr/glibc-compat/lib/$LINKER_NAME "$pkgdir"/lib/$LINKER_NAME
  ln -s /usr/glibc-compat/lib/$LINKER_NAME "$pkgdir"/lib64/$LINKER_NAME
  ln -s /usr/glibc-compat/lib/$LINKER_NAME "$pkgdir"/usr/glibc-compat/lib64/$LINKER_NAME
  ln -s /usr/glibc-compat/etc/ld.so.cache "$pkgdir"/etc/ld.so.cache
}

bin() {
  depends="$pkgname libgcc"
  mkdir -p "$subpkgdir"/usr/glibc-compat
  cp -a "$srcdir"/usr/glibc-compat/bin "$subpkgdir"/usr/glibc-compat
  # bash might not be installed on alpine, should override BASH_SHELL on glibc build
  sed -i 's/\/usr\/bin\/bash/\/bin\/sh/g' "$subpkgdir"/usr/glibc-compat/bin/*
  cp -a "$srcdir"/usr/glibc-compat/sbin "$subpkgdir"/usr/glibc-compat
}

i18n() {
  depends="$pkgname-bin"
  arch="noarch"
  mkdir -p "$subpkgdir"/usr/glibc-compat
  cp -a "$srcdir"/usr/glibc-compat/share "$subpkgdir"/usr/glibc-compat
}

sha512sums="<${GLIBC_VERSION}-checksum>  glibc-bin-${GLIBC_VERSION}.tar.gz
478bdd9f7da9e6453cca91ce0bd20eec031e7424e967696eb3947e3f21aa86067aaf614784b89a117279d8a939174498210eaaa2f277d3942d1ca7b4809d4b7e  nsswitch.conf
2912f254f8eceed1f384a1035ad0f42f5506c609ec08c361e2c0093506724a6114732db1c67171c8561f25893c0dd5c0c1d62e8a726712216d9b45973585c9f7  ld.so.conf"
