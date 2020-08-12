# Maintainer: Codist <countstarlight@gmail.com>

pkgname=deepin-wine-tim
pkgver=3.1.0.21789
deepintimver=2.0.0deepin4
pkgrel=1
pkgdesc="Tencent TIM (com.qq.office) on Deepin Wine For Archlinux"
arch=("x86_64")
url="http://tim.qq.com/"
license=('custom')
depends=('p7zip' 'wine' 'wine-mono' 'wine-gecko' 'xorg-xwininfo' 'wqy-microhei' 'lib32-alsa-lib' 'lib32-alsa-plugins' 'lib32-libpulse' 'lib32-openal' 'lib32-mpg123' 'lib32-gnutls')
conflicts=('wine-tim' 'deepin.com.qq.office' 'deepin-tim-for-arch')
install="deepin-wine-tim.install"
_mirror="https://mirrors.ustc.edu.cn/deepin"
source=("$_mirror/pool/non-free/d/deepin.com.qq.office/deepin.com.qq.office_${deepintimver}_i386.deb"
  "https://dldir1.qq.com/qqfile/qq/TIM3.1.0/TIM${pkgver}.exe"
  "run.sh"
  "reg.patch")
md5sums=('d5c37cb4f960e13111ce24dbc0dd2d58'
  '344c9438db69db52c86f3fa575dff143'
  '20625edae7b3d57e9f624fe473d4f7fa'
  '8cdb3c8863cee35a54fe5ba29e770fd3')

build() {
  msg "Extracting DPKG package ..."
  mkdir -p "${srcdir}/dpkgdir"
  tar -xvf data.tar.xz -C "${srcdir}/dpkgdir"
  sed "s/\(Categories.*$\)/\1Network;/" -i "${srcdir}/dpkgdir/usr/share/applications/deepin.com.qq.office.desktop"
  sed "13s/TIM.exe/tim.exe/" -i "${srcdir}/dpkgdir/usr/share/applications/deepin.com.qq.office.desktop"
  msg "Extracting Deepin Wine TIM archive ..."
  7z x -aoa "${srcdir}/dpkgdir/opt/deepinwine/apps/Deepin-TIM/files.7z" -o"${srcdir}/deepintimdir"
  msg "Removing original outdated TIM directory ..."
  rm -r "${srcdir}/deepintimdir/drive_c/Program Files/Tencent/TIM"
  msg "Patching reg files ..."
  patch -p1 -d "${srcdir}/deepintimdir/" < "${srcdir}/reg.patch"
  msg "Creating font file link ..."
  ln -sf "/usr/share/fonts/wenquanyi/wqy-microhei/wqy-microhei.ttc" "${srcdir}/deepintimdir/drive_c/windows/Fonts/wqy-microhei.ttc"
  msg "Repackaging app archive ..."
  7z a -t7z -r "${srcdir}/files.7z" "${srcdir}/deepintimdir/*"
}

package() {
  msg "Preparing icons ..."
  install -d "${pkgdir}/usr/share"
  cp -a ${srcdir}/dpkgdir/usr/share/* "${pkgdir}/usr/share/"
  msg "Copying TIM to /opt/deepinwine/apps/Deepin-TIM ..."
  install -d "${pkgdir}/opt/deepinwine/apps/Deepin-TIM"
  install -m644 "${srcdir}/files.7z" "${pkgdir}/opt/deepinwine/apps/Deepin-TIM/"
  install -m644 "${srcdir}/reg.patch" "${pkgdir}/opt/deepinwine/apps/Deepin-TIM/"
  install -m755 "${srcdir}/run.sh" "${pkgdir}/opt/deepinwine/apps/Deepin-TIM/"
  install -m644 "${srcdir}/TIM$pkgver.exe" "${pkgdir}/opt/deepinwine/apps/Deepin-TIM/"
}
