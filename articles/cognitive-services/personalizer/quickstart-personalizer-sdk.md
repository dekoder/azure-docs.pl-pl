---
title: 'Szybki Start: Tworzenie i używanie pętli szkoleniowej z zestawem SDK — Personalizacja'
description: Ten przewodnik Szybki Start przedstawia sposób tworzenia bazy wiedzy i zarządzania nią przy użyciu biblioteki klienta narzędzia Personalizacja.
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: quickstart
ms.date: 08/27/2020
ms.custom: cog-serv-seo-aug-2020
keywords: Personalizacja, Personalizacja platformy Azure, uczenie maszynowe
zone_pivot_groups: programming-languages-set-six
ms.openlocfilehash: 3acfd8134a2a65e6ea9f019baff24b8b8149f7f3
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/06/2020
ms.locfileid: "91777328"
---
# <a name="quickstart-personalizer-client-library"></a>Szybki Start: Biblioteka kliencka personalizacji

Wyświetl spersonalizowaną zawartość w tym przewodniku szybki start za pomocą usługi personalizacji.

Rozpocznij pracę z biblioteką klienta personalizacji. Wykonaj następujące kroki, aby zainstalować pakiet i wypróbować przykładowy kod dla podstawowych zadań.

 * Ranga API — wybiera najlepszy element z akcji, na podstawie informacji w czasie rzeczywistym dostarczanych na temat zawartości i kontekstu.
 * Interfejs API nagradzania — możesz określić wynik nagrody w zależności od potrzeb firmy, a następnie wysłać go do personalizacji przy użyciu tego interfejsu API. Ten wynik może być pojedynczą wartością, taką jak 1 dla dobra, i 0 dla uszkodzonych lub algorytmem tworzonym w zależności od potrzeb firmy.

::: zone pivot="programming-language-csharp"
[!INCLUDE [Get intent with C# SDK](./includes/quickstart-sdk-csharp.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [Get intent with Node.js SDK](./includes/quickstart-sdk-nodejs.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Get intent with Python SDK](./includes/quickstart-sdk-python.md)]
::: zone-end

## <a name="clean-up-resources"></a>Czyszczenie zasobów

Jeśli chcesz wyczyścić i usunąć subskrypcję Cognitive Services, możesz usunąć zasób lub grupę zasobów. Usunięcie grupy zasobów spowoduje również usunięcie wszystkich skojarzonych z nią zasobów.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Interfejs wiersza polecenia platformy Azure](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Jak działa](how-personalizer-works.md) 
>  Personalizacja [Gdzie używać personalizacji?](where-can-you-use-personalizer.md)
