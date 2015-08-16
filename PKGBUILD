# Maintainer: Tom Kuther <gimpel@sonnenkinder.org>

pkgname=kde-workspace-appmenu-stable
pkgver=20120504
pkgrel=1
pkgdesc="KDE Base Workspace with KWin appmenu support"
arch=('i686' 'x86_64')
url='https://gitorious.org/kde-workspace-appmenu/repositories'
license=('GPL' 'LGPL' 'FDL')
groups=('kde')
# note on libxdamage:
# 	not detected by namcap because libgl depends on it
#	but nvidia providing libgl does not depend on libxdamage
depends=('kdepim-runtime' 'lm_sensors' 'libraw1394' 'libqalculate'
         'qimageblitz' 'polkit-kde' 'consolekit' 'xorg-xprop' 'libxdamage'
         'libxklavier' 'xorg-xsetroot' 'libxcomposite' 'libxinerama'
         'xorg-xrdb' 'libgles' 'libegl' 'kactivities' 'libxres' 'libxtst'
	 'appmenu-qt' 'libdbusmenu-qt' 'kded-appmenu-git')
makedepends=('cmake' 'automoc4' 'boost' 'kdebindings-python' 'networkmanager' 'kded-appmenu-git')
optdepends=('kde-wallpapers: wallpapers for KDE Plasma Workspaces')
replaces=('kdmtheme' 'kde-common' 'guidance-power-manager' 'policykit-kde'
          'kdebase-kinfocenter'
	  'oxygen-appmenu' 'kdebase-workspace-appmenu')
conflicts=('kde-common' 'guidance-power-manager' 'policykit-kde'
           'kdebase-kinfocenter'
	   'oxygen-appmenu' 'kdebase-workspace')
provides=('kdebase-workspace=4.8.1')
install="${pkgname/-appmenu/}.install"
backup=('usr/share/config/kdm/kdmrc'
        'etc/pam.d/kde'
        'etc/pam.d/kde-np'
        'etc/pam.d/kscreensaver')
options=('emptydirs')
source=('kdm' 'kde.pam' 'kde-np.pam' 'kscreensaver.pam'
        'fixpath.patch' 'terminate-server.patch' 'kdm-xinitrd.patch')
sha1sums=('5db3a245201bd4a50e65aa2ef583cf5490e4f646'
          'e4198a443703f749b58a4fec88362fced1b45789'
          '264e395c2c44cdda5a242c6dee649afab44cd6e5'
          '106635aa1aae51d6f0668b1853f6c49a4fe9d3d8'
          'd7b5883f7e65c6839b1f65f94d58026673dd0226'
          'ac7bc292c865bc1ab8c02e6341aa7aeaf1a3eeee'
          'd509dac592bd8b310df27991b208c95b6d907514')
_gitroot="git://gitorious.org/kde-workspace-appmenu/kde-workspace-appmenu-stable.git"
_gitname=${pkgname}

build() {
	cd ${srcdir}

	 if [[ -d $srcdir/$_gitname ]] ; then
	    cd $_gitname
	    git pull origin
	    msg "The local files are updated."
	  else
	    git clone $_gitroot
	  fi
  	msg "Starting make..."
	
	cd "${srcdir}"/${_gitname}
	patch -p1 -i "${srcdir}"/kdm-xinitrd.patch
	patch -p0 -i "${srcdir}"/fixpath.patch
	patch -p0 -i "${srcdir}"/terminate-server.patch

	cd "${srcdir}"
	rm -fr build
	mkdir build
	cd build
	cmake ../${pkgname} \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_SKIP_RPATH=ON \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DWITH_Xmms=OFF \
        -DWITH_Googlegadgets=OFF \
        -DWITH_libgps=OFF \
        -DWITH_OpenGLES=ON \
        -DKWIN_BUILD_WITH_OPENGLES=ON \
        -DPYTHON_EXECUTABLE=/usr/bin/python2
	make
}

package() {
	cd "${srcdir}"/build
	make DESTDIR="${pkgdir}" install

	install -D -m755 "${srcdir}"/kdm "${pkgdir}"/etc/rc.d/kdm
	install -D -m644 "${srcdir}"/kde.pam "${pkgdir}"/etc/pam.d/kde
	install -D -m644 "${srcdir}"/kde-np.pam "${pkgdir}"/etc/pam.d/kde-np
	install -D -m644 "${srcdir}"/kscreensaver.pam "${pkgdir}"/etc/pam.d/kscreensaver
	install -d -m755 "${pkgdir}"/usr/share/xsessions/
	ln -sf /usr/share/apps/kdm/sessions/kde-plasma{,-safe}.desktop "${pkgdir}"/usr/share/xsessions/
	install -d -m755 "${pkgdir}"/etc/kde/{env,shutdown}

	install -d -g 135 -o 135 "${pkgdir}"/var/lib/kdm
}
