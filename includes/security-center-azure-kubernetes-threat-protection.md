---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 06/30/2020
ms.topic: include
ms.openlocfilehash: 051cd8646718c17c288b06a535d35e1eed5e9259
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/24/2020
ms.locfileid: "95562137"
---
Security Center zapewnia ochronę przed zagrożeniami w czasie rzeczywistym w środowiskach kontenerów i generuje alerty dla podejrzanych działań. Te informacje pozwalają na szybkie rozwiązywanie problemów dotyczących zabezpieczeń i poprawę bezpieczeństwa kontenerów.

Security Center zapewnia ochronę przed zagrożeniami na różnych poziomach: 

* **Poziom hosta (dostarczany przez usługę Azure Defender dla serwerów)** — za pomocą tego samego agenta log Analytics, który Security Center używany na innych maszynach wirtualnych, usługa Azure Defender monitoruje węzły AKS systemu Linux pod kątem podejrzanych działań, takich jak wykrywanie powłoki sieci Web i połączenie z znanymi podejrzanymi adresami IP. Agent monitoruje również dla analiz specyficznych dla kontenera, takich jak tworzenie kontenera uprzywilejowanego, podejrzany dostęp do serwerów interfejsu API i serwery Secure Shell (SSH) działające wewnątrz kontenera Docker.

    >[!IMPORTANT]
    > Jeśli zdecydujesz się nie instalować agentów na hostach, będziesz otrzymywać tylko podzbiór korzyści z ochrony przed zagrożeniami i alerty zabezpieczeń. Nadal będziesz otrzymywać alerty dotyczące analizy sieci i komunikacji ze złośliwymi serwerami.

    Listę alertów na poziomie hosta AKS można znaleźć w [tabeli referencyjnej alertów](../articles/security-center/alerts-reference.md#alerts-containerhost).


* **Poziom klastra AKS (udostępniony przez usługę Azure Defender dla Kubernetes)** — na poziomie klastra ochrona przed zagrożeniami jest oparta na analizie dzienników inspekcji Kubernetes. Aby włączyć monitorowanie **bez agenta** , Włącz usługę Azure Defender. Aby generować alerty na tym poziomie, Security Center monitoruje usługi zarządzane przez AKS przy użyciu dzienników pobranych przez AKS. Przykłady zdarzeń na tym poziomie obejmują uwidocznione pulpity nawigacyjne Kubernetes, tworzenie uprzywilejowanych ról i tworzenie poufnych instalacji.

    >[!NOTE]
    > Security Center generuje alerty zabezpieczeń dotyczące akcji i wdrożeń usługi Azure Kubernetes w przypadku włączenia opcji Kubernetes w ustawieniach subskrypcji. 

    Listę alertów na poziomie klastra AKS można znaleźć w [tabeli referencyjnej alertów](../articles/security-center/alerts-reference.md#alerts-akscluster).

Ponadto nasz globalny zespół badaczy ds. zabezpieczeń stale monitoruje zagrożenie w poziomie. Po ich odnalezieniu są dodawane alerty i luki specyficzne dla kontenera.

> [!TIP]
> Alerty kontenera można symulować, postępując zgodnie z instrukcjami zawartymi w [tym wpisie w blogu](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270).