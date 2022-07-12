PLAN

- qt5webengine
- qt5webview
- qt5virtualkeyboard

- https://github.com/nerves-web-kiosk/
- https://github.com/nerves-project/
- https://github.com/samuelventura/nerves_system_rpi4

git clone git@github.com:samuelventura/nerves_system_rpi4.git
cd nerves_system_rpi4
rm -fr deps/ .nerves/ _build/
mix archive.install hex nerves_bootstrap
mix local.nerves
mix deps.get
mix nerves.system.shell
make menuconfig
make savedefconfig
make -j32
make -j16
exit
mix nerves.artifact
mv *.tar.gz ~/.nerves/artifacts/

mix nerves.new example #no deps
cd example
rm -fr deps/ .nerves/ _build/
MIX_TARGET=rpi4 mix deps.unlock --all
#update app and host names to rpi4hmi
#update nerves_system_rpi4 path ../
MIX_TARGET=rpi4 mix deps.get
MIX_TARGET=rpi4 mix firmware
MIX_TARGET=rpi4 mix firmware.image image.img
MIX_TARGET=rpi4 mix burn
MIX_TARGET=rpi4 mix burn -d image.img
