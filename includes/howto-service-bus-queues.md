---
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/25/2018
ms.author: spelluru
ms.openlocfilehash: d3d33c87dc1adf65a53b71cc4c833e7f4a191670
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/05/2020
ms.locfileid: "67183471"
---
## <a name="what-are-service-bus-queues"></a>Czym są kolejki usługi Service Bus?
Kolejki usługi Service Bus obsługują model komunikacji z użyciem **komunikatów obsługiwanych przez blokera**. Podczas korzystania z kolejek składniki aplikacji rozproszonej nie komunikują się bezpośrednio ze sobą, lecz wymieniają komunikaty za pośrednictwem kolejki, która działa jako pośrednik (broker). Producent komunikatu (nadawca) przekazuje komunikat do kolejki i kontynuuje jego przetwarzanie. Konsument komunikatu (odbiorca) asynchronicznie pobiera komunikat z kolejki i go przetwarza. Producent nie musi czekać na odpowiedź od konsumenta, aby kontynuować przetwarzanie i wysyłanie dalszych komunikatów. Kolejki umożliwiają dostarczanie komunikatów metodą **pierwszy na wejściu — pierwszy na wyjściu (FIFO)** do jednego lub większej liczby konkurencyjnych konsumentów. To oznacza, że komunikaty są zwykle odbierane i przetwarzane przez odbiorców w kolejności, w której zostały dodane do kolejki, a każdy komunikat jest odbierany i przetwarzany przez tylko jednego konsumenta.

![QueueConcepts](./media/howto-service-bus-queues/sb-queues-08.png)

Kolejki usługi Service Bus to technologia ogólnego przeznaczenia, która może być używana w wielu różnych scenariuszach:

* Komunikacja między rolami sieci Web i procesów roboczych w wielowarstwowej aplikacji Azure.
* Komunikacja między aplikacjami lokalnymi i hostowanymi na platformie Azure w rozwiązaniu hybrydowym.
* Komunikacja między składnikami aplikacji rozproszonej działającej lokalnie w różnych organizacjach lub działach organizacji.

Korzystanie z kolejek umożliwia łatwiejsze skalowanie aplikacji i pozwala na większą elastyczność architektury.


