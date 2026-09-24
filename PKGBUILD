# SPDX-License-Identifier: AGPL-3.0

#    -----------------------------------------------------
#    Copyright © 2024, 2025, 2026  Pellegrino Prevete
#
#    All rights reserved
#    -----------------------------------------------------
#
#    This program is free software: you can redistribute
#    it and/or modify it under the terms of the
#    GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of
#    the License, or (at your option) any later version.
#
#    This program is distributed in the hope that it
#    will be useful, but WITHOUT ANY WARRANTY;
#    without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
#    See the GNU Affero General Public License for
#    more details.
#
#    You should have received a copy of the
#    GNU Affero General Public License
#    along with this program.
#    If not, see <https://www.gnu.org/licenses/>.

# Maintainers:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Contributors:
#   Morten Linderud
#     <foxboron@archlinux.org>
#   Daniel Martí
#     <mvdan@mvdan.cc>
#   Bartłomiej Piotrowski
#     <bpiotrowski@archlinux.org>
#   Alexander F. Rødseth
#     <xyproto@archlinux.org>
#   Pierre Neidhardt
#     <ambrevar@gmail.com>
#   Vesa Kaihlavirta
#     <vegai@iki.fi>
#   Rémy Oudompheng
#     <remy@archlinux.org>
#   Andres Perera
#     <andres87p gmail>
#   Matthew Bauer
#     <mjbauer95@gmail.com>
#   Christian Himpel
#     <chressie@gmail.com>
#   Mike Rosset
#     <mike.rosset@gmail.com>
#   Daniel YC Lin
#     <dlin.tw@gmail.com>
#   John Luebs
#     <jkluebs@gmail.com>

# TODO
# Remember to rebuild go-tools, delve, gopls,
# golangci-lint, staticcheck on new go versions
# pkgctl \
#   build \
#     --offload \
#     --rebuild \
#     --testing \
#     --release \
#       "go-tools" \
#       "delve" \
#       "gopls" \
#       "golangci-lint"
#       "staticcheck"
# pkgctl \
#   db \
#     move \
#       "extra-testing" \
#       "extra" \
#       "go" \
#       "go-tools" \
#       "delve" \
#       "gopls" \
#       "golangci-lint" \
#       "staticcheck"

_os="$(
  uname \
    -o)"
_arch="$(
  uname \
    -m)"
