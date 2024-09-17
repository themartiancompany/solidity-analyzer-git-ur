# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Maintainer: Truocolo <truocolo@aol.com>
# Contributor: Filipe Bertelli <filipebertelli@tutanota.com>

_offline='false'
_git='false'
_source='ur'
_ns="NomicFoundation"
_pub="@nomicfoundation"
_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "Android" ]]; then
  _source="ur"
fi
_pkg=solidity-analyzer
_pkgbase="${_pkg}"
pkgbase="${_pkgbase}-git"
_pkgname="${_pkgbase}"
pkgname="${pkgbase}"
_pkgdesc=(
  'API library built in Rust,'
  'which exposes a single function,'
  'which takes the contents of a Solidity source file'
  'and returns its imports and version pragmas'
)
pkgdesc="${_pkgdesc[*]}"
pkgver=0.1.2.1.1
pkgrel=1
arch=(
  'any'
)
if [[ "${_source}" == "ur" ]]; then
  _ns="themartiancompany"
fi
_gh="https://github.com"
url="${_gh}/${_ns}/${_pkg}"
_gh_api="https://api.${_gh}/repos/${_ns}/${_pkgname}"
license=(
  'custom'
)
depends=(
  'nodejs'
)
makedepends=(
  'npm'
)
provides=(
  "${_pkgname}=${pkgver}"
)
conflicts=(
  "${_pkgname}"
)
source=(
)
sha256sums=(
)
_url="${url}"
[[ "${_offline}" == "true" ]] && \
  _url="file://${HOME}/${pkgname}"
_branch="main"
[[ "${_git}" == true ]] && \
  makedepends+=(
    git
  ) && \
  source+=(
    "${_pkgname}-${_branch}::git+${_url}#branch=${_branch}"
  )
[[ "${_git}" == false ]] && \
  makedepends+=(
    curl
    jq
  ) && \
  source+=(
    "${_pkgname}.tar.gz::${_url}/archive/refs/heads/${_branch}.tar.gz"
  )
sha256sums+=(
  'SKIP'
)

_nth() {
  local \
    _str="${1}" \
    _n="${2}"
  echo \
    "${_str}" | \
    awk \
      -F '+' \
      '{print $'"${_n}"'}'
}

_jq_pkgver() {
  local \
    _version \
    _rev \
    _commit
  _version="$( \
    curl \
      --silent \
      "${_gh_api}/tags" | \
      jq \
        '.[0].name')"
  _version_commit="$( \
    curl \
      --silent \
      "${_gh_api}/tags" | \
      jq \
        '.[0].commit.sha')"
  _rev="$( \
    curl \
      --silent \
      "${_gh_api}/commits" | \
      jq \
        'map(.sha == '${_version_commit}' ) | index(true)')"
  _commit="$( \
    curl \
      --silent \
      "${_gh_api}/commits" | \
      jq \
        '.[0].sha')"
  printf \
    "%s.r%s.g%s" \
    "${_version}" \
    "${_rev}" \
    "${_commit}"
}

_parse_ver() {
  local \
    _pkgver="${1}" \
    _out="" \
    _ver \
    _rev \
    _commit
  _ver="$( \
    _nth \
      "${_pkgver}" \
      "1")"
  _rev="$( \
    _nth \
      "${_pkgver}" \
      "2")"
  _commit="$( \
    _nth \
      "${_pkgver}" \
      "3")"
  _out=${_ver}
  [[ "${_rev}" != "" ]] && \
    _out+=".r${_rev}"
  [[ "${_commit}" != "" ]] && \
    _out+=".${_commit}"
  echo \
    "${_out}"
}

_git_pkgver() {
  local \
    _pkgver
  _pkgver="$( \
    git \
      describe \
      --tags \
      --long | \
      sed \
        's/-/+/g')"
  _parse_ver \
    "${_pkgver}"
}


package() {
  local \
    _npm_options=() \
    _tgz
  _npm_options=(
    -g
    # --user=root
    --prefix="${pkgdir}/usr"
  )
  cd \
    ${_tarname}
  yarn
  yarn \
    build
  rm \
    -fr \
      "${pkgdir}/usr/etc"
  # Fix npm derp
  find \
    "${pkgdir}/usr" \
    -type d \
    -exec \
      chmod \
        755 \
	'{}' \
	+
}

# vim:set sw=2 sts=-1 et:
