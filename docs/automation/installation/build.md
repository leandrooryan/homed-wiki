# Automation: Компиляция исходного кода

Перед компиляция исходного кода _HOMEd Automation_ необходимо подготовить окружение, включающее в себя нужный компилятор и библиотеки _Qt 5_.

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
git clone https://github.com/u236/homed-serivce-automation.git \
  homed/homed-automation
```

```sh
cd homed/homed-automation
```

```sh
/my/qt/location/bin/qmake homed-automation.pro && \
  make -j $(nproc)
```