if [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
  _compiler="clang"
  _libcompiler="llvm-libs"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
  _compiler="gcc"
  _libcompiler="libgcc"
elif [[ "${_os}" == "Msys" ]]; then
  _libc="msys2-w32api-runtime"
  _libc_headers="msys2-w32api-headers"
  _compiler="gcc"
  _libcompiler="gcc-libs"
  _sh="sh"
else
  _msg=(
    "Unknown os '${_os}'."
  )
  msg \
    "${_msg[*]}"
  _libc="msys2-w32api-runtime"
  _libc_headers="msys2-w32api-headers"
  _compiler="gcc"
  _libcompiler="gcc-libs"
  _sh="sh"
fi
if [[ ! -v "_bootstrap" ]]; then
  _bootstrap="false"
  if [[ "${_os}" == "Android" ]]; then
    _bootstrap="true"
  fi
fi 
_pkg=go
_pkg_alt="${_pkg}lang"
_go_pkg="${_pkg}"
if [[ "${_bootstrap}" == "true" ]]; then
  _go_pkg="${_pkg_alt}"
fi
if [[ ! -v "_docs" ]]; then
  _docs="true"
fi
if [[ ! -v "_git" ]]; then
  _git="true"
fi
_git="true"
pkgbase="${_pkg}"
pkgname=(
  "${_pkg}"
)
epoch=2
pkgver=1.27.1
pkgrel=53
pkgdesc='Core compiler tools for the Go programming language'
arch=(
  "aarch64"
  "arm"
  "armv6h"
  "armv7l"
  "armv8l"
  "i686"
  "pentium4"
  "powerpc"
  "x86_64"
)
mingw_arch=(
  'mingw64'
  'ucrt64'
  'clang64'
  'clangarm64'
)
url="https://${_pkg}.dev"
license=(
  "BSD-3-Clause"
)
depends=(
  "${_libcompiler}"
)
_filesystem_optdepends=(
  "filesystem:"
    "For resolv-conf stuff."
)
_resolv_conf_optdepends=(
  "resolv-conf:"
    "Termux vanilla calls the"
    "'filesystem' package 'resolv-conf"
    "(because Debian does so probably)."
)
optdepends=(
  "${_filesystem_optdepends[*]}"
  "${_resolv_conf_optdepends[*]}"
)
makedepends=(
  # An apparent self-dependency
  "${_go_pkg}"
)
if [[ "${_os}" == "Android" ]]; then
  # I think this may be available
  # only available on aarch64 and
  # that's why the build fails
  makedepends+=(
    "${_go_pkg}-static"
  )
fi
if [[ "${_git}" == "true" ]]; then
  makedepends+=(
    "git"
  )
fi
if [[ "${_os}" == "Android" ]]; then
  if [[ "${_arch}" == "x86_64" ]]; then
    makedepends+=(
      "gcc"
    )
  fi
fi
replaces=(
  "${_pkg}-pie"
)
provides=(
  "${_pkg}-pie=${pkgver}"
  "${_pkg}lang=${pkgver}"
)
conflicts=(
  "${_pkg}lang"
)
options=(
  "!strip"
)
if [[ "${_os}" != "Android" ]]; then
  options+=(
    "staticlibs"
  )
elif [[ "${_os}" == "Android" ]]; then
  options+=(
    "!lto"
  )
fi
_tarname="${_pkg}"
_archive_format="tar.gz"
_tarfile="${_tarname}.${_archive_format}"
_uri="https://${_pkg}.dev/dl/${_pkg}${pkgver}.src.${_archive_format}"
# Android wants some patches maybe
_src="${_tarfile}::${_uri}"
_sum='4e408abae126d916b6164627193f2c54f0e3ca1312d693b86db45f862ab238b1'
_paths_patchname="fix-hardcoded-etc-resolv-conf.diff"
_paths_sum="f9379880162bb743b26f7588492932bf5b71c68f8937f7daa8e8f80d02922b15"
_pidfd_patchname="remove-pidfd.diff"
_pidfd_sum="a673dc274d6dd5ef0fd48fc21b576ca41de19d49832c43afae28f0e3c37c17a2"
_futex_patchname="remove-futex_time64.diff"
_futex_sum="9360d1a816c7ea3c7532b11e4375c8a969f3256d2efb568764e15653b4757d5f"
_netlink_patchname="fix-android-netlink.diff"
_netlink_sum="aac8f44df4bd06ed1466ba5f0369fac666bb2a60aac8805d8629f2d0cde28c3f"
source=(
  "${_paths_patchname}"
  "${_pidfd_patchname}"
  "${_futex_patchname}"
  "${_netlink_patchname}"
  "${_src}"{"",".asc"}
)
validpgpkeys=(
  # Google Inc. (Linux Packages Signing Authority)
  #   <linux-packages-keymaster@google.com>
  'EB4C1BFD4F042F6DDDCCEC917721F63BD38B4796'
)
sha256sums=(
  "${_paths_sum}"
  "${_pidfd_sum}"
  "${_futex_sum}"
  "${_netlink_sum}"
  "${_sum}"
  'SKIP'
)

_usr_get() {
  local \
    _bin
  _bin="$(
    dirname \
      "$(command \
           -v \
	         "env")")"
  dirname \
    "${_bin}"
}

_android_fix_shebang() {
  local \
    _file="${1}" \
    _msg=() \
    _pattern \
    _patterns=() \
    _repl
  if [[ ! -e "${_file}" ]]; then
    _msg=(
      "File '${_file}' does"
      "not exist."
    )
    echo \
      "${_msg[*]}" \
      1>&2
  fi
  _pattern=(
    "^#!/usr/bin/env"
    "^#! /usr/bin/env"
  )
  _repl="#!/data/data/com.termux/files/usr/bin/env bash"
  for _pattern in "${_patterns[@]}"; do
    sed \
      "s%${_pattern}%${_repl}%g" \
      -i \
      "${_file}"
  done
  _termux_fix_shebang="$(
    command \
      -v \
      "termux-fix-shebang")"
  if [[ "${_termux_fix_shebang}" != "" ]]; then
    termux-fix-shebang \
      "${_file}"
  fi
}

