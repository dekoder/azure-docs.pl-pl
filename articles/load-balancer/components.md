---
title: Składniki Azure Load Balancer
description: Przegląd składników Azure Load Balancer
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/04/2020
ms.author: allensu
ms.openlocfilehash: bf7a35e8cedbe62aafb29aa6d9dc8fcb42e90b2e
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2020
ms.locfileid: "94693770"
---
# <a name="azure-load-balancer-components"></a>Składniki Azure Load Balancer

Azure Load Balancer zawiera kilka najważniejszych składników. Te składniki można skonfigurować w ramach subskrypcji za pośrednictwem:

* Azure Portal
* Interfejs wiersza polecenia platformy Azure
* Azure PowerShell
* Szablony usługi Resource Manager

## <a name="frontend-ip-configuration"></a>Konfiguracja adresu IP frontonu <a name = "frontend-ip-configurations"></a>

Adres IP Azure Load Balancer. Jest to punkt kontaktu dla klientów. Mogą to być następujące adresy IP:

- **Publiczny adres IP**
- **Prywatny adres IP**

Charakter adresu IP określa **Typ** utworzonego modułu równoważenia obciążenia. Wybór prywatnego adresu IP powoduje utworzenie wewnętrznego modułu równoważenia obciążenia. Wybór publicznego adresu IP powoduje utworzenie publicznego modułu równoważenia obciążenia.

|  | Publiczny moduł równoważenia obciążenia  | Wewnętrzny moduł równoważenia obciążenia |
| ---------- | ---------- | ---------- |
| **Konfiguracja adresu IP frontonu**| Publiczny adres IP | Prywatny adres IP|
| **Opis** | Publiczny moduł równoważenia obciążenia mapuje publiczny adres IP i port ruchu przychodzącego na prywatny adres IP i port maszyny wirtualnej. Usługa równoważenia obciążenia mapuje ruch w inny sposób wokół ruchu odpowiedzi z maszyny wirtualnej. Można dystrybuować określone typy ruchu między wieloma maszynami wirtualnymi lub usługami, stosując reguły równoważenia obciążenia. Na przykład można rozłożyć obciążenie ruchu związanego z żądaniami internetowymi na wiele serwerów internetowych.| Wewnętrzny moduł równoważenia obciążenia dystrybuuje ruch do zasobów znajdujących się w sieci wirtualnej. Platforma Azure ogranicza dostęp do adresów IP frontonu sieci wirtualnej ze zrównoważonym obciążeniem. Adresy IP frontonu i sieci wirtualne nigdy nie są bezpośrednio ujawniane w internetowym punkcie końcowym. Wewnętrzne aplikacje biznesowe są uruchomiane na platformie Azure i dostęp do nich jest uzyskiwany z poziomu platformy Azure lub zasobów lokalnych. |
| **Obsługiwane jednostki SKU** | Basic, standard | Basic, standard |

![Przykład modułu równoważenia obciążenia warstwowego](./media/load-balancer-overview/load-balancer.png)

Load Balancer może mieć wiele adresów IP frontonu. Dowiedz się więcej na temat [wielu frontonów](load-balancer-multivip-overview.md).

## <a name="backend-pool"></a>Pula zaplecza

Grupa maszyn wirtualnych lub wystąpień w zestawie skalowania maszyn wirtualnych obsługujących żądanie przychodzące. Aby zapewnić oszczędne skalowanie w celu spełnienia dużych ilości ruchu przychodzącego, wytyczne obliczeniowe zwykle zaleca się dodanie większej liczby wystąpień do puli zaplecza.

Usługa równoważenia obciążenia natychmiast ponownie konfiguruje siebie za pomocą automatycznej zmiany konfiguracji podczas skalowania wystąpień w górę lub w dół. Dodawanie lub usuwanie maszyn wirtualnych z puli zaplecza ponownie konfiguruje moduł równoważenia obciążenia bez dodatkowych operacji. Zakresem puli zaplecza jest dowolna maszyna wirtualna w sieci wirtualnej.

Biorąc pod uwagę sposób projektowania puli zaplecza, należy zaprojektować dla najmniejszej liczby zasobów puli zaplecza, aby zoptymalizować długość operacji zarządzania. Nie ma różnicy w wydajności lub skali danych.

## <a name="health-probes"></a>Sondy kondycji

