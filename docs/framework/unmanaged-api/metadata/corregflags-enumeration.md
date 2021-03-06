---
title: Перечисление CorRegFlags
ms.date: 03/30/2017
api_name:
- CorRegFlags
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorRegFlags
helpviewer_keywords:
- CorRegFlags enumeration [.NET Framework metadata]
ms.assetid: 8d3080ee-39fe-4c57-8950-51323632d045
topic_type:
- apiref
ms.openlocfilehash: 8fe6216e11a64ea182d796247d888b862b1e8377
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2020
ms.locfileid: "79177930"
---
# <a name="corregflags-enumeration"></a>Перечисление CorRegFlags
Обеспечивает значения флага, используемые для регистрации при установке модуля или композитного изображения.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
typedef enum
{  
    regNoCopy  = 0x00000001,  
    regConfig  = 0x00000002,  
    regHasRefs = 0x00000004  
} CorRegFlags;  
```  
  
## <a name="members"></a>Члены  
  
|Участник|Описание|  
|------------|-----------------|  
|`regNoCopy`|Уточняется, что файлы не должны быть скопированы в пункт назначения.|  
|`regConfig`|Уточняется, что модуль или композит является конфигурацией.|  
|`regHasRefs`|Уточняется, что модуль или композит имеет ссылки класса.|  
  
## <a name="requirements"></a>Требования  
 **Платформы:** см. раздел [Требования к системе](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Заголовок:** Cor.h  
  
 **Библиотека:** Включено в качестве ресурса в MsCorEE.dll  
  
 **Версии платформы .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>См. также раздел

- [Перечисления метаданных](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