prepare_android() {
  local \
    _file
  cd \
    "${srcdir}/${_pkg}"
  # Fix hardcoded resolv-conf
  for _file \
    in "src/net/conf_android.go" \
       "src/net/dnsclient_android.go"; do
	  if [[ -e "${_file}" ]]; then
      echo \
        "File ${_file} already exists." \
        1>&2
      exit \
        1
	  fi
  done
  cp \
    -T \
    "src/net/conf.go" \
    "src/net/conf_android.go"
  cp \
    -T \
    "src/net/dnsclient_unix.go" \
    "src/net/dnsclient_android.go"
  sed \
    -e \
    "s|@TERMUX_PREFIX@|${TERMUX_PREFIX}|" \
	  "${srcdir}/${_paths_patchname}" |
	  patch \
      --silent \
      -p1
  # Remove pidfd
  sed \
    -e \
    "s|@TERMUX_PREFIX@|${TERMUX_PREFIX}|" \
	  "${srcdir}/${_pidfd_patchname}" |
	  patch \
      --silent \
      -p1
  # remove futex time64
  sed \
    -e \
      "s|@TERMUX_PREFIX@|${TERMUX_PREFIX}|" \
	  "${_futex_patchname}" |
	  patch \
      --silent \
      -p1
  # fix android netlink
  for _file \
    in "src/net/interface_android.go" \
       "src/syscall/netlink_android.go"; do
  	if [ -e "${_file}" ]; then
  		echo \
        "File ${f} already exists." \
        1>&2
      exit \
        1
  	fi
  done
  cp \
    -T \
    "src/syscall/netlink_linux.go" \
    "src/syscall/netlink_android.go"
  cp \
    -T \
    "src/net/interface_linux.go" \
    "src/net/interface_android.go"
  sed \
    -e \
      "s|@TERMUX_PREFIX@|${TERMUX_PREFIX}|" \
  	"${_netlink_patchname}" |
    patch \
      --silent \
      -p1
}

_prepare() {
  if [[ "${_os}" == "Android" ]]; then
    _prepare_android
  fi
}