Sonda kondycji służy do określenia stanu kondycji wystąpień w puli zaplecza. Podczas tworzenia modułu równoważenia obciążenia Skonfiguruj sondę kondycji, aby można było używać usługi równoważenia obciążenia.  Sonda kondycji określi, czy wystąpienie jest w dobrej kondycji i może odbierać ruch.

Można zdefiniować próg złej kondycji dla sond kondycji. Gdy Sonda nie odpowiada, Load Balancer przestaje wysyłać nowe połączenia do wystąpień w złej kondycji. Awaria sondy nie ma wpływu na istniejące połączenia. Połączenie jest kontynuowane do momentu, gdy aplikacja:

- Zamyka przepływ
- Limit czasu bezczynności
- Maszyna wirtualna jest zamykana

Load Balancer oferuje różne typy sond kondycji dla punktów końcowych: TCP, HTTP i HTTPS. [Dowiedz się więcej o Load Balancer sondy kondycji](load-balancer-custom-probe-overview.md).

Podstawowa Load Balancer nie obsługuje sond protokołu HTTPS. Podstawowa Load Balancer zamyka wszystkie połączenia TCP (łącznie z ustanowionymi połączeniami).

## <a name="load-balancing-rules"></a>Reguły równoważenia obciążenia

Reguła Load Balancer służy do definiowania sposobu dystrybucji ruchu przychodzącego do **wszystkich** wystąpień w ramach puli zaplecza. Reguła równoważenia obciążenia mapuje daną konfigurację i port IP frontonu na wiele adresów IP i portów zaplecza.

Na przykład użyj reguły równoważenia obciążenia dla portu 80, aby skierować ruch z adresu IP frontonu do portu 80 wystąpień zaplecza.

<p align="center">
  <img src="./media/load-balancer-components/lbrules.svg" alt= "Figure depicts how Azure Load Balancer directs frontend port 80 to three instances of backend port 80." width="512" title="Reguły równoważenia obciążenia">
</p>

*Ilustracja: reguły równoważenia obciążenia*

## <a name="high-availability-ports"></a>Porty wysokiej dostępności

Reguła modułu równoważenia obciążenia skonfigurowana z opcją **"Protokół-All i port-0"**. 

Ta reguła umożliwia pojedynczej regule równoważenia obciążenia wszystkich przepływów TCP i UDP, które docierają do wszystkich portów wewnętrznej usługa Load Balancer w warstwie Standardowa. 

Podejmowana jest decyzja dotycząca równoważenia obciążenia dla przepływu. Ta akcja jest oparta na następującym połączeniu z pięcioma krotkami: 

1. 
    źródłowy adres IP
  
2. port źródłowy
3. docelowy adres IP
4. port docelowy
5. protokol

Reguły równoważenia obciążenia portów HA ułatwiają scenariusze krytyczne, takie jak wysoka dostępność i skalowanie dla wirtualnych urządzeń sieciowych (urządzeń WUS) w sieciach wirtualnych. Funkcja może pomóc w przypadku, gdy duża liczba portów musi być zrównoważona obciążenia.

<p align="center">
  <img src="./media/load-balancer-components/harules.svg" alt="Figure depicts how Azure Load Balancer directs all frontend ports to three instances of all backend ports" width="512" title="Reguły portów HA">
</p>

*Ilustracja: reguły portów HA*

Dowiedz się więcej o [portach ha](load-balancer-ha-ports-overview.md).

## <a name="inbound-nat-rules"></a>Reguły NAT dla ruchu przychodzącego

Przychodzące reguły NAT przesyłają ruch przychodzący do kombinacji portów i adresów IP frontonu. Ruch jest wysyłany do **określonej** maszyny wirtualnej lub wystąpienia w puli zaplecza. Przekazywanie portów odbywa się przy użyciu tego samego rozkładu opartego na wykorzystaniu skrótu co Równoważenie obciążenia.

Na przykład jeśli chcesz, aby w ramach sesji Remote Desktop Protocol (RDP) lub Secure Shell (SSH) oddzielić wystąpienia maszyn wirtualnych w puli zaplecza. Wiele wewnętrznych punktów końcowych można zamapować na porty przy użyciu tego samego adresu IP frontonu. Adresy IP frontonu mogą służyć do zdalnego administrowania maszynami wirtualnymi bez dodatkowego pola skoku.

<p align="center">
  <img src="./media/load-balancer-components/inboundnatrules.svg" alt="Figure depicts how Azure Load Balancer directs frontend ports 3389, 443, and 80 to backend ports with the same values on separate servers." width="512" title="Reguły NAT dla ruchu przychodzącego">
</p>

*Ilustracja: reguły NAT dla ruchu przychodzącego*

Reguły NAT dla ruchu przychodzącego w kontekście Virtual Machine Scale Sets są pulami NAT dla ruchu przychodzącego. Dowiedz się więcej o [składnikach Load Balancer i zestawie skalowania maszyn wirtualnych](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#azure-virtual-machine-scale-sets-with-azure-load-balancer).

## <a name="outbound-rules"></a>Reguły ruchu wychodzącego

Reguła ruchu wychodzącego konfiguruje wychodzące translatora adresów sieciowych dla wszystkich maszyn wirtualnych lub wystąpień identyfikowanych przez pulę zaplecza. Ta reguła umożliwia wystąpienie w zapleczu komunikację (wychodzącą) z Internetem lub innymi punktami końcowymi.

Dowiedz się więcej o [połączeniach wychodzących i regułach](load-balancer-outbound-connections.md).

Podstawowa usługa równoważenia obciążenia nie obsługuje reguł ruchu wychodzącego.

## <a name="limitations"></a>Ograniczenia

- Dowiedz się więcej na temat [limitów](../azure-resource-manager/management/azure-subscription-service-limits.md) Load Balancer 
- Moduł równoważenia obciążenia zapewnia Równoważenie obciążenia i przekazywanie portów dla określonych protokołów TCP lub UDP. Reguły równoważenia obciążenia i reguły NAT dla ruchu przychodzącego obsługują protokoły TCP i UDP, ale nie są to adresy IP, w tym protokół ICMP.
- Przepływ wychodzący z maszyny wirtualnej zaplecza do frontonu wewnętrznej Load Balancer zakończy się niepowodzeniem.
- Reguła modułu równoważenia obciążenia nie może obejmować dwóch sieci wirtualnych.  Frontony i ich wystąpienia zaplecza muszą znajdować się w tej samej sieci wirtualnej.  
- Fragmenty adresów IP przekazywania nie są obsługiwane w regułach równoważenia obciążenia. Fragmentacja IP pakietów UDP i TCP nie jest obsługiwana w regułach równoważenia obciążenia. Reguły równoważenia obciążenia portów HA mogą służyć do przesyłania dalej istniejących fragmentów adresów IP. Aby uzyskać więcej informacji, zobacz [Omówienie portów wysokiej dostępności](load-balancer-ha-ports-overview.md).

## <a name="next-steps"></a>Następne kroki

- Aby rozpocząć korzystanie z Load Balancer, zobacz [tworzenie usługa Load Balancer w warstwie Standardowa publicznego](quickstart-load-balancer-standard-public-portal.md) .
- Dowiedz się więcej o [Azure Load Balancer](load-balancer-overview.md).
- Informacje o [publicznym adresie IP](../virtual-network/virtual-network-public-ip-address.md)
- Informacje o [prywatnym adresie IP](../virtual-network/private-ip-addresses.md)
- Dowiedz się więcej o korzystaniu z [Usługa Load Balancer w warstwie Standardowa i strefy dostępności](load-balancer-standard-availability-zones.md).
- Dowiedz się więcej na temat [diagnostyki usługa Load Balancer w warstwie Standardowa](load-balancer-standard-diagnostics.md).
- Więcej informacji [na temat resetowania protokołu TCP w trybie bezczynności](load-balancer-tcp-reset.md).
- Dowiedz się więcej o [Usługa Load Balancer w warstwie Standardowa z regułami równoważenia obciążenia dla portów ha](load-balancer-ha-ports-overview.md).
- Dowiedz się więcej na temat [sieciowych grup zabezpieczeń](../virtual-network/network-security-groups-overview.md).
- Dowiedz się więcej o [limitach usługi równoważenia obciążenia](../azure-resource-manager/management/azure-subscription-service-limits.md#load-balancer).
- Dowiedz się więcej o używaniu [przekazywania portów](./tutorial-load-balancer-port-forwarding-portal.md).