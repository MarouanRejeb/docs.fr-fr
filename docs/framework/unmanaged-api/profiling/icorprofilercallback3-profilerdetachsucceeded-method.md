---
title: ICorProfilerCallback3::ProfilerDetachSucceeded, méthode
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback3.ProfilerDetachSucceeded Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback3::ProfilerDetachSucceeded
helpviewer_keywords:
- ProfilerDetachSucceeded method [.NET Framework profiling]
- ICorProfilerCallback3::ProfilerDetachSucceeded method [.NET Framework profiling]
ms.assetid: 05164966-16ce-4cc9-a530-43a640c00711
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: efe3070b41b1d71e0cf533a7f9f211f4c6626726
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61782562"
---
# <a name="icorprofilercallback3profilerdetachsucceeded-method"></a>ICorProfilerCallback3::ProfilerDetachSucceeded, méthode
Indique au profileur que le Common Language Runtime (CLR) est sur le point de décharger sa DLL.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
HRESULT ProfilerDetachSucceeded();  
```  
  
## <a name="return-value"></a>Valeur de retour  
 La valeur de retour de ce rappel est ignorée.  
  
## <a name="remarks"></a>Notes  
 Le rappel `ProfilerDetachSucceeded` est émis une fois que tous les threads ont quitté le code du profileur. Quand cette méthode est appelée, le profileur doit effectuer les tâches de dernière minute qui ne sont pas appropriées pour son destructeur, telles que la notification de son interface utilisateur ou l’enregistrement du composant. Toutefois, le profileur ne doit pas appeler de fonctions sur les interfaces qui sont fournies par le CLR pendant ce rappel (telles que la [ICorProfilerInfo](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo-interface.md) ou `IMetaData*` interfaces).  
  
 Le CLR crée une entrée dans le journal des événements d'application Windows pour indiquer que l'opération de détachement a abouti.  
  
 Quand le profileur retourne de ce rappel, le CLR libère l'objet de profileur et décharge la DLL du profileur. Par conséquent, le profileur ne doit pas exécuter d'actions susceptibles de provoquer l'exécution à l'intérieur de la DLL du profileur après le retour de ce rappel. Par exemple, il ne doit pas créer de threads ou enregistrer de rappels de la minuterie.  
  
## <a name="requirements"></a>Configuration requise  
 **Plateformes :** Consultez [Configuration requise](../../../../docs/framework/get-started/system-requirements.md).  
  
 **En-tête :** CorProf.idl, CorProf.h  
  
 **Bibliothèque :** CorGuids.lib  
  
 **Versions du .NET Framework :** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Voir aussi

- [Interfaces de métadonnées](../../../../docs/framework/unmanaged-api/metadata/metadata-interfaces.md)
- [ICorProfilerInfo3, interface](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo3-interface.md)
- [Interfaces de profilage](../../../../docs/framework/unmanaged-api/profiling/profiling-interfaces.md)
- [Profilage](../../../../docs/framework/unmanaged-api/profiling/index.md)
