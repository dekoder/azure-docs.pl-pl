---
title: Pojęcia — łączność z siecią
description: Dowiedz się więcej na temat kluczowych aspektów i przypadków użycia sieci i połączeń z platformą Azure VMware.
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: f8e9ed143d53afe2f7a24c832c69390c6ffcb36b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91575762"
---
# <a name="azure-vmware-solution-networking-and-interconnectivity-concepts"></a>Azure VMware rozwiązanie dotyczące sieci i międzyłączności

[!INCLUDE [avs-networking-description](includes/azure-vmware-solution-networking-description.md)]

Przydatną perspektywą dotyczącą międzyłączności jest rozważenie dwóch typów implementacji chmury prywatnej rozwiązania Azure VMware:

1. [**Podstawowa platforma Azure — tylko łączność międzyfirmowa**](#azure-virtual-network-interconnectivity) umożliwia zarządzanie chmurą prywatną i korzystanie z niej tylko przy użyciu jednej sieci wirtualnej na platformie Azure. Ta implementacja najlepiej nadaje się w przypadku ocen lub implementacji rozwiązań VMware platformy Azure, które nie wymagają dostępu ze środowisk lokalnych.

1. Funkcja międzyusługowa [**w chmurze z chmurą prywatną pozwala**](#on-premises-interconnectivity) rozszerzyć podstawową implementację wyłącznie platformy Azure w celu uwzględnienia wzajemnej łączności między chmurami prywatnymi i rozwiązaniami VMware platformy Azure.
 
W tym artykule omówiono kilka najważniejszych koncepcji, które nawiązują połączenie sieciowe i międzysieciowe, w tym wymagania i ograniczenia. Podajemy również więcej informacji na temat dwóch typów implementacji międzyłączności chmury prywatnej rozwiązań VMware platformy Azure. Ten artykuł zawiera informacje potrzebne do poprawnego skonfigurowania sieci do pracy z rozwiązaniem VMware platformy Azure.

## <a name="azure-vmware-solution-private-cloud-use-cases"></a>Scenariusze użycia chmury prywatnej dla rozwiązań VMware platformy Azure

Przypadki użycia dla chmur prywatnych rozwiązania Azure VMware obejmują:
- Nowe obciążenia maszyn wirtualnych VMware w chmurze
- Obciążenie pracą maszyny wirtualnej w chmurze (tylko rozwiązanie firmy Microsoft do platformy Azure VMware)
- Migracja obciążenia maszyn wirtualnych do chmury (tylko rozwiązanie do oprogramowania VMware na platformie Azure)
- Odzyskiwanie po awarii (rozwiązanie VMware platformy Azure do rozwiązania VMware platformy Azure lub w środowisku lokalnym do rozwiązania Azure VMware)
- Użycie usług platformy Azure

> [!TIP]
> Wszystkie przypadki użycia usługi Azure VMware Solution są włączane z łącznością lokalną z chmurą prywatną.

## <a name="azure-virtual-network-interconnectivity"></a>Łączność z siecią wirtualną platformy Azure

W ramach implementacji chmury prywatnej w sieci wirtualnej możesz zarządzać chmurą prywatną rozwiązania Azure VMware, korzystać z obciążeń w chmurze prywatnej i uzyskiwać dostęp do usług platformy Azure za pośrednictwem połączenia ExpressRoute. 

Na poniższym diagramie przedstawiono podstawową komunikację międzysieciową w czasie wdrażania chmury prywatnej. Przedstawia ona logiczne, ExpressRoute sieci między siecią wirtualną na platformie Azure a chmurą prywatną. Międzyłączność spełnia trzy podstawowe przypadki użycia:
* Dostęp przychodzący do programu vCenter Server i Menedżera NSX-T, który jest dostępny z maszyn wirtualnych w ramach subskrypcji platformy Azure, a nie z systemów lokalnych. 
* Dostęp wychodzący z maszyn wirtualnych do usług platformy Azure. 
* Dostęp przychodzący i użycie obciążeń z uruchomioną chmurą prywatną.

:::image type="content" source="media/concepts/adjacency-overview-drawing-single.png" alt-text="Podstawowa Sieć wirtualna do łączności z chmurą prywatną" border="false":::

## <a name="on-premises-interconnectivity"></a>Międzyfirmowa łączność lokalna

W sieci wirtualnej i w środowisku lokalnym do pełnej implementacji chmury prywatnej możesz uzyskać dostęp do chmur prywatnych rozwiązań VMware platformy Azure ze środowiska lokalnego. Ta implementacja stanowi rozszerzenie podstawowej implementacji opisanej w poprzedniej sekcji. Podobnie jak w przypadku podstawowej implementacji, obwód usługi ExpressRoute jest wymagany, ale w tej implementacji jest używany do nawiązywania połączeń ze środowisk lokalnych z chmurą prywatną na platformie Azure. 

Na poniższym diagramie przedstawiono międzyfirmowe łączenie z chmurą prywatną, co umożliwia wykonywanie następujących przypadków użycia:
* Gorąca/zimna vMotion — vCenter
* Dostęp do zarządzania chmurą prywatną w usłudze Azure VMware

:::image type="content" source="media/concepts/adjacency-overview-drawing-double.png" alt-text="Podstawowa Sieć wirtualna do łączności z chmurą prywatną" border="false":::

W celu uzyskania pełnej łączności z chmurą prywatną należy włączyć ExpressRoute Global Reach a następnie zażądać klucza autoryzacji i prywatnego identyfikatora komunikacji równorzędnej dla Global Reach w Azure Portal. Klucz autoryzacji i identyfikator komunikacji równorzędnej są używane do ustanawiania Global Reach między obwodem usługi ExpressRoute w ramach subskrypcji i obwodem usługi ExpressRoute dla nowej chmury prywatnej. Po połączeniu te dwa obwody usługi ExpressRoute kierują ruchem sieciowym między środowiskami lokalnymi a chmurą prywatną.  Zapoznaj się z [samouczkiem dotyczącym tworzenia ExpressRoute Global REACH komunikacji równorzędnej z chmurą prywatną](tutorial-expressroute-global-reach-private-cloud.md) w celu uzyskania procedur żądania i używania klucza autoryzacji i identyfikatora komunikacji równorzędnej.



## <a name="next-steps"></a>Następne kroki 
Zapoznaj się z [pojęciami dotyczącymi magazynu w chmurze prywatnej](concepts-storage.md).


<!-- LINKS - external -->
[enable Global Reach]: ../expressroute/expressroute-howto-set-global-reach.md

<!-- LINKS - internal -->

