# Maintainer: Sven-Hendrik Haase <svenstaro@archlinux.org>
# Maintainer: Thomas Baechler <thomas@archlinux.org>
# Contributor: James Rayner <iphitus@gmail.com>
# Contributor: Vasiliy Stelmachenok <ventureo@yandex.ru>

pkgbase=nvidia-merged-utils
pkgname=('nvidia-merged-utils' 'opencl-nvidia-merged' 'nvidia-merged-dkms' 'lib32-nvidia-merged-utils' 'lib32-opencl-nvidia-merged')
pkgver=550.90.07
_hostver=550.90.05
_gridver=550.90.07
pkgrel=8
arch=('x86_64')
url="http://www.nvidia.com/"
license=('custom')
options=('!strip')
makedepends=('patchelf' 'git')
_pkg="NVIDIA-Linux-x86_64-${pkgver}"
_driverpack="NVIDIA-GRID-Linux-KVM-${_hostver}-${_gridver}-552.74"
_vgpukvmdriver="NVIDIA-Linux-x86_64-${_hostver}-vgpu-kvm"
_gridversion=17.3
_mergeddriver="NVIDIA-Linux-x86_64-$pkgver-merged-vgpu-kvm-patched"
source=('nvidia-drm-outputclass.conf'
        'nvidia-utils.sysusers'
        'nvidia.rules'
        'systemd-homed-override.conf'
        'systemd-suspend-override.conf'
        "https://us.download.nvidia.com/XFree86/Linux-x86_64/${pkgver}/${_pkg}.run"
        "make-modeset-fbdev-default.patch"
        "6.11-fbdev.patch"
        'kernel-6.12.patch'
        "nvidia-drm-Set-FOP_UNSIGNED_OFFSET-for-nv_drm_fops.f.patch"
        'remove_no_llseek.patch'
        'kernel-6.13-vfio.patch'
        'nvidia-470xx-fix-linux-6.13.patch'
        'nvidia-grid.conf'
        "NVIDIA-GRID-Linux-KVM-550.90.05-550.90.07-552.74.zip"
        "git+https://github.com/VGPU-Community-Drivers/vGPU-Unlock-patcher.git#branch=${pkgver%.*}"
        'git+https://github.com/DualCoder/vgpu_unlock.git')
sha512sums=('de7116c09f282a27920a1382df84aa86f559e537664bb30689605177ce37dc5067748acf9afd66a3269a6e323461356592fdfc624c86523bf105ff8fe47d3770'
            '4b3ad73f5076ba90fe0b3a2e712ac9cde76f469cd8070280f960c3ce7dc502d1927f525ae18d008075c8f08ea432f7be0a6c3a7a6b49c361126dcf42f97ec499'
            'f8f071f5a46c1a5ce5188e104b017808d752e61c0c20de1466feb5d693c0b55a5586314411e78cc2ab9c0e16e2c67afdd358da94c0c75df1f8233f54c280762c'
            'a0183adce78e40853edf7e6b73867e7a8ea5dabac8e8164e42781f64d5232fbe869f850ab0697c3718ebced5cde760d0e807c05da50a982071dfe1157c31d6b8'
            '55def6319f6abb1a4ccd28a89cd60f1933d155c10ba775b8dfa60a2dc5696b4b472c14b252dc0891f956e70264be87c3d5d4271e929a4fc4b1a68a6902814cee'
            'b8c2cdc918ec74b44517fc181f9eb08ea44d0d9a53f221c0aa243e34872203721a9a7fb27628d35e3028a6aa68917abd2962cc13d5d4b09e92866e14678567a4'
            '73a3734aa0dd4df3cfba9dd7153f9b82981c4a4e86df0c804fb966280c02af8c39ad649bfa3d4119b82709974a40eaab67d357c586b2414c66113929a47628e9'
            '7dc54cf55b7c2014d0ddb443e42ef11e8a50fad9c42a7c0b71c60037e7a9009608095aff6fab5e9617369b5ac27163220c537b68c0583d58cd58931a34da3fa8'
            '9be4f085277f551a4619309644a5ab50d9c302b565c7071e6d91475b4fdb13c90470af67f80a3fde840e4ebb9dfbca10f97e723731b8dcbdad08119ae61d152a'
            '65ee42612a775699a6a4d842e7a42de0adff360c64ab3917aaf3ade1c9034851d2a245e0659e5016fb7c87fc4b97aa777a17c3dd7dc9a8856055bf7c0d641e15'
            '523705553b96b1e7ce4ebce8a00c1ebf174cf461f35ba2bd91a66eb3ab11d1d41457c2164ad98a08ef980baa34c3a8c9bc76ffe6020322ca700e074bcc822afb'
            'a06d880990df6f74ed47190fe08e4310e6acd50247856318e4ecdad9cfa0f3fc73ffad523fdd314c43438cf93690d1d796a0c4c857f0d782224b6a9dd054ee87'
            'c577d422799580e6a7b12670439dd7d68f9474ae17e96355144b7037d9a36228f22e53682f317ec7ca0f6a83d2520ede376350d02a2c072d0d195768f4115cba'
            '5e1a6b9243d825e6e6fbe152f557a398b17f7b774e485599b0de1570d26147df9cf8226898aa0341e5b23d7fdbc9bae495ddfe775ce56e87966438b6ae069351'
            '19720058cea769fa0db8e97914552d033f3b8a612caea5266fa96ad147fbfd77d83ad96b8c205380743141cab3019a5fd59aab25ae1123cef8ffd4a723ed1be7'
            'SKIP'
            'SKIP')


