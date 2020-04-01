# Maintainer: CountStarlight <countstarlight@gmail.com>

pkgname=deepin-wine-tim
pkgver=3.0.0.21315
deepintimver=2.0.0deepin4
pkgrel=1
pkgdesc="Tencent TIM (com.qq.office) on Deepin Wine For Archlinux"
arch=("x86_64")
url="http://tim.qq.com/"
license=('custom')
depends=('p7zip' 'wine' 'wine-mono' 'wine_gecko' 'xorg-xwininfo' 'wqy-microhei' 'lib32-alsa-lib' 'lib32-alsa-plugins' 'lib32-libpulse' 'lib32-openal' 'lib32-mpg123' 'lib32-gnutls')
conflicts=('wine-tim' 'deepin.com.qq.office' 'deepin-tim-for-arch')
install="deepin-wine-tim.install"
_mirror="https://mirrors.ustc.edu.cn/deepin"
source=("$_mirror/pool/non-free/d/deepin.com.qq.office/deepin.com.qq.office_${deepintimver}_i386.deb"
  "https://dldir1.qq.com/qqfile/qq/TIM3.0.0/TIM${pkgver}.exe"
  "run.sh"
  "reg.patch")
md5sums=('d5c37cb4f960e13111ce24dbc0dd2d58'
  '05ccc6f90f26170c83f00d28628c1e2b'
  '812b2e77ab9b559278915eeb803a2d9e'
  '79efbcfa58f4f3d539f09ed5951a0899')

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
  msg "Adding font file ..."
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
  install -m755 "${srcdir}/run.sh" "${pkgdir}/opt/deepinwine/apps/Deepin-TIM/"
  install -m644 "${srcdir}/TIM$pkgver.exe" "${pkgdir}/opt/deepinwine/apps/Deepin-TIM/"
  msg "Printing help info ..."
  echo -e "\033[0;34m=========================提示/INFO==============================="
  echo -e "\033[0;34m* 报告问题(Report issue):"
  echo -e "\033[0;34m  https://github.com/countstarlight/deepin-wine-tim-arch/issues"
  echo -e "\033[0;34m* 切换到 'deepin-wine'(Switch to 'deepin-wine'):"
  echo -e "\033[0;34m  https://github.com/countstarlight/deepin-wine-tim-arch"
  echo -e "\033[0;34m* 安装包下载(Installation package download):"
  echo -e "\033[0;34m  https://github.com/countstarlight/deepin-wine-tim-arch/releases"
  echo -e "\033[0;34m================================================================="
}
