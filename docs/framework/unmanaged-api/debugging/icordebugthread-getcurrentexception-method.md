---
title: ICorDebugThread::GetCurrentException, méthode
ms.date: 03/30/2017
api_name:
- ICorDebugThread.GetCurrentException
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugThread::GetCurrentException
helpviewer_keywords:
- ICorDebugThread::GetCurrentException method [.NET Framework debugging]
- GetCurrentException method [.NET Framework debugging]
ms.assetid: 331ed465-a195-4359-8584-b82c6098b29b
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: c4baa4eb4da48b923ab0137ca25d9d819c94e33d
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61994030"
---
# <a name="icordebugthreadgetcurrentexception-method"></a>ICorDebugThread::GetCurrentException, méthode
Obtient un pointeur d’interface vers un objet ICorDebugValue qui représente une exception levée par le code managé.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
HRESULT GetCurrentException (  
    [out] ICorDebugValue **ppExceptionObject  
);  
```  
  
## <a name="parameters"></a>Paramètres  
 `ppExceptionObject`  
 [out] Un pointeur vers l’adresse d’un `ICorDebugValue` objet qui représente l’exception levée par code managé.  
  
## <a name="remarks"></a>Notes  
 L’objet exception existe à partir du moment où l’exception est levée jusqu'à la fin de la `catch` bloc. Une évaluation de fonction, qui est effectuée par les méthodes ICorDebugEval, effacera l’objet exception sur le programme d’installation et restaurez-la sur la saisie semi-automatique.  
  
 Les exceptions peuvent être imbriquées (par exemple, si une exception est levée dans un filtre ou dans une évaluation de fonction), afin qu’il peut y avoir plusieurs exceptions en attente sur un thread unique. `GetCurrentException` Retourne l’exception la plus récente.  
  
 L’objet exception et le type peuvent changer pendant la durée de vie de l’exception. Par exemple, après la levée d’une exception de type x, le common language runtime (CLR) peut manquer de mémoire et le promouvoir en une exception de mémoire insuffisante.  
  
## <a name="requirements"></a>Configuration requise  
 **Plateformes :** Consultez [Configuration requise](../../../../docs/framework/get-started/system-requirements.md).  
  
 **En-tête :** CorDebug.idl, CorDebug.h  
  
 **Bibliothèque :** CorGuids.lib  
  
 **Versions du .NET Framework :** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