create_links() {
    # create soname links
    find "$pkgdir" -type f -name '*.so*' ! -path '*xorg/*' -print0 | while read -d $'\0' _lib; do
        _soname=$(dirname "${_lib}")/$(readelf -d "${_lib}" | grep -Po 'SONAME.*: \[\K[^]]*' || true)
        _base=$(echo ${_soname} | sed -r 's/(.*)\.so.*/\1.so/')
        [[ -e "${_soname}" ]] || ln -s $(basename "${_lib}") "${_soname}"
        [[ -e "${_base}" ]] || ln -s $(basename "${_soname}") "${_base}"
    done
}

prepare() {
    mv ${_pkg}.run vGPU-Unlock-patcher/
    mv Host_Drivers/${_vgpukvmdriver}.run vGPU-Unlock-patcher/

    cd vGPU-Unlock-patcher
    git submodule init
    git config submodule.unlock.url "$srcdir/vgpu_unlock"
    git -c protocol.file.allow=always submodule update

    # add 2070 to supported gpus
    sed -i '717i \ \ \ \ vcfgclone ${TARGET}/vgpuConfig.xml 0x1E30 0x12BA 0x1F02 0x0000\t# RTX 2070' patch.sh

    ./patch.sh --remap-p2v general-merge
    cd "${_mergeddriver}"
    bsdtar -xf nvidia-persistenced-init.tar.bz2

    # Enable modeset and fbdev as default
    # This avoids various issue, when Simplefb is used
    # https://gitlab.archlinux.org/archlinux/packaging/packages/nvidia-utils/-/issues/14
    # https://github.com/rpmfusion/nvidia-kmod/blob/master/make_modeset_default.patch
    patch -Np1 < "$srcdir"/make-modeset-fbdev-default.patch

    # Add fix for fbdev "phantom" monitor with 6.11
    # https://gitlab.archlinux.org/archlinux/packaging/packages/linux/-/issues/80
    patch -Np1 < "$srcdir"/6.11-fbdev.patch

    patch -Np1 < "$srcdir"/kernel-6.12.patch -d kernel

    patch -Np1 < "$srcdir"/remove_no_llseek.patch

    # Patch by NVIDIA to fix the 6.12 Kernel opening the display
    # https://github.com/NVIDIA/open-gpu-kernel-modules/issues/712
    patch -Np1 < "$srcdir"/nvidia-drm-Set-FOP_UNSIGNED_OFFSET-for-nv_drm_fops.f.patch -d kernel

    patch -Np1 < "$srcdir"/kernel-6.13-vfio.patch
    patch -Np1 < "$srcdir"/nvidia-470xx-fix-linux-6.13.patch -d kernel

    cd kernel

    sed -i "s/__VERSION_STRING/${pkgver}/" dkms.conf
    sed -i 's/__JOBS/`nproc`/' dkms.conf
    sed -i 's/__DKMS_MODULES//' dkms.conf
    sed -i '$iBUILT_MODULE_NAME[0]="nvidia"\
DEST_MODULE_LOCATION[0]="/kernel/drivers/video"\
BUILT_MODULE_NAME[1]="nvidia-uvm"\
DEST_MODULE_LOCATION[1]="/kernel/drivers/video"\
BUILT_MODULE_NAME[2]="nvidia-modeset"\
DEST_MODULE_LOCATION[2]="/kernel/drivers/video"\
BUILT_MODULE_NAME[3]="nvidia-drm"\
DEST_MODULE_LOCATION[3]="/kernel/drivers/video"\
BUILT_MODULE_NAME[4]="nvidia-peermem"\
DEST_MODULE_LOCATION[4]="/kernel/drivers/video"\
BUILT_MODULE_NAME[5]="nvidia-vgpu-vfio"\
DEST_MODULE_LOCATION[5]="/kernel/drivers/video"' dkms.conf

    # Gift for linux-rt guys
    sed -i 's/NV_EXCLUDE_BUILD_MODULES/IGNORE_PREEMPT_RT_PRESENCE=1 NV_EXCLUDE_BUILD_MODULES/' dkms.conf
}

