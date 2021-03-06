---
title: Метод ICorDebugSymbolProvider2::GetGenericDictionaryInfo
ms.date: 03/30/2017
ms.assetid: ba28fe4e-5491-4670-bff7-7fde572d7593
ms.openlocfilehash: 02ecaf56e845680472f42c04f3978e54e7a69272
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791507"
---
# <a name="icordebugsymbolprovider2getgenericdictionaryinfo-method"></a>Метод ICorDebugSymbolProvider2::GetGenericDictionaryInfo

Получает универсальную карту словаря

## <a name="syntax"></a>Синтаксис

```cpp
HRESULT GetGenericDictionaryInfo(
   [out] ICorDebugMemoryBuffer** ppMemoryBuffer
);
```

## <a name="parameters"></a>Параметры

`ppMemoryBuffer`\
заполняет Указатель на адрес объекта [икордебугмеморибуффер](icordebugmemorybuffer-interface.md) , содержащего универсальную карту словаря. Дополнительные сведения см. в разделе «Примечания».

## <a name="remarks"></a>Заметки

> [!NOTE]
> Этот метод доступен только в машинном коде .NET.

Карта состоит из двух разделов верхнего уровня.

- [Каталог](#Directory) , содержащий относительные виртуальные адреса (RVA) всех словарей, входящих в эту карту.

- [Куча](#Heap) с согласованием байтов, которая содержит сведения о создании экземпляра объекта. Она запускается сразу после последней записи каталога.

<a name="Directory"></a>

## <a name="the-directory"></a>Каталог

Каждая запись в каталоге ссылается на смещение в куче; то есть это смещение относительно начала кучи. Значения отдельных записей, не обязательно должны быть уникальными; несколько записей каталога могут указывать на одно и то же смещение в куче.

Часть каталога универсальной карты словаря имеет следующую структуру.

- Первые 4 байта содержит число записей словаря (т. е. число относительных виртуальных адресов в словаре). Мы будем называть это значение *N*. Если задан высокий бит, записи сортируются по относительному виртуальному адресу в возрастающем порядке.

- Далее следуют записи каталога *N* . Каждая запись состоит из 8 байт в двух 4-байтовых сегментах.

  - Байты с 0 по 3: RVA; относительный виртуальный адрес словаря.

  - Байты с 4 по 7: смещение; смещение относительно начала кучи.

<a name="Heap"></a>

## <a name="the-heap"></a>Куча

Размер кучи может быть вычислен модулем чтения потока путем вычитания длины потока из размера каталога + 4. Другими словами:

```csharp
Heap Size = Stream.Length – (Directory Size + 4)
```

где размер каталога равен `N * 8`.

Формат каждого элемента сведений о создании экземпляра в куче выглядит следующим образом.

- Длина этого элемента сведений о создании экземпляра в байтах в сжатом формате ECMA метаданных. Значение исключает эти сведения о длине.

- Число универсальных типов создания экземпляров, или *T*, в сжатом формате метаданных ECMA.

- Типы *T* , каждый из которых представлен в формате сигнатуры типа ECMA.

Включение длины для каждого элемента кучи позволяет простую сортировку раздела каталога без влияния на кучу.

## <a name="requirements"></a>Требования

**Платформы:** см. раздел [Требования к системе](../../../../docs/framework/get-started/system-requirements.md).

**Заголовок:** CorDebug.idl, CorDebug.h

**Библиотека:** CorGuids.lib

**Версии платформы .NET Framework:** [!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]

## <a name="see-also"></a>См. также:

- [Интерфейс ICorDebugSymbolProvider2](icordebugsymbolprovider2-interface.md)
- [Интерфейсы отладки](debugging-interfaces.md)
