# Maintainer: Andrea Repetto <darkraziel at libero dot it>

pkgname=upsmon
pkgver=5.3.0
pkgrel=1
pkgdesc="Powershield 3 UPS Monitor for Riello UPS series"
arch=('i686' 'x86_64')
url="http://www.riello-ups.com/?en/downloads/1/powershield3-free"
license=('custom')
depends=('sh' 'bash')
optdepends=('java-runtime: to execute the graphical utilities')
source=(upsmon.d upsmon.service ups-run-as-root
        xupsetup.desktop xupsview.desktop xwizsetup.desktop)
md5sums=('e27cf56a3019669d198759ce464d3d20'
         'c700c3dd4955d5a9ef1b2b02e1348ed0'
         '4948b07103213041b911bf8571706911'
         'a9db7ed077c02d14f3b2bf08a053ff76'
         '55d6adbd80341faf59ff7ad0eb5765dd'
         '7b0aeda87f3c5b02d6358be2802b5c70')
if [ "$CARCH" = "i686" ]; then
	_arch='intel'
	_pkg="$pkgname-$pkgver-linux-2.6-$_arch"
	source+=("http://www.riello-ups.com/areaftp/Linux-Debian-i386/$_pkg.deb")
	md5sums+=('ae2b69a58d44072cbea24b79c47b7e90')
elif [ "$CARCH" = "x86_64" ]; then
	_arch='x86_64'
	_pkg="$pkgname-$pkgver-linux-2.6-$_arch"
	source+=("http://www.riello-ups.com/areaftp/Linux-Debian-x86_64/$_pkg.deb")
	md5sums+=('95fa1398f940da9ede4ded4b3d00c042')

fi
install=upsmon.install
backup=('opt/upsmon/upsmon.ini')

build() {
	
  cd $srcdir
  
  #Unpack the deb package
  ar xf $_pkg.deb
  tar -xf data.tar.gz
  
  
  # Install license
  install -D -m644 $srcdir/opt/$pkgname/License.txt \
  "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  
  # Copy files in /etc
  install -d $pkgdir/etc/rc.d/
  install -D -m755 $srcdir/upsmon.d $pkgdir/etc/rc.d/upsmon
  
  install -d $pkgdir/etc/systemd/system  
  install -D -m755 $srcdir/upsmon.service $pkgdir/etc/systemd/system/
  
  # Copy files in /opt
  install -d $pkgdir/opt/$pkgname
  cp -r $srcdir/opt/$pkgname/* $pkgdir/opt/$pkgname
  
  # Replace init.d with rc.d in the start/stop/restart scripts
  cd $pkgdir/opt/$pkgname
  echo upsstart upsstop upsrestart | xargs sed -i 's/init.d/rc.d/'
   
  # Copy files in /usr/bin
  install -D -m755 $srcdir/ups-run-as-root $pkgdir/usr/bin/ups-run-as-root
   
  # Create scripts for running applications
  install -d $pkgdir/usr/bin
  
  local _apps=(upsetup upsview xupsetup xupsview xwizsetup)
  for _app in ${_apps[@]}; do
    cat > $pkgdir/usr/bin/$_app <<EOF
#!/bin/bash

cd "/opt/upsmon"
./$_app
EOF
    chmod 755 $pkgdir/usr/bin/$_app
  done
    
  # Fix permissions for directories
  chmod a+x $pkgdir/opt/upsmon/images
  chmod a+x $pkgdir/opt/upsmon/html
  
  # Add desktop entries
  install -d $pkgdir/usr/share/applications
  install -D -m644 $srcdir/xupsetup.desktop $pkgdir/usr/share/applications/
  install -D -m644 $srcdir/xupsview.desktop $pkgdir/usr/share/applications/
  install -D -m644 $srcdir/xwizsetup.desktop $pkgdir/usr/share/applications/
}

# vim:set ts=2 sw=2 et:
