# Maintainer: Xilin Wu <sophon@radxa.com>
# Upstream: https://github.com/qualcomm/camera-service

pkgname=qcom-camera-service
pkgver=1.0
pkgrel=2
pkgdesc="Qualcomm QMMF camera service for QCM6490"
arch=('aarch64')
url="https://github.com/qualcomm/camera-service"
license=('BSD-3-Clause-Clear')
depends=('qcom-camx-kodiak')
makedepends=('git' 'cmake' 'protobuf' 'abseil-cpp')
source=("${pkgname}::git+https://github.com/qualcomm/camera-service.git#commit=47fe2cb12f283c46b865733a9673987bc22ad5e5")
sha256sums=('SKIP')
options=('!strip')
install=qcom-camera-service.install

prepare() {
  cd "$pkgname"
  git reset --hard HEAD
  git clean -fd
  sed -i '/^\[Unit\]/a ConditionPathExistsGlob=/sys/module/camera*/parameters/debug_mdl' \
    qmmf-server/service/cam-server-base.service.in
}

build() {
  cmake -B build -S "$pkgname" \
    -DBUILD_CATEGORY=ALL \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_INSTALL_SYSCONFDIR=/etc \
    -DTARGET_BOARD_PLATFORM=qcm6490 \
    -DCAM_SERVER_GROUP=video \
    -DCMAKE_CXX_FLAGS="-I/usr/include/camx-api/camx/service"
  cmake --build build
}

package() {
  DESTDIR="$pkgdir" cmake --install build

  install -Dm644 "$pkgname/LICENSE.txt" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE.txt"
}