build() {
  local \
    _arch \
    _cc \
    _cflags=() \
    _cflags=() \
    _cxx_flags=() \
    _go_flags=() \
    _ldflags=() \
    _linker \
    _msg=() \
    _make_bat \
    _make_win \
    _make \
    _usr
  if [[ "${_os}" == "Msys" ]]; then
    export \
      GOOS="windows"
  fi
  _cflags=(
    ${CFLAGS}
  )
  _cflags=(
    # ${CFLAGS}
  )
  _cxx_flags+=(
    ${CXXFLAGS}
  )
  _go_flags+=(
    -trimpath
    -extldflags="-pie"
    -ldflags="-linkmode=external"
    -cxxflags="-fPIC"
    # -mod=vendor
    -modcacherw
  )
  _make_bash="${srcdir}/${_tarname}/src/make.bash"
  _make_bat="${srcdir}/${_tarname}/src/make.bat"
  _usr="$(
    _usr_get)"
  _arch="$(
    uname \
      -m)"
  if [[ "${_arch}" == "arm" || \
        "${_arch}" == "i686" || \
        "${_arch}" == "pentium4" ]]; then
    _msg=(
      "On 32-bit architecture '${_arch}'"
      "do not add '-buildmode=pie'."
    )
    echo \
      "${_msg[*]}"
  else
    _go_flags+=(
      -buildmode=pie
    )
  fi
  if [[ "${_arch}" == "aarch64" ]]; then
    _msg=(
      "Do not specify any architecture"
      "on aarch64."
    )
    echo \
      "${_msg[*]}" \
      1>&2
  elif [[ "${_arch}" == "x86_64" ]]; then
    if [[ "${_os}" == "GNU/Linux" ]]; then
      # make sure we're building for the right x86-64 version
      export \
        GOARCH="amd64" \
        GOAMD64="v1"
    fi
  fi
  export \
    GOROOT_FINAL="${_usr}/lib/go"
    GOROOT_BOOTSTRAP="${_usr}/lib/go"
  # Disable dwarf5 until debugedit catches up
  export \
    GOEXPERIMENT="nodwarf5"
  cd \
    "${_tarname}/src"
  if [[ "${_os}" == "Android" ]]; then
    _ldflags+=(
      # "$LDFLAGS"
      # This option only gets passed
      # to gcc. not even
      # -extldflags="-pie"
      -pie
      -Wl,-pie
    )
    _linker="/system/bin/linker"
    if [[ "${_arch}" == "aarch64" || \
          "${_arch}" == "x86_64" ]]; then
      _linker="${_linker}64"
    fi
    if [[ "${_arch}" == "arm" || \
          "${_arch}" == "i686" ]]; then
      # Reason:
      # ld.ldd: error: relocation R_ARM_ABS32
      # cannot be used against symbol 'runtime.call256';
      # recompile with -fPIC
      _cflags+=(
        -fPIC
      )
      _cxxflags+=(
        -fPIC
      )
    fi
    if [[ "${_arch}" == "x86_64" ]]; then
      _cc="gcc"
    else
      _cc="clang"
    fi
    export \
      CC_FOR_TARGET="clang" \
		  CXX_FOR_TARGET="clang" \
		  CC="${_cc}" \
		  CFLAGS="${_cflags[*]}" \
		  CXXFLAGS="${_cxxflags[*]}"
    export \
      CGO_ENABLED=1 \
      CGO_CPPFLAGS="${CPPFLAGS}" \
      CGO_CFLAGS="${CFLAGS}" \
      CGO_CXXFLAGS="${_cxxflags[*]}" \
      CGO_LDFLAGS="${_ldflags[*]}" \
      G0_LDSO="${_linker}" \
      GOFLAGS="${_go_flags[*]}"
    _android_fix_shebang \
      "${_make_bash}"
  fi
  echo "${_os}"
  if [[ "${_os}" == "Msys" ]]; then
    _make="${_make_bat}"
    _cflags+=(
      -D__USE_MINGW_ANSI_STDIO=1
    )
    echo \
      "Windows build."
    export \
      GO_CFLAGS="${_cflags[*]}" \
      CFLAGS="${_cflags[*]}" \
      GO_BUILD_VERBOSE=1 \
      ROOT_BOOTSTRAP="${MINGW_PREFIX}/lib/go" \
      GOROOT_FINAL="${MINGW_PREFIX}/lib/go"
    cmd \
      //c \
      "${_make}"
  else
    _make="${_make_bash}"
    "${_make}" \
      -v
  fi
}

check() {
  export \
    GO_TEST_TIMEOUT_SCALE=3
  cd \
    "${_pkg}/src"
  # TODO:
  #   Disable LSAN tests as it's crashing
  #   and we don't want to wait for upstream.
  #   See: https://github.com/golang/go/issues/74476
  "./run.bash" \
    --no-rebuild \
    -v \
    -v \
    -v \
    -k \
    -run \
      "!cmd/cgo/internal/testsanitizers"
}

