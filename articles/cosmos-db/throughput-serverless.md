---
title: Jak wybrać między zainicjowaną przepływność a bezserwerową Azure Cosmos DB
description: Dowiedz się, jak wybrać między zainicjowaną przepływność a bezserwerowym obciążeniem.
author: ThomasWeiss
ms.author: thweiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/25/2020
ms.openlocfilehash: 1943aae3a2b01490dca687bcdea99d76da238d51
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/26/2020
ms.locfileid: "96187259"
---
# <a name="how-to-choose-between-provisioned-throughput-and-serverless"></a>Jak wybrać między zainicjowaną przepływność a bezserwerową
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Cosmos DB jest dostępny w dwóch różnych trybach pojemności: [zainicjowana przepływność](set-throughput.md) i [bezserwerowy](serverless.md). Można wykonać dokładnie te same operacje bazy danych w obu trybach, ale sposób rozliczania dla tych operacji jest radykalnie różny. W poniższym filmie wideo objaśniono podstawowe różnice między tymi trybami i jak dopasowują się do różnych typów obciążeń:

> [!VIDEO https://www.youtube.com/embed/CgYQo6uHyt0]

## <a name="detailed-comparison"></a>Szczegółowe porównanie

| Kryteria | Aprowizowana przepływność | Praca bezserwerowa |
| --- | --- | --- |
| Stan | Ogólnie dostępne | W wersji zapoznawczej |
| Najlepiej dopasowane do | Obciążenia o krytycznym znaczeniu wymagające przewidywalnej wydajności | Małe i średnie obciążenia niekrytyczne z lekkim i sporadycznym ruchem |
| Jak to działa | W przypadku każdego kontenera należy udostępnić pewną ilość przepływności wyrażoną w [jednostkach żądań](request-units.md) na sekundę. Co sekundę ta liczba jednostek żądania jest dostępna dla operacji bazy danych. Tymczasową przepływność można aktualizować ręcznie lub automatycznie korygować przy użyciu funkcji automatycznego [skalowania](provision-throughput-autoscale.md). | Uruchamiasz operacje bazy danych względem kontenerów bez konieczności aprowizacji żadnej pojemności. |
| Dystrybucja geograficzna | Dostępne (nieograniczona liczba regionów świadczenia usługi Azure) | Niedostępne (konta bezserwerowe można uruchamiać tylko w 1 regionie platformy Azure) |
| Maksymalny rozmiar magazynu na kontener | Nieograniczona liczba | 50 GB |
| Wydajność | 99,99% do 99,999% dostępności objętej umową SLA<br>< 10 ms opóźnienia do odczytu punktów i zapisów objętych umową SLA<br>99,99% gwarantowaną przepływność objętą umową SLA | 99,9% do 99,99% dostępności objętej umową SLA<br>< 10 ms opóźnienia do odczytu punktów i < 30 ms dla zapisów objętych przez cel SLO<br>95% możliwości przenoszenia objętej umową SLO |
| Model rozliczania | Opłaty są naliczane za godzinę, dla których Zainicjowano obsługę RU, niezależnie od liczby użytych jednostek ru. | Opłaty są naliczane za godzinę dla ilości jednostek ru zużytych przez operacje bazy danych. |

> [!IMPORTANT]
> Niektóre ograniczenia bezserwerowe mogą być rozładowaniu lub usuwane, gdy bezserwerowy staną się ogólnie dostępne i **Twoje opinie** pomogą nam podjąć decyzję! Skontaktuj się z nami i przekaż nam więcej informacji na temat środowiska bezserwerowego: [azurecosmosdbserverless@service.microsoft.com](mailto:azurecosmosdbserverless@service.microsoft.com) .

## <a name="estimating-your-expected-consumption"></a>Szacowanie oczekiwanego użycia

W niektórych sytuacjach może zostać wyczyszczone, czy dla danego obciążenia należy wybrać zainicjowaną przepływność lub bezserwerowy. Aby uzyskać pomoc dotyczącą tej decyzji, możesz oszacować ogólne, **przewidywane użycie**, czyli całkowitą liczbę jednostek ruów, które mogą zużywać w ciągu miesiąca (można oszacować za pomocą tabeli pokazanej w [tym miejscu](plan-manage-costs.md#estimating-serverless-costs)).

**Przykład 1**: oczekiwano obciążenia na maksymalnie 500 ru/s i zużywamy łącznie 20 000 000 jednostek RU w ciągu miesiąca.

- W trybie elastycznej przepływności należy udostępnić kontener z 500 RU/s dla miesięcznego kosztu: $0,008 * 5 * 730 = **$29,20**
- W trybie bezserwerowym płacisz za zużytą jednostek ru: $0,25 * 20 = **$5,00**

**Przykład 2**: oczekiwano obciążenia na maksymalnie 500 ru/s i zużywamy łącznie 250 000 000 jednostek RU w ciągu miesiąca.

- W trybie elastycznej przepływności należy udostępnić kontener z 500 RU/s dla miesięcznego kosztu: $0,008 * 5 * 730 = **$29,20**
- W trybie bezserwerowym płacisz za zużytą jednostek ru: $0,25 * 250 = **$62,50**

(te przykłady nie dotyczą kosztu magazynu, który jest taki sam jak w obu trybach)

> [!NOTE]
> Koszty pokazane w poprzednim przykładzie służą wyłącznie do celów demonstracyjnych. Aby uzyskać najnowsze informacje o cenach, zobacz [stronę z cennikiem](https://azure.microsoft.com/pricing/details/cosmos-db/) .

## <a name="next-steps"></a>Następne kroki

- Przeczytaj więcej [na temat przepływności aprowizacji na Azure Cosmos DB](set-throughput.md)
- Przeczytaj więcej o [Azure Cosmos DB bezserwerowym](serverless.md)
- Zapoznaj się z koncepcją [jednostek żądania](request-units.md)
