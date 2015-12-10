# Maintainer: Gordian Edenhofer <gordian.edenhofer[at]yahoo[dot]de>

# Please consider the size of the package, before building on a potential small partition
# like /tmp (bare in mind that most AUR helpers do so). The extracted content of the files
# is about 3 GB in size.

# Prevent compression of the final package since it would take too long (sereausly!)
PKGEXT='.pkg.tar'

pkgname=unity-editor-bin
pkgver=5.2.2f1+20151018
pkgrel=2
pkgdesc="The world's most popular development platform for creating 2D and 3D multiplatform games and interactive experiences."
arch=('x86_64')
license=('custom')
url="https://unity3d.com/"
depends=('desktop-file-utils' 'xdg-utils' 'gcc-multilib' 'libgl' 'glu' 'nss' 'libpng12' 'libxtst' 'libpqxx' 'monodevelop')
optdepends=('ffmpeg: for WebGL exporting'
            'nodejs: for WebGL exporting'
            'java-runtime: for WebGL exporting'
            'gzip: for WebGL exporting'
            'java-environment: for Android and Tizen exporting'
            'android-sdk: for Android Remote'
            'android-udev: for Android Remote')
provides=('unity-editor')
conflicts=('unity-editor')
options=(!strip)
install=${pkgname}.install
source=("http://files.unity3d.com/levi/unity-editor-${pkgver}_amd64.deb")
md5sums=('5ef887483b737b0d20ca69ff4d368103')

package() {
    bsdtar xf data.tar.gz

    # Moving stuff in place
    mv usr "${pkgdir}"
    mv opt "${pkgdir}"

    # Setting permissions on chrome-sandbox - necessary to run the program
    chown root:root "${pkgdir}"/opt/Unity/Editor/chrome-sandbox
    chmod 4755 "${pkgdir}"/opt/Unity/Editor/chrome-sandbox

    # Linking executables (symlinking does not work!)
    mkdir -p "${pkgdir}"/usr/bin
    #ln -s /opt/Unity/Editor/Unity "${pkgdir}"/usr/bin/unity-editor
    #ln -s /opt/Unity/MonoDevelop/bin/monodevelop "${pkgdir}"/usr/bin/unity-monodevelop
    echo -e "#!/bin/sh\nexec /opt/Unity/Editor/Unity \"$@\"" > "${pkgdir}"/usr/bin/unity-editor
    echo -e "#!/bin/sh\n\n# This prevents the editor from crashing when opening projects on some systems\nunset GTK_IM_MODULE\n\nexec /opt/Unity/MonoDevelop/bin/monodevelop \"$@\"" > "${pkgdir}"/usr/bin/unity-monodevelop
    chmod 755 "${pkgdir}"/usr/bin/unity-editor "${pkgdir}"/usr/bin/unity-monodevelop

    # Refering to the online license
    mkdir -p "${pkgdir}"/usr/share/licenses/${pkgname}
    echo "Please refer to https://unity3d.com/legal/eula for detailed license information." > "${pkgdir}"/usr/share/licenses/${pkgname}/LICENSE
}