package_opencl-nvidia-merged() {
    pkgdesc="OpenCL implemention for NVIDIA"
    depends=('zlib')
    optdepends=('opencl-headers: headers necessary for OpenCL development')
    conflicts=('opencl-nvidia')
    provides=('opencl-nvidia' 'opencl-driver')
    cd "vGPU-Unlock-patcher/${_mergeddriver}"

    # OpenCL
    install -Dm644 nvidia.icd "${pkgdir}/etc/OpenCL/vendors/nvidia.icd"
    install -Dm755 "libnvidia-opencl.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-opencl.so.${pkgver}"

    create_links

    mkdir -p "${pkgdir}/usr/share/licenses"
    ln -s nvidia-utils "${pkgdir}/usr/share/licenses/opencl-nvidia"
}

package_nvidia-merged-dkms() {
    pkgdesc="NVIDIA drivers - module sources"
    depends=('dkms' "nvidia-utils=$pkgver" 'libglvnd')
    provides=("nvidia-dkms=${pkgver}" 'NVIDIA-MODULE' 'nvidia')
    conflicts=('nvidia-dkms' 'NVIDIA-MODULE' 'nvidia')

    cd "vGPU-Unlock-patcher/${_mergeddriver}"

    install -dm 755 "${pkgdir}"/usr/src
    cp -dr --no-preserve='ownership' kernel "${pkgdir}/usr/src/nvidia-${pkgver}"

    install -Dt "${pkgdir}/usr/share/licenses/${pkgname}" -m644 LICENSE
}

