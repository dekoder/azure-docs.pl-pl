---
title: Jak oznaczyć maszynę wirtualną przy użyciu szablonu
description: Dowiedz się więcej na temat tagowania maszyny wirtualnej przy użyciu szablonu.
ms.service: virtual-machines
ms.topic: how-to
ms.workload: infrastructure-services
ms.author: cynthn
author: cynthn
ms.date: 10/26/2018
ms.openlocfilehash: d1acbe82a086574a102e7897bbd3b99683c1185e
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2020
ms.locfileid: "94594982"
---
# <a name="tagging-a-vm-using-a-template"></a>Tagowanie maszyny wirtualnej przy użyciu szablonu


W tym artykule opisano, jak oznaczyć maszynę wirtualną na platformie Azure przy użyciu szablonu Menedżer zasobów. Tagi to zdefiniowane przez użytkownika pary klucz/wartość, które mogą być umieszczone bezpośrednio w ramach zasobu lub grupy zasobów. Platforma Azure obsługuje obecnie do 50 tagów na zasób i grupę zasobów. Tagi mogą być umieszczane na zasobie w momencie tworzenia lub dodawane do istniejącego zasobu.

[Ten szablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) umieszcza znaczniki w następujących zasobach: COMPUTE (maszyna wirtualna), Storage (konto magazynu) i sieć (publiczny adres IP, Virtual Network i interfejs sieciowy). Ten szablon jest przeznaczony dla maszyny wirtualnej z systemem Windows, ale można go dostosować do maszyn wirtualnych z systemem Linux.

Kliknij przycisk **Wdróż na platformie Azure** w [linku szablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags). Spowoduje to przejście do [Azure Portal](https://portal.azure.com/) , w którym można wdrożyć ten szablon.

![Proste wdrożenie ze znacznikami](./media/tag/deploy-to-azure-tags.png)

Ten szablon zawiera następujące znaczniki: *dział* , *aplikacja* i *utworzone przez*. Możesz dodawać lub edytować te Tagi bezpośrednio w szablonie, jeśli chcesz zmienić nazwy tagów.

![Tagi platformy Azure w szablonie](./media/tag/azure-tags-in-a-template.png)

Jak widać, znaczniki są definiowane jako pary klucz/wartość, oddzielone dwukropkiem (:). Tagi muszą być zdefiniowane w tym formacie:

```config
"tags": {
    "Key1" : "Value1",
    "Key2" : "Value2"
}
```

Zapisz plik szablonu po zakończeniu edycji przy użyciu wybranych przez Ciebie tagów.

Następnie w sekcji **Edytuj parametry** można wypełnić wartości dla tagów.

![Edytuj Tagi w Azure Portal](./media/tag/edit-tags-in-azure-portal.png)

Kliknij przycisk **Utwórz** , aby wdrożyć ten szablon z wartościami tagów.


**Następne kroki**

- Aby dowiedzieć się więcej o znakowaniu zasobów platformy Azure, zobacz [Azure Resource Manager omówienie](../azure-resource-manager/management/overview.md) i [Używanie tagów do organizowania zasobów platformy Azure](../azure-resource-manager/management/tag-resources.md).
- Aby dowiedzieć się, jak tagi mogą ułatwić zarządzanie użyciem zasobów platformy Azure, zobacz [Opis rachunku na korzystanie z platformy Azure](../cost-management-billing/understand/review-individual-bill.md) i [Uzyskiwanie szczegółowych informacji o zużyciu zasobów Microsoft Azure](../cost-management-billing/manage/usage-rate-card-overview.md).
