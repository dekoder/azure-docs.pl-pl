---
title: Samouczek — Dodawanie segmentu sieci NSX-T w rozwiązaniu Azure VMware
description: Dowiedz się, jak utworzyć segment sieci NSX-T do użycia dla maszyn wirtualnych w programie vCenter.
ms.topic: tutorial
ms.date: 11/09/2020
ms.openlocfilehash: 8ecb37a42e2986bd1c6261b8fe6c23382323b31d
ms.sourcegitcommit: 2a8a53e5438596f99537f7279619258e9ecb357a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2020
ms.locfileid: "94335052"
---
# <a name="tutorial-add-a-network-segment-in-azure-vmware-solution"></a>Samouczek: Dodawanie segmentu sieci w rozwiązaniu VMware platformy Azure 

Maszyny wirtualne utworzone w programie vCenter są umieszczane na segmentach sieci utworzonych w NSX-T i są widoczne w programie vCenter.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Nawigowanie w Menedżerze NSX-T w celu dodania segmentów sieci
> * Dodaj nowy segment sieci
> * Obserwuj nowy segment sieci w programie vCenter

## <a name="prerequisites"></a>Wymagania wstępne

Chmurę prywatną rozwiązania VMware platformy Azure z dostępem do interfejsów Menedżera vCenter i NSX-T. Aby uzyskać więcej informacji, zobacz samouczek [Konfigurowanie sieci](tutorial-configure-networking.md) .

## <a name="add-a-network-segment"></a>Dodawanie segmentu sieci

[!INCLUDE [add-network-segment-steps](includes/add-network-segment-steps.md)]

## <a name="next-steps"></a>Następne kroki

W tym samouczku utworzysz segment sieci NSX-T do użycia dla maszyn wirtualnych w programie vCenter. 

Teraz możesz: 

- [Tworzenie i zarządzanie serwerem DHCP dla rozwiązań VMware platformy Azure](manage-dhcp.md)
- [Tworzenie biblioteki zawartości w celu wdrożenia maszyn wirtualnych w rozwiązaniu VMware platformy Azure](deploy-vm-content-library.md) 
- [Równorzędne środowiska lokalne z chmurą prywatną](tutorial-expressroute-global-reach-private-cloud.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->
