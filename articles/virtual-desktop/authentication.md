---
title: Uwierzytelnianie pulpitu wirtualnego systemu Windows — Azure
description: Metody uwierzytelniania dla pulpitu wirtualnego systemu Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 09/04/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 5681228e5da2708912d69f16a4b09a4a93d8bb04
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2020
ms.locfileid: "89500304"
---
# <a name="supported-authentication-methods"></a>Obsługiwane metody uwierzytelniania

W tym artykule przedstawiono krótkie omówienie rodzajów uwierzytelniania, których można użyć na pulpicie wirtualnym systemu Windows.

## <a name="session-host-authentication"></a>Uwierzytelnianie hosta sesji

Pulpit wirtualny systemu Windows obsługuje uwierzytelnianie hosta sesji zarówno NT LAN Manager (NTLM), jak i protokołu Kerberos. Aby jednak korzystać z protokołu Kerberos, klient musi uzyskać bilety zabezpieczeń Kerberos z usługi centrum dystrybucji kluczy (KDC) działającej na kontrolerze domeny. Aby uzyskać bilety, klient potrzebuje bezpośredniej linii wglądu do kontrolera domeny. Możesz uzyskać bezpośredni wgląd w szczegółowe informacje przy użyciu sieci firmowej. Możesz również użyć połączenia sieci VPN z siecią firmową.

Są to obecnie obsługiwane metody logowania:

- Klient klasyczny systemu Windows
    - Nazwa użytkownika i hasło
    - Kart
    - Windows Hello
- Klient sklepu Windows
    - Nazwa użytkownika i hasło
- Klient internetowy
    - Nazwa użytkownika i hasło
- Android
    - Nazwa użytkownika i hasło
- iOS
    - Nazwa użytkownika i hasło
- macOS
    - Nazwa użytkownika i hasło

>[!NOTE]
>Karty inteligentne i Windows Hello mogą korzystać tylko z protokołu Kerberos do logowania. Zalogowanie się przy użyciu protokołu Kerberos wymaga linii wglądu do kontrolera domeny.

## <a name="hybrid-identity"></a>Tożsamość hybrydowa

Pulpit wirtualny systemu Windows obsługuje [tożsamości hybrydowe](../active-directory/hybrid/whatis-hybrid-identity.md) za pośrednictwem Azure Active Directory (AD), w tym federacyjnych przy użyciu Active Directory Federation Services (AD FS). Ponieważ użytkownicy muszą mieć możliwość odnajdywania za pomocą usługi Azure AD, pulpit wirtualny systemu Windows nie obsługuje wdrożeń autonomicznych Active Directory z usługami ADFS.

## <a name="single-sign-on-sso"></a>Logowanie jednokrotne

Pulpit wirtualny systemu Windows obecnie nie obsługuje Active Directory Federation Services (ADFS) na potrzeby logowania jednokrotnego.

Jedynym sposobem, aby uniknąć wyświetlania monitu o podanie poświadczeń dla hosta sesji jest zapisanie ich na kliencie. Zalecamy wykonanie tej czynności tylko z bezpiecznymi urządzeniami, aby uniemożliwić innym użytkownikom dostęp do zasobów.

## <a name="next-steps"></a>Następne kroki

Chcesz wiedzieć o innych sposobach zapewnienia bezpieczeństwa wdrożenia? Zapoznaj się z [najlepszymi rozwiązaniami dotyczącymi zabezpieczeń](security-guide.md).
