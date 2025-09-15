# ZigBee: Компиляция исходного кода

Перед компиляция исходного кода _HOMEd ZigBee_ необходимо подготовить окружение, включающее в себя нужный компилятор и библиотеки _Qt 5_.

Инструкция по подготовке окружения, а так же готовые сборки _Qt 5_ и компиляторы, находятся [здесь](/common/build/).

## Компиляция

```sh
mkdir homed
```

```sh
git clone https://github.com/u236/homed-service-common.git \
  homed/homed-common
```

```sh
git clone https://github.com/u236/homed-serivce-zigbee.git \
  homed/homed-zigbee
```

```sh
cd homed/homed-zigbee
```

```sh
/my/qt/location/bin/qmake homed-zigbee.pro && \
  make -j $(nproc)
```