package() {
  local \
    _arch \
    _usr
  _usr="$(
    _usr_get)"
  _arch="$(
    uname \
      -m)"
  cd \
    "${_tarname}"
  install \
    -vdm755 \
    "${pkgdir}/usr/bin" \
    "${pkgdir}/usr/lib/${_pkg}" \
    "${pkgdir}/usr/share/doc/${_pkg}"
  if [[ "${_arch}" == "x86_64" ]]; then
    if [[ "${_os}" == "Android" ]]; then
      install \
        -vdm755 \
        "${pkgdir}/usr/lib/${_pkg}/pkg/android_amd64_"{"dynlink","race"}
    fi
    if [[ "${_os}" == "Msys" ]]; then
      echo \
        "boh"
    else
      install \
        -vdm755 \
        "${pkgdir}/usr/lib/go/pkg/linux_amd64_"{"dynlink","race"}
    fi
  elif [[ "${_arch}" == "aarch64" ]]; then
    if [[ "${_os}" == "Android" ]]; then
      install \
        -vdm755 \
        "${pkgdir}/usr/lib/${_pkg}/pkg/linux_aarch64_"{"dynlink","race"} \
        "${pkgdir}/usr/lib/${_pkg}/pkg/linux_arm64_"{"dynlink","race"} \
        "${pkgdir}/usr/lib/${_pkg}/pkg/linux_arm64_"{"dynlink","race"}
    fi
    if [[ "${_os}" == "Msys" ]]; then
      echo \
        "boh"
    else
      install \
        -vdm755 \
        "${pkgdir}/usr/lib/${_pkg}/pkg/linux_aarch64_"{"dynlink","race"} \
        "${pkgdir}/usr/lib/${_pkg}/pkg/linux_arm64_"{"dynlink","race"} \
        "${pkgdir}/usr/lib/${_pkg}/pkg/linux_arm64_"{"dynlink","race"}
    fi
  elif [[ "${_arch}" == "arm" ]]; then
    if [[ "${_os}" == "Android" ]]; then
      install \
        -vdm755 \
        "${pkgdir}/usr/lib/${_pkg}/pkg/android_arm_"{"dynlink","race"} \
        "${pkgdir}/usr/lib/${_pkg}/pkg/android_armv7l_"{"dynlink","race"} \
        "${pkgdir}/usr/lib/${_pkg}/pkg/android_armv8l_"{"dynlink","race"}
    fi
    if [[ "${_os}" == "Msys" ]]; then
      echo \
        "boh"
    else
      install \
        -vdm755 \
        "${pkgdir}/usr/lib/${_pkg}/pkg/linux_arm_"{"dynlink","race"} \
        "${pkgdir}/usr/lib/${_pkg}/pkg/linux_armv7l_"{"dynlink","race"} \
        "${pkgdir}/usr/lib/${_pkg}/pkg/linux_armv8l_"{"dynlink","race"}
    fi
  fi
  cp \
    -a \
    "bin" \
    "pkg" \
    "src" \
    "lib" \
    "misc" \
    "api" \
    "test" \
    "${pkgdir}/usr/lib/${_pkg}"
  # We can't strip all binaries and libraries,
  # as that also strips some testdata directories and breaks the tests.
  # Just strip the packaged binaries as a compromise.
  strip \
    ${STRIP_BINARIES} \
    "${pkgdir}/usr/lib/${_pkg}"{"/bin/"*,"/pkg/tool/"*"/"*}
  if [[ "${_docs}" == "true" ]]; then
    cp \
      -r \
      "doc/"* \
      "${pkgdir}/usr/share/doc/${_pkg}"
  fi
  ln \
    -sf \
    "${_usr}/lib/${_pkg}/bin/${_pkg}" \
    "${pkgdir}/usr/bin/${_pkg}"
  ln \
    -sf \
    "${_usr}/lib/${_pkg}/bin/${_pkg}fmt" \
    "${pkgdir}/usr/bin/${_pkg}fmt"
  ln \
    -sf \
    "${_usr}/share/doc/${_pkg}" \
    "${pkgdir}/usr/lib/${_pkg}/doc"
  install \
    -vDm644 \
    "VERSION" \
    "${pkgdir}/usr/lib/${_pkg}/VERSION"
  rm \
    -rf \
    "${pkgdir}/usr/lib/${_pkg}/pkg/bootstrap"
  # TODO: Figure out if really needed
  rm \
    -rf \
    "${pkgdir}/usr/lib/${_pkg}/pkg/obj/${_pkg}-build"
  # https://github.com/golang/go/issues/57179
  install \
    -vDm644 \
    "${_pkg}.env" \
    "${pkgdir}/usr/lib/${_pkg}/${_pkg}.env"
  install \
    -vDm644 \
    "LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  if [[ "${_os}" == "Msys" ]]; then
    echo \
      "export GOROOT=${_usr}/lib/${_pkg}" > \
      "${pkgdir}${_usr}/etc/profile.d/${_pkg}.sh"
    cp \
      "${pkgdir}${_usr}/etc/profile.d/${_pkg}."{"sh","zsh"}
  fi
}

# vim: ts=2 sw=2 et
