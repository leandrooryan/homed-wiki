---
title: 'ZigBee: Библиотека устройств'
---

# ZigBee: Библиотека устройств

Все поддерживаемые устройства описаны в файлах библиотеки, находящихся в каталоге `/usr/share/homed-zigbee`.

Путь к каталогу библиотеки может быть изменен в [конфигурации](/zigbee/configuration/). Библиотека может быть дополнена при помощи пользовательских [расширений](#_2).

## Характеристики устройств

- [Свойства](https://github.com/u236/homed-service-zigbee/blob/master/property.cpp)
- [Действия](https://github.com/u236/homed-service-zigbee/blob/master/action.cpp)
- [Биндинги](https://github.com/u236/homed-service-zigbee/blob/master/binding.cpp)
- [Отчеты](https://github.com/u236/homed-service-zigbee/blob/master/reporting.cpp)
- [Опросы](https://github.com/u236/homed-service-zigbee/blob/master/poll.cpp)
- [Способности](/common/exposes)
- [Опции](/common/options)

## Структура библиотеки

Каждый файл библиотеки представляет из себя JSON-объект, описывающий характеристики конечных точек устройства. Актуальные файлы библиотеки всегда доступны на [GitHub](https://github.com/u236/homed-service-zigbee/blob/master/deploy/data/usr/share/homed-zigbee).

Устройства идентифицируются по аттрибутам _"ManufacturerName"_ и _"ModelIdentifier"_ [кластера](/assets/pdf/ZigBee_Cluster_Library_Specification.pdf) _"Basic"_.

!!! info ""

    Существуют исключения из правил, такие, как устройства TUYA, у которых всегда _относительно_ одинаковое значение аттрибута _"ModelIdentifier"_, зато аттрибут _"ManufacturerName"_ всегда разный. Для этих устройств название производителя подменяется на "TUYA", а в качестве названия модели используется значение аттрибута _"ManufacturerName"_.

Общая структура библиотеки выглядит так:

```json
{
  "manufacturer_a":
  [
    {
      "modelNames":     ["model_m", "model_n"],
      "properties":     ["property", "property", "property"],
      "actions":        ["action", "action"],
      "bindings":       ["binding"],
      "reportings":     ["reporting"],
      "exposes":        ["expose", "expose"],
      "endpointId":     2
    },
    {
      "modelNames":     ["model_x"],
      "properties":     ["property"],
      "actions":        ["action"],
      "exposes":        ["expose"],
      "options":        {"option": "value"}
    }
  ],
  "manufacturer_b":
  [
    {
      "modelNames":     ["model_z"],
      "properties":     ["property", "property"],
      "bindings":       ["binding"],
      "polls":          ["poll"],
      "options":        {"pollInterval": 3600}
    }
  ]
}
```

### `description`

Необязательное поле, имеющее исключительно информационный характер, используется для понятного описания секции библиотеки.

### `modelNames`

Массив значений аттрибута _"ModelIdentifier"_ для устройств с одинаковым набором характеристик. Допускается использовать повторяющиеся значения этого поля в переделах одгого блока производителя устройств, например:

```json
...
    {
      "modelNames":     ["model_a", "model_b"],
      "properties":     ["status", "level"],
      "actions":        ["status", "level"]
    },
    {
      "modelNames":     ["model_a"],
      "properties":     ["colorTemperature"],
      "actions":        ["colorTemperature"]
    }
...
```

### `properties`

Массив [свойств](https://github.com/u236/homed-service-zigbee/blob/master/property.cpp) конечной точки устройства.

К свойствам относятся данные о состоянии устройства, например, температура, заряд батарейки, сигнал срабатывания датчика движения и так далее.

### `actions`

Массив [действий](https://github.com/u236/homed-service-zigbee/blob/master/action.cpp), которые могут быть применены к конечной точке устройства.

К действиям относится то, как пользователь может управлять устройством, например, включать или выключать, изменять настройки и так далее.

### `bindings`

Массив [биндингов](https://github.com/u236/homed-service-zigbee/blob/master/binding.cpp) для конечной точки устройства.

Дело в том, что далеко не все устройства после подключения к сети начинают отправлять отчеты координатору. Эту проблему решают биндинги, назначая адресом назначения для отчетов адрес координатора.

### `reportings`

Массив предустановленных параметров отправки [отчетов](https://github.com/u236/homed-service-zigbee/blob/master/reporting.cpp) для конечной точки устройства.

Отчеты - это функция устройства, с помощью которой оно периодически передает координатору некоторые свои свойства. Отчеты отправляются по заданным критериям - временным интервалам и изменению значения соответствующего аттрибута. Параметры отчетов хранятся на самом устройстве и настраиваются при [добавлении](/zigbee/commands/#setpermitjoin) устройства в сеть, при [обновлении](/zigbee/commands/#updatedevice) характеристик устройства или [вручную](/zigbee/commands/#setupreporting).

### `polls`

Массив [опросов](https://github.com/u236/homed-service-zigbee/blob/master/poll.cpp) конечной точки устройства.

Опросы - это способ принудительно получить некоторые свойства устройств, не поддерживающих отчеты. В этом случае координатор сам обращается к устройству, либо один раз, при запуске сервиса, либо раз в заданный промежуток времени.

### `exposes`

Массив [способностей](/common/exposes) устройства.

### `options`

Объект с [опцииями](/common/options) устройства.

### `endpointId`

Поле, описывающее, к каким конечным точкам устройства применимы описанные характеристики. Если поле отсутствует, используятся конечнная точка по умолчанию - `1`.

Допускается перечисление нескольких конечных точек для одной и той же модели устройства:

```json
...
    {
      "modelNames":     ["lumi.sensor_cube", "lumi.sensor_cube.aqgl01"],
      "properties":     ["lumiBatteryVoltage"],
      "endpointId":     1
    },
    {
      "modelNames":     ["lumi.sensor_cube", "lumi.sensor_cube.aqgl01"],
      "properties":     ["lumiCubeMovement"],
      "endpointId":     2
    },
    {
      "modelNames":     ["lumi.sensor_cube", "lumi.sensor_cube.aqgl01"],
      "properties":     ["lumiCubeRotation"],
      "endpointId":     3
    }
...
```

Кроме того, поле может содержать не только одиночное значение номера конечной точки, но и массив значений.

Это применимо к устройстам, имеющим несколько конечных точек c идентичным набором характеристик, таким, как многоканальные реле или выключатели:

```json
...
    {
      "modelNames":     ["multi_channel"],
      "properties":     ["status"],
      "actions":        ["status"],
      "endpointId":     [1, 2, 3]
    }
...
```

В этом случае к топикам [`fd`](/common/topics/#fd-from-device) и [`td`](/common/topics/#td-to-device), будет добавляться еще один уровень, содержащий номер конкретной конечной точки.

## Расширения библиотеки

Поддержку новых устройств можно добавлять при помощи пользовательских расширений библиотеки. Расширения это файлы с _произвольным_ именем. Содержимое таких файлов должно соответствовать формату основной библиотеки, описанному в данной статье.

Эти файлы можно создавать в каталоге, путь к которому настраивается парамером `external` в секции `[device]` файла [конфигурации](/zigbee/configuration/).

!!! warning ""

    Пользовательские расширения имеют приоритет над основной библиотекой, список файлов расширений сортируется по имени, парсер библиотеки прекращает поиск описания устройства, как только найдет первое совпадение.