package_nvidia-merged-utils() {
    pkgdesc="NVIDIA drivers utilities"
    depends=('libglvnd' 'egl-wayland' 'egl-gbm')
    optdepends=('nvidia-settings: configuration tool'
                'xorg-server: Xorg support'
                'xorg-server-devel: nvidia-xconfig'
                'opencl-nvidia: OpenCL support')
    conflicts=('nvidia-utils' 'nvidia-libgl')
    provides=("nvidia-utils=${pkgver}" 'vulkan-driver' 'opengl-driver' 'nvidia-libgl')
    replaces=('nvidia-libgl')
    install="${pkgname}.install"

    cd "vGPU-Unlock-patcher/${_mergeddriver}"

    # Check http://us.download.nvidia.com/XFree86/Linux-x86_64/${pkgver}/README/installedcomponents.html
    # for hints on what needs to be installed where.

    # X driver
    install -Dm755 nvidia_drv.so "${pkgdir}/usr/lib/xorg/modules/drivers/nvidia_drv.so"

    # Wayland/GBM
    mkdir -p "${pkgdir}/usr/lib/gbm"
    ln -sr "${pkgdir}/usr/lib/libnvidia-allocator.so.${pkgver}" "${pkgdir}/usr/lib/gbm/nvidia-drm_gbm.so"

    # firmware
    install -Dm644 -t "${pkgdir}/usr/lib/firmware/nvidia/${pkgver}/" firmware/*.bin

    # GLX extension module for X
    install -Dm755 "libglxserver_nvidia.so.${pkgver}" "${pkgdir}/usr/lib/nvidia/xorg/libglxserver_nvidia.so.${pkgver}"
    # Ensure that X finds glx
    ln -s "libglxserver_nvidia.so.${pkgver}" "${pkgdir}/usr/lib/nvidia/xorg/libglxserver_nvidia.so.1"
    ln -s "libglxserver_nvidia.so.${pkgver}" "${pkgdir}/usr/lib/nvidia/xorg/libglxserver_nvidia.so"

    install -Dm755 "libGLX_nvidia.so.${pkgver}" "${pkgdir}/usr/lib/libGLX_nvidia.so.${pkgver}"

    # OpenGL libraries
    install -Dm755 "libEGL_nvidia.so.${pkgver}" "${pkgdir}/usr/lib/libEGL_nvidia.so.${pkgver}"
    install -Dm755 "libGLESv1_CM_nvidia.so.${pkgver}" "${pkgdir}/usr/lib/libGLESv1_CM_nvidia.so.${pkgver}"
    install -Dm755 "libGLESv2_nvidia.so.${pkgver}" "${pkgdir}/usr/lib/libGLESv2_nvidia.so.${pkgver}"
    install -Dm644 "10_nvidia.json" "${pkgdir}/usr/share/glvnd/egl_vendor.d/10_nvidia.json"

    # OpenGL core library
    install -Dm755 "libnvidia-glcore.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-glcore.so.${pkgver}"
    install -Dm755 "libnvidia-eglcore.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-eglcore.so.${pkgver}"
    install -Dm755 "libnvidia-glsi.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-glsi.so.${pkgver}"

    # misc
    install -Dm755 "libnvidia-api.so.1" "${pkgdir}/usr/lib/libnvidia-api.so.1"
    install -Dm755 "libnvidia-fbc.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-fbc.so.${pkgver}"
    install -Dm755 "libnvidia-encode.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-encode.so.${pkgver}"
    install -Dm755 "libnvidia-cfg.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-cfg.so.${pkgver}"
    install -Dm755 "libnvidia-ml.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-ml.so.${pkgver}"
    install -Dm755 "libnvidia-glvkspirv.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-glvkspirv.so.${pkgver}"
    install -Dm755 "libnvidia-allocator.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-allocator.so.${pkgver}"
    install -Dm755 "libnvidia-gpucomp.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-gpucomp.so.${pkgver}"

    # Vulkan ICD
    install -Dm644 "nvidia_icd.json" "${pkgdir}/usr/share/vulkan/icd.d/nvidia_icd.json"
    install -Dm644 "nvidia_layers.json" "${pkgdir}/usr/share/vulkan/implicit_layer.d/nvidia_layers.json"

    # VDPAU
    install -Dm755 "libvdpau_nvidia.so.${pkgver}" "${pkgdir}/usr/lib/vdpau/libvdpau_nvidia.so.${pkgver}"

    # nvidia-tls library
    install -Dm755 "libnvidia-tls.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-tls.so.${pkgver}"

    # CUDA
    install -Dm755 "libcuda.so.${pkgver}" "${pkgdir}/usr/lib/libcuda.so.${pkgver}"
    install -Dm755 "libnvcuvid.so.${pkgver}" "${pkgdir}/usr/lib/libnvcuvid.so.${pkgver}"
    install -Dm755 "libcudadebugger.so.${pkgver}" "${pkgdir}/usr/lib/libcudadebugger.so.${pkgver}"

    # NVVM Compiler library loaded by the CUDA driver to do JIT link-time-optimization
    install -Dm644 "libnvidia-nvvm.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-nvvm.so.${pkgver}"

    # PTX JIT Compiler (Parallel Thread Execution (PTX) is a pseudo-assembly language for CUDA)
    install -Dm755 "libnvidia-ptxjitcompiler.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-ptxjitcompiler.so.${pkgver}"

    # raytracing
    install -Dm755 "nvoptix.bin" "${pkgdir}/usr/share/nvidia/nvoptix.bin"
    install -Dm755 "libnvoptix.so.${pkgver}" "${pkgdir}/usr/lib/libnvoptix.so.${pkgver}"
    install -Dm755 "libnvidia-rtcore.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-rtcore.so.${pkgver}"

    # NGX
    install -Dm755 nvidia-ngx-updater "${pkgdir}/usr/bin/nvidia-ngx-updater"
    install -Dm755 "libnvidia-ngx.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-ngx.so.${pkgver}"
    install -Dm755 _nvngx.dll "${pkgdir}/usr/lib/nvidia/wine/_nvngx.dll"
    install -Dm755 nvngx.dll "${pkgdir}/usr/lib/nvidia/wine/nvngx.dll"

    # Optical flow
    install -Dm755 "libnvidia-opticalflow.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-opticalflow.so.${pkgver}"

    # Cryptography library wrapper
    ls libnvidia-pkcs*
    ls *openssl*
    install -Dm755 "libnvidia-pkcs11.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-pkcs11.so.${pkgver}"
    install -Dm755 "libnvidia-pkcs11-openssl3.so.${pkgver}" "${pkgdir}/usr/lib/libnvidia-pkcs11-openssl3.so.${pkgver}"

    # Debug
    install -Dm755 nvidia-debugdump "${pkgdir}/usr/bin/nvidia-debugdump"

    # nvidia-xconfig
    install -Dm755 nvidia-xconfig "${pkgdir}/usr/bin/nvidia-xconfig"
    install -Dm644 nvidia-xconfig.1.gz "${pkgdir}/usr/share/man/man1/nvidia-xconfig.1.gz"

    # nvidia-bug-report
    install -Dm755 nvidia-bug-report.sh "${pkgdir}/usr/bin/nvidia-bug-report.sh"

    # nvidia-smi
    install -Dm755 nvidia-smi "${pkgdir}/usr/bin/nvidia-smi"
    install -Dm644 nvidia-smi.1.gz "${pkgdir}/usr/share/man/man1/nvidia-smi.1.gz"

    # nvidia-cuda-mps
    install -Dm755 nvidia-cuda-mps-server "${pkgdir}/usr/bin/nvidia-cuda-mps-server"
    install -Dm755 nvidia-cuda-mps-control "${pkgdir}/usr/bin/nvidia-cuda-mps-control"
    install -Dm644 nvidia-cuda-mps-control.1.gz "${pkgdir}/usr/share/man/man1/nvidia-cuda-mps-control.1.gz"

    # nvidia-modprobe
    # This should be removed if nvidia fixed their uvm module!
    install -Dm4755 nvidia-modprobe "${pkgdir}/usr/bin/nvidia-modprobe"
    install -Dm644 nvidia-modprobe.1.gz "${pkgdir}/usr/share/man/man1/nvidia-modprobe.1.gz"

    # nvidia-persistenced
    install -Dm755 nvidia-persistenced "${pkgdir}/usr/bin/nvidia-persistenced"
    install -Dm644 nvidia-persistenced.1.gz "${pkgdir}/usr/share/man/man1/nvidia-persistenced.1.gz"
    install -Dm644 nvidia-persistenced-init/systemd/nvidia-persistenced.service.template "${pkgdir}/usr/lib/systemd/system/nvidia-persistenced.service"
    sed -i 's/__USER__/nvidia-persistenced/' "${pkgdir}/usr/lib/systemd/system/nvidia-persistenced.service"

    # application profiles
    install -Dm644 nvidia-application-profiles-${pkgver}-rc "${pkgdir}/usr/share/nvidia/nvidia-application-profiles-${pkgver}-rc"
    install -Dm644 nvidia-application-profiles-${pkgver}-key-documentation "${pkgdir}/usr/share/nvidia/nvidia-application-profiles-${pkgver}-key-documentation"

    install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/nvidia-utils/LICENSE"
    install -Dm644 README.txt "${pkgdir}/usr/share/doc/nvidia/README"
    install -Dm644 NVIDIA_Changelog "${pkgdir}/usr/share/doc/nvidia/NVIDIA_Changelog"
    cp -r html "${pkgdir}/usr/share/doc/nvidia/"
    ln -s nvidia "${pkgdir}/usr/share/doc/nvidia-utils"

    # new power management support
    install -Dm644 systemd/system/*.service -t "${pkgdir}/usr/lib/systemd/system"
    install -Dm755 systemd/system-sleep/nvidia "${pkgdir}/usr/lib/systemd/system-sleep/nvidia"
    install -Dm755 systemd/nvidia-sleep.sh "${pkgdir}/usr/bin/nvidia-sleep.sh"
    install -Dm755 nvidia-powerd "${pkgdir}/usr/bin/nvidia-powerd"
    install -Dm644 nvidia-dbus.conf "${pkgdir}"/usr/share/dbus-1/system.d/nvidia-dbus.conf
    install -Dm644 "${srcdir}"/systemd-homed-override.conf "${pkgdir}"/usr/lib/systemd/system/systemd-homed.service.d/10-nvidia-no-freeze-session.conf
    install -Dm644 "${srcdir}"/systemd-suspend-override.conf "${pkgdir}"/usr/lib/systemd/system/systemd-suspend.service.d/10-nvidia-no-freeze-session.conf
    install -Dm644 "${srcdir}"/systemd-suspend-override.conf "${pkgdir}"/usr/lib/systemd/system/systemd-suspend-then-hibernate.service.d/10-nvidia-no-freeze-session.conf
    install -Dm644 "${srcdir}"/systemd-suspend-override.conf "${pkgdir}"/usr/lib/systemd/system/systemd-hibernate.service.d/10-nvidia-no-freeze-session.conf
    install -Dm644 "${srcdir}"/systemd-suspend-override.conf "${pkgdir}"/usr/lib/systemd/system/systemd-hybrid-sleep.service.d/10-nvidia-no-freeze-session.conf

    # NVIDIA GRID
    install -Dm644 GRID_LICENSE "${pkgdir}/usr/share/licenses/nvidia-utils/GRID_LICENSE"
    install -Dm644 grid-third-party-licenses.txt "${pkgdir}/usr/share/licenses/nvidia-utils/grid-third-party-licenses.txt"
    install -Dm755 "libnvidia-vgpu.so.${_hostver}" "${pkgdir}/usr/lib/libnvidia-vgpu.so.${_hostver}"
    install -Dm755 "libnvidia-vgxcfg.so.${_hostver}" "${pkgdir}/usr/lib/libnvidia-vgxcfg.so.${_hostver}"
    install -Dm755 "nvidia-vgpud" "${pkgdir}/usr/bin/nvidia-vgpud"
    install -Dm755 "nvidia-vgpu-mgr" "${pkgdir}/usr/bin/nvidia-vgpu-mgr"
    install -Dm755 "sriov-manage" "${pkgdir}/usr/lib/nvidia/sriov-manage"
    install -Dm644 "vgpuConfig.xml" "${pkgdir}/usr/share/nvidia/vgpu/vgpuConfig.xml"
    install -Dm644 init-scripts/systemd/*.service -t "${pkgdir}/usr/lib/systemd/system"
    install -Dm644 "$srcdir/nvidia-grid.conf" "${pkgdir}/usr/share/dbus-1/system.d/nvidia-grid.conf"

    # distro specific files must be installed in /usr/share/X11/xorg.conf.d
    install -Dm644 "${srcdir}/nvidia-drm-outputclass.conf" "${pkgdir}/usr/share/X11/xorg.conf.d/10-nvidia-drm-outputclass.conf"

    install -Dm644 "${srcdir}/nvidia-utils.sysusers" "${pkgdir}/usr/lib/sysusers.d/$pkgname.conf"

    install -Dm644 "${srcdir}/nvidia.rules" "$pkgdir"/usr/lib/udev/rules.d/60-nvidia.rules

    echo "blacklist nouveau" | install -Dm644 /dev/stdin "${pkgdir}/usr/lib/modprobe.d/${pkgname}.conf"
    echo "nvidia-uvm" | install -Dm644 /dev/stdin "${pkgdir}/usr/lib/modules-load.d/${pkgname}.conf"

    create_links
}

package_lib32-opencl-nvidia-merged() {
    pkgdesc="OpenCL implemention for NVIDIA (32-bit)"
    depends=('lib32-zlib' 'lib32-gcc-libs')
    optdepends=('opencl-headers: headers necessary for OpenCL development')
    conflicts=('lib32-opencl-nvidia')
    provides=('lib32-opencl-driver')

    cd "vGPU-Unlock-patcher/${_mergeddriver}/32"

    # OpenCL
    install -D -m755 "libnvidia-opencl.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-opencl.so.${pkgver}"

    create_links

    mkdir -p "${pkgdir}/usr/share/licenses"
    ln -s nvidia-utils "${pkgdir}/usr/share/licenses/lib32-opencl-nvidia"
}

package_lib32-nvidia-merged-utils() {
    pkgdesc="NVIDIA drivers utilities (32-bit)"
    depends=('lib32-zlib' 'lib32-gcc-libs' 'lib32-libglvnd' "nvidia-utils=${pkgver}")
    optdepends=('lib32-opencl-nvidia')
    conflicts=('lib32-nvidia-libgl' 'lib32-nvidia-utils')
    provides=('lib32-vulkan-driver' 'lib32-opengl-driver' 'lib32-nvidia-libgl')
    replaces=('lib32-nvidia-libgl')

    cd "vGPU-Unlock-patcher/${_mergeddriver}/32"

    # Check http://us.download.nvidia.com/XFree86/Linux-x86_64/${pkgver}/README/installedcomponents.html
    # for hints on what needs to be installed where.

    install -D -m755 "libGLX_nvidia.so.${pkgver}" "${pkgdir}/usr/lib32/libGLX_nvidia.so.${pkgver}"

    # OpenGL libraries
    install -D -m755 "libEGL_nvidia.so.${pkgver}" "${pkgdir}/usr/lib32/libEGL_nvidia.so.${pkgver}"
    install -D -m755 "libGLESv1_CM_nvidia.so.${pkgver}" "${pkgdir}/usr/lib32/libGLESv1_CM_nvidia.so.${pkgver}"
    install -D -m755 "libGLESv2_nvidia.so.${pkgver}" "${pkgdir}/usr/lib32/libGLESv2_nvidia.so.${pkgver}"

    # OpenGL core library
    install -D -m755 "libnvidia-glcore.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-glcore.so.${pkgver}"
    install -D -m755 "libnvidia-eglcore.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-eglcore.so.${pkgver}"
    install -D -m755 "libnvidia-glsi.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-glsi.so.${pkgver}"

    # misc
    mkdir -p "${pkgdir}/usr/lib32/gbm" && ln -sr "${pkgdir}/usr/lib32/libnvidia-allocator.so.${pkgver}" "${pkgdir}/usr/lib32/gbm/nvidia-drm_gbm.so"
    install -D -m755 "libnvidia-fbc.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-fbc.so.${pkgver}"
    install -D -m755 "libnvidia-encode.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-encode.so.${pkgver}"
    install -D -m755 "libnvidia-ml.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-ml.so.${pkgver}"
    install -D -m755 "libnvidia-glvkspirv.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-glvkspirv.so.${pkgver}"
    install -D -m755 "libnvidia-allocator.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-allocator.so.${pkgver}"
    install -D -m755 "libnvidia-gpucomp.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-gpucomp.so.${pkgver}"

    # VDPAU
    install -D -m755 "libvdpau_nvidia.so.${pkgver}" "${pkgdir}/usr/lib32/vdpau/libvdpau_nvidia.so.${pkgver}"

    # nvidia-tls library
    install -D -m755 "libnvidia-tls.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-tls.so.${pkgver}"

    # CUDA
    install -D -m755 "libcuda.so.${pkgver}" "${pkgdir}/usr/lib32/libcuda.so.${pkgver}"
    install -D -m755 "libnvcuvid.so.${pkgver}" "${pkgdir}/usr/lib32/libnvcuvid.so.${pkgver}"

    # PTX JIT Compiler (Parallel Thread Execution (PTX) is a pseudo-assembly language for CUDA)
    install -D -m755 "libnvidia-ptxjitcompiler.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-ptxjitcompiler.so.${pkgver}"

    # Optical flow
    install -D -m755 "libnvidia-opticalflow.so.${pkgver}" "${pkgdir}/usr/lib32/libnvidia-opticalflow.so.${pkgver}"

    create_links

    rm -rf "${pkgdir}"/usr/{include,share,bin}
    mkdir -p "${pkgdir}/usr/share/licenses"
    ln -s nvidia-utils "${pkgdir}/usr/share/licenses/${pkgname}"
}
