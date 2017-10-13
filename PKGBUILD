# Maintainer: wszqkzqk <wszqkzqk@gmail.com>
# Maintainer: ssfdust <ssfdust@gmail.com>

pkgname=deepin-tim-for-arch
pkgver=1.1.5
deepintimver=1.0.4deepin4
pkgrel=0
pkgdesc="Latest Tencent TIM (com.qq.office) on Deepin Wine For Archlinux"
arch=("x86_64")
url="http://tim.qq.com/"
license=('custom')
depends=('p7zip' 'wine' 'xorg-xwininfo' 'xdotool' 'wqy-microhei' 'adobe-source-han-sans-cn-fonts')
conflicts=('wine-tim' 'deepin.com.qq.office')
install="deepin-tim-for-arch.install"
_mirror="https://mirrors.ustc.edu.cn/deepin"
source=("$_mirror/pool/non-free/d/deepin.com.qq.office/deepin.com.qq.office_${deepintimver}_i386.deb"
  "http://dldir1.qq.com/qqfile/qq/TIM${pkgver}/21175/TIM${pkgver}.exe"
  "run.sh"
  "reg_files.tar.bz2"
  "update.policy")
md5sums=('24de53e74f6917dad0693b57e1e6ba4b'
  '4d63de9d589c2d60bb36107849fc87e2'
  '458c0f3c66cf2dbd653738fc82937aed'
  'ebde755e3bd213550f5ccc69d3192060'
  'a66646b473a3fbad243ac1afd64da07a')

build() {
  msg "Extracting DPKG package ..."
  mkdir -p "${srcdir}/dpkgdir"
  tar -xvf data.tar.xz -C "${srcdir}/dpkgdir"
  msg "Extracting Deepin Wine TIM archive ..."
  7z x -aoa "${srcdir}/dpkgdir/opt/deepinwine/apps/Deepin-TIM/files.7z" -o"${srcdir}/deepintimdir"
  msg "Removing original outdated TIM directory ..."
  rm -r "${srcdir}/deepintimdir/drive_c/Program Files/Tencent/TIM"
  msg "Adding config files and fonts"
  tar -jxvf reg_files.tar.bz2 -C "${srcdir}/"
  cp userdef.reg "${srcdir}/deepintimdir/userdef.reg"
  cp system.reg "${srcdir}/deepintimdir/system.reg"
  cp update.policy "${srcdir}/deepintimdir/update.policy"
  cp user.reg "${srcdir}/deepintimdir/user.reg"
  cp "/usr/share/fonts/wenquanyi/wqy-microhei/wqy-microhei.ttc" "${srcdir}/deepintimdir/drive_c/windows/Fonts/wqy-microhei.ttc"
  cp "/usr/share/fonts/adobe-source-han-sans/SourceHanSansCN-Medium.otf" "${srcdir}/deepintimdir/drive_c/windows/Fonts/SourceHanSansCN-Medium.otf"
  msg "Repackaging app archive ..."
  7z a -t7z -r "${srcdir}/files.7z" "${srcdir}/deepintimdir/*"
}

package() {
  msg "Preparing icons ..."
  mkdir -p "${pkgdir}/usr/share"
  mv ${srcdir}/dpkgdir/usr/local/share/* "${pkgdir}/usr/share/"
  msg "Copying TIM to /opt/deepinwine/apps/Deepin-TIM ..."
  mkdir -p "${pkgdir}/opt/deepinwine/apps/Deepin-TIM"
  install -m644 "${srcdir}/files.7z" "${pkgdir}/opt/deepinwine/apps/Deepin-TIM/"
  install -m755 "${srcdir}/run.sh" "${pkgdir}/opt/deepinwine/apps/Deepin-TIM/"
  install -m644 "${srcdir}/TIM$pkgver.exe" "${pkgdir}/opt/deepinwine/apps/Deepin-TIM/"
}
