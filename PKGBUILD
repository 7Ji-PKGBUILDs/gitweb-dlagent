# Maintainer: Mahmut Dikcizgi <boogiepop a~t gmx com>
# Maintainer: 7Ji <pugokushin@gmail.com>

pkgname=gitweb-dlagent
pkgver=0.3
pkgrel=1
pkgdesc='A dlagent targets for efficiency in Archlinux vcs packages'
arch=('any')
url='https://github.com/hbiyik/agrrepo/gitweb-dlagent'
depends=('python' 'git' 'tar' 'pigz')
source=('gitweb-dlagent')
sha256sums=('SKIP')


package() {
  install -D --mode=755 gitweb-dlagent "${pkgdir}/usr/bin/${pkgname}"
}
