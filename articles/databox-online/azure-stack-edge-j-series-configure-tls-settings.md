---
title: Konfigurowanie protokołu TLS 1,2 na klientach z systemem Windows uzyskujących dostęp do urządzenia Azure Stack Edge
description: Zawiera opis sposobu konfigurowania protokołu TLS 1,2 na klientach z systemem Windows, do którego jest uzyskiwany dostęp Azure Stack do urządzenia z procesorem GPU
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: 9a6b0910fcfd2a632f2520a2fe683b15592017cf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2020
ms.locfileid: "90891171"
---
# <a name="configure-tls-12-on-windows-clients-accessing-azure-stack-edge-pro-device"></a>Konfigurowanie protokołu TLS 1,2 na klientach z systemem Windows uzyskujących dostęp do urządzenia Azure Stack EDGE Pro

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

W przypadku korzystania z klienta systemu Windows w celu uzyskania dostępu do urządzenia z systemem Azure Stack EDGE Pro wymagane jest skonfigurowanie protokołu TLS 1,2 na komputerze klienckim. Ten artykuł zawiera zasoby i wskazówki dotyczące konfigurowania protokołu TLS 1,2 na kliencie systemu Windows. 

Wytyczne podane w tym miejscu są oparte na testowaniu wykonywanym na kliencie z systemem Windows Server 2016.

## <a name="configure-tls-12-for-current-powershell-session"></a>Konfigurowanie protokołu TLS 1,2 dla bieżącej sesji programu PowerShell

Wykonaj następujące kroki, aby skonfigurować protokół TLS 1,2 na kliencie.

1. Uruchom program PowerShell jako administrator.
2. Aby ustawić protokół TLS 1,2 dla bieżącej sesji programu PowerShell, wpisz:
  
    ```azurepowershell
    $TLS12Protocol = [System.Net.SecurityProtocolType] 'Ssl3 , Tls12'
    [System.Net.ServicePointManager]::SecurityProtocol = $TLS12Protocol
    ```
## <a name="configure-tls-12-on-client"></a>Konfigurowanie protokołu TLS 1,2 na kliencie

Jeśli chcesz ustawić protokół TLS 1,2 dla danego środowiska, postępuj zgodnie z wytycznymi w następujących dokumentach:

- [Ogólne — Włączanie protokołu TLS 1,2](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12)
- [Jak włączyć protokół TLS 1,2 na klientach](https://docs.microsoft.com/configmgr/core/plan-design/security/enable-tls-1-2-client)
- [Jak włączyć protokół TLS 1,2 na serwerach lokacji i zdalnych systemach lokacji](https://docs.microsoft.com/configmgr/core/plan-design/security/enable-tls-1-2-server)
- [Protokoły protokołu TLS/SSL (dostawca obsługi zabezpieczeń Schannel)](https://docs.microsoft.com/windows-server/security/tls/manage-tls#configuring-tls-ecc-curve-order)
- [Mechanizmy szyfrowania](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12): w [celu skonfigurowania kolejności mechanizmów szyfrowania TLS](https://docs.microsoft.com/windows-server/security/tls/manage-tls#configuring-tls-cipher-suite-order) upewnij się, że zostały wymienione bieżące mechanizmy szyfrowania i poprzedź wszystkie brakujące z poniższej listy:

    - TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
    - TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
    - TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
    - TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384

    Można również dodać te mechanizmy szyfrowania przez bezpośrednie edytowanie ustawień rejestru.

    ```azurepowershell
    New-ItemProperty -Path "$HklmSoftwarePath\Policies\Microsoft\Cryptography\Configuration\SSL\00010002" -Name "Functions"  -PropertyType String -Value ("TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384, TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384, TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384")
    ```

- Jak ustawić krzywe eliptyczne

    Upewnij się, że wyświetlasz listę aktualnych krzywych i dowiesz się, że nie ma żadnych braku z poniższej listy:

    - P-256 
    - P-384

    Możesz również dodać te krzywe eliptyczne przez bezpośrednie edytowanie ustawień rejestru.
    
    ```azurepowershell
    New-ItemProperty -Path "$HklmSoftwarePath\Policies\Microsoft\Cryptography\Configuration\SSL\00010002" -Name "EccCurves" -PropertyType MultiString -Value @("NistP256", "NistP384")
    ```
    
    - [Ustaw minimalny rozmiar wymiany klucza RSA na 2048](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#keyexchangealgorithm---client-rsa-key-sizes).



## <a name="next-steps"></a>Następne kroki

[Połącz z Azure Resource Manager](azure-stack-edge-j-series-connect-resource-manager.md)