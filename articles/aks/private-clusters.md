---
title: Tworzenie prywatnego klastra usługi Azure Kubernetes Service
description: Dowiedz się, jak utworzyć prywatny klaster usługi Azure Kubernetes Service (AKS)
services: container-service
ms.topic: article
ms.date: 7/17/2020
ms.openlocfilehash: 450d68e26c5a3fc1ecfbaf6a3be6b5f698ee65e3
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/26/2020
ms.locfileid: "96183264"
---
# <a name="create-a-private-azure-kubernetes-service-cluster"></a>Tworzenie prywatnego klastra usługi Azure Kubernetes Service

W klastrze prywatnym płaszczyzna kontroli lub serwer interfejsu API ma wewnętrzne adresy IP, które są zdefiniowane w [alokacji RFC1918-Address dla prywatnego dokumentu internetowego](https://tools.ietf.org/html/rfc1918) . Za pomocą klastra prywatnego można zapewnić, że ruch sieciowy między serwerem interfejsu API a pulami węzłów pozostanie tylko w sieci prywatnej.

Płaszczyzna kontroli lub serwer interfejsu API znajduje się w subskrypcji platformy Azure zarządzanej przez usługę Azure Kubernetes Service (AKS). Klaster klienta lub Pula węzłów znajduje się w subskrypcji klienta. Serwer i Pula węzłów mogą komunikować się ze sobą za pomocą [usługi Azure Private link][private-link-service] w sieci wirtualnej serwera interfejsu API i prywatnego punktu końcowego, który jest udostępniany w podsieci klastra AKS klienta.

## <a name="region-availability"></a>Dostępność w danym regionie

Klaster prywatny jest dostępny w regionach publicznych, Azure Government i w Chinach, w których [są obsługiwane AKS](https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service).

> [!NOTE]
> Obsługiwane są Azure Government witryny, ale US Gov Teksas nie są obecnie obsługiwane z powodu braku obsługi linku prywatnego.

## <a name="prerequisites"></a>Wymagania wstępne

* Interfejs wiersza polecenia platformy Azure w wersji 2.2.0 lub nowszej

## <a name="create-a-private-aks-cluster"></a>Tworzenie prywatnego klastra AKS

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów lub Użyj istniejącej grupy zasobów dla klastra AKS.

```azurecli-interactive
az group create -l westus -n MyResourceGroup
```

### <a name="default-basic-networking"></a>Domyślna sieć podstawowa 

```azurecli-interactive
az aks create -n <private-cluster-name> -g <private-cluster-resource-group> --load-balancer-sku standard --enable-private-cluster  
```
Gdzie `--enable-private-cluster` jest obowiązkową flagą dla klastra prywatnego. 

### <a name="advanced-networking"></a>Zaawansowane sieci  

```azurecli-interactive
az aks create \
    --resource-group <private-cluster-resource-group> \
    --name <private-cluster-name> \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id <subnet-id> \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 
```
Gdzie `--enable-private-cluster` jest obowiązkową flagą dla klastra prywatnego. 

> [!NOTE]
> Jeśli adres CIDR (172.17.0.1/16) mostka platformy Docker koliduje z maską CIDR podsieci, należy odpowiednio zmienić adres mostka platformy Docker.

## <a name="options-for-connecting-to-the-private-cluster"></a>Opcje łączenia się z klastrem prywatnym

Punkt końcowy serwera interfejsu API nie ma publicznego adresu IP. Aby zarządzać serwerem interfejsu API, należy użyć maszyny wirtualnej, która ma dostęp do Virtual Network platformy Azure klastra AKS. Istnieje kilka opcji ustanawiania łączności sieciowej z klastrem prywatnym.

* Utwórz maszynę wirtualną w tej samej usłudze Azure Virtual Network (VNet) jako klaster AKS.
* Użyj maszyny wirtualnej w oddzielnym sieci i skonfiguruj [komunikację równorzędną sieci wirtualnej][virtual-network-peering].  Zapoznaj się z sekcją poniżej, aby uzyskać więcej informacji na temat tej opcji.
* Użyj usługi [Express Route lub połączenia sieci VPN][express-route-or-VPN] .

Najłatwiej jest utworzyć maszynę wirtualną w tej samej sieci wirtualnej, co klaster AKS.  Funkcja Express Route i sieci VPN zwiększa koszty i wymaga dodatkowej złożoności sieci.  Komunikacja równorzędna sieci wirtualnych wymaga zaplanowania zakresów CIDR sieci, aby upewnić się, że nie ma nakładających się zakresów.

## <a name="virtual-network-peering"></a>Komunikacja równorzędna sieci wirtualnych

Jak wspomniano, Komunikacja równorzędna sieci wirtualnej jest jednym ze sposobów uzyskiwania dostępu do klastra prywatnego. Aby użyć komunikacji równorzędnej sieci wirtualnej, należy skonfigurować połączenie między siecią wirtualną i prywatną strefą DNS.
    
1. Przejdź do grupy zasobów węzła w Azure Portal.  
2. Wybierz prywatną strefę DNS.   
3. W lewym okienku wybierz łącze **Sieć wirtualna** .  
4. Utwórz nowy link, aby dodać sieć wirtualną maszyny wirtualnej do prywatnej strefy DNS. Udostępnienie linku strefy DNS może potrwać kilka minut.  
5. W Azure Portal przejdź do grupy zasobów zawierającej sieć wirtualną klastra.  
6. W prawym okienku wybierz sieć wirtualną. Nazwa sieci wirtualnej ma postać *AKS-VNET- \**.  
7. W lewym okienku wybierz pozycję **Komunikacja równorzędna**.  
8. Wybierz pozycję **Dodaj**, Dodaj sieć wirtualną maszyny wirtualnej, a następnie utwórz komunikację równorzędną.  
9. Przejdź do sieci wirtualnej, w której znajduje się maszyna wirtualna, wybierz pozycję **Komunikacja równorzędna**, wybierz sieć wirtualną AKS, a następnie utwórz komunikację równorzędną. Jeśli zakresy adresów w sieci wirtualnej AKS i konflikty sieci wirtualnej maszyn wirtualnych są niepowodzeniem, Komunikacja równorzędna nie powiedzie się. Aby uzyskać więcej informacji, zobacz  [wirtualne sieci równorzędne][virtual-network-peering].

## <a name="hub-and-spoke-with-custom-dns"></a>Koncentrator i szprycha z niestandardowym systemem DNS

[Architektury](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) gwiazdy są często używane do wdrażania sieci na platformie Azure. W wielu z tych wdrożeń ustawienia DNS w szprychie sieci wirtualnych są skonfigurowane tak, aby odwoływać się do centralnej usługi przesyłania dalej w systemie DNS w celu umożliwienia lokalnego rozpoznawania nazw DNS opartych na platformie Azure. W przypadku wdrażania klastra AKS do takiego środowiska sieciowego należy wziąć pod uwagę pewne szczególne kwestie.

![Prywatny koncentrator klastra i szprycha](media/private-clusters/aks-private-hub-spoke.png)

1. Domyślnie po zainicjowaniu obsługi klastra prywatnego w grupie zasobów zarządzanej przez klaster jest tworzony prywatny punkt końcowy (1) i prywatna strefa DNS (2). Klaster używa rekordu A w strefie prywatnej w celu rozpoznania adresu IP prywatnego punktu końcowego na potrzeby komunikacji z serwerem interfejsu API.

2. Prywatna strefa DNS jest połączona tylko z siecią wirtualną, do której są dołączone węzły klastra (3). Oznacza to, że prywatny punkt końcowy może być rozpoznany tylko przez hosty w połączonej sieci wirtualnej. W scenariuszach, w których w sieci wirtualnej nie skonfigurowano żadnych niestandardowych nazw DNS (domyślnie), to działa bez problemu jako hosty w 168.63.129.16 dla systemu DNS, który może rozpoznawać rekordy w prywatnej strefie DNS ze względu na link.

3. W scenariuszach, w których sieć wirtualna zawierająca klaster ma niestandardowe ustawienia DNS (4), wdrożenie klastra kończy się niepowodzeniem, chyba że prywatna strefa DNS jest połączona z siecią wirtualną, która zawiera niestandardowe resolvery DNS (5). Ten link można utworzyć ręcznie po utworzeniu strefy prywatnej podczas aprowizacji klastra lub za pośrednictwem automatyzacji podczas wykrywania tworzenia strefy przy użyciu mechanizmów wdrażania opartych na zdarzeniach (na przykład Azure Event Grid i Azure Functions).

## <a name="dependencies"></a>Zależności  

* Usługa link prywatny jest obsługiwana tylko w przypadku standardowej Azure Load Balancer. Podstawowa Azure Load Balancer nie jest obsługiwana.  
* Aby użyć niestandardowego serwera DNS, należy dodać Azure DNS 168.63.129.16 IP jako nadrzędny Serwer DNS na niestandardowym serwerze DNS.

## <a name="limitations"></a>Ograniczenia 
* Nie można zastosować dozwolonych zakresów adresów IP do punktu końcowego serwera prywatnego interfejsu API, są one stosowane tylko do publicznego serwera interfejsu API
* [Strefy dostępności][availability-zones] są obecnie obsługiwane w niektórych regionach. 
* [Ograniczenia usługi Azure Private link][private-link-service] są stosowane do klastrów prywatnych.
* Brak obsługi dla agentów hostowanych przez firmę Microsoft dla platformy Azure DevOps z klastrami prywatnymi. Rozważ użycie [agentów samoobsługowych][devops-agents]. 
* W przypadku klientów, którzy muszą umożliwić Azure Container Registry pracy z prywatnym AKS, Container Registry sieci wirtualnej musi być połączona z siecią wirtualną klastra agentów.
* Brak bieżącej obsługi Azure Dev Spaces
* Brak obsługi konwertowania istniejących klastrów AKS do klastrów prywatnych
* Usunięcie lub zmodyfikowanie prywatnego punktu końcowego w podsieci klienta spowoduje, że klaster przestanie działać. 
* Azure Monitor kontenerów danych na żywo nie są obecnie obsługiwane.
* Umowa SLA na czas działania nie jest obecnie obsługiwana.


<!-- LINKS - internal -->
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[private-link-service]: ../private-link/private-link-service-overview.md#limitations
[virtual-network-peering]: ../virtual-network/virtual-network-peering-overview.md
[azure-bastion]: ../bastion/tutorial-create-host-portal.md
[express-route-or-vpn]: ../expressroute/expressroute-about-virtual-network-gateways.md
[devops-agents]: /azure/devops/pipelines/agents/agents?view=azure-devops
[availability-zones]: availability-zones.md