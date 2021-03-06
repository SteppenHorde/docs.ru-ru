---
title: Метод ICorProfilerCallback6::GetAssemblyReferences
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- ICorProfilerCallback6.GetAssemblyReferences
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.assetid: 8b391afb-d79f-41bd-94ce-43ce62c6b5fc
topic_type:
- apiref
ms.openlocfilehash: 0717ef5fdb6c0447ceeb801f239be08f8cca5988
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76865055"
---
# <a name="icorprofilercallback6getassemblyreferences-method"></a>Метод ICorProfilerCallback6::GetAssemblyReferences
[Поддерживается в .NET Framework 4.5.2 и более поздних версиях.]  
  
 Уведомляет профилировщика о том, что сборка находится на очень ранней стадии загрузки, когда среда CLR выполняет обход замыкания ссылки на сборку.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp
HRESULT GetAssemblyReferences(        [in, string] const WCHAR* wszAssemblyPath,  
        [in] ICorProfilerAssemblyReferenceProvider* pAsmRefProvider  
);  
```  
  
## <a name="parameters"></a>Параметры  
 `wszAssemblyPath`  
 [в] Путь и имя сборки, метаданные которой будут изменены.  
  
 `pAsmRefProvider`  
 окне Указатель на адрес интерфейса [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) , указывающий ссылки на сборку для добавления.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Значения, возвращаемые из этого обратного вызова, игнорируются.  
  
## <a name="remarks"></a>Заметки  
 Этот обратный вызов управляется путем установки флага [COR_PRF_HIGH_ADD_ASSEMBLY_REFERENCES](cor-prf-high-monitor-enumeration.md) маски событий при вызове метода [ICorProfilerCallback5:: SetEventMask2](icorprofilerinfo5-seteventmask2-method.md) . Если профилировщик регистрируется для метода обратного вызова [ICorProfilerCallback6:: GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) , среда выполнения передает путь и имя загружаемой сборки вместе с указателем на объект интерфейса [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) для этого метода. Затем профилировщик может вызвать метод [ICorProfilerAssemblyReferenceProvider:: AddAssemblyReference](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md) с объектом `COR_PRF_ASSEMBLY_REFERENCE_INFO` для каждой целевой сборки, на которую планируется ссылаться из сборки, указанной в обратном вызове `GetAssemblyReferences`.  
  
 Используйте обратный вызов `GetAssemblyReferences`, только если для добавления ссылок на сборку профилировщик должен изменить метаданные сборки. (Но обратите внимание, что фактическое изменение метаданных сборки выполняется в методе обратного вызова [ICorProfilerCallback:: ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md).) Профилировщик должен реализовать метод обратного вызова `GetAssemblyReferences`, чтобы сообщить среде CLR о том, что ссылки на сборки будут добавлены при загрузке модуля.  Это гарантирует, что решения о предоставлении общего доступа к сборке, сделанные средой CLR во время этой ранней стадии, остаются в силе, хотя позже профилировщик планирует изменить ссылки на сборку метаданных.  Это поможет избежать некоторых случаев, при которых модификации метаданных профилировщика приводят к ошибке `SECURITY_E_INCOMPATIBLE_SHARE`.  
  
 Профилировщик использует объект [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) , предоставленный этим методом, для добавления ссылок на сборки в обход закрытия ссылок на сборки CLR.  Объект [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) должен использоваться только в этом обратном вызове. Вызовы метода [ICorProfilerAssemblyReferenceProvider:: AddAssemblyReference](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md) из этого обратного вызова не приводят к изменению метаданных, но только в измененном анализе закрытия ссылки на сборку. Профилировщику по-прежнему придется использовать объект [IMetaDataAssemblyEmit](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyemit-interface.md) , чтобы явно добавлять ссылки на сборки из обратного вызова [ICorProfilerCallback:: ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) для ссылающейся сборки, даже если она реализует обратный вызов `GetAssemblyReferences`.  
  
 Профилировщик должен быть готов к получению повторяющихся вызовов этого обратного вызова для одной и той же сборки и должен одинаково реагировать на каждый такой повторный вызов (путем выполнения одного и того же набора вызовов [ICorProfilerAssemblyReferenceProvider:: AddAssemblyReference](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md) ).  
  
## <a name="requirements"></a>Требования  
 **Платформы:** см. раздел [Требования к системе](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Заголовок:** CorProf.idl, CorProf.h  
  
 **Библиотека:** CorGuids.lib  
  
 **Версии платформы .NET Framework:** [!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>См. также:

- [Интерфейс ICorProfilerCallback6](icorprofilercallback6-interface.md)
- [Метод ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md)
- [Структура COR_PRF_ASSEMBLY_REFERENCE_INFO](cor-prf-assembly-reference-info-structure.md)
- [Интерфейс ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md)
