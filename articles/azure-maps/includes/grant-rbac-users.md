---
title: Udzielanie użytkownikom dostępu opartego na rolach
titleSuffix: Azure Maps
description: Korzystanie z kontroli dostępu opartej na rolach w celu przyznania użytkownikom autoryzacji do Azure Maps
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/17/2020
ms.topic: include
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 19806fe24d0ff3b87ebe61b45ac302947c734fa0
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/28/2020
ms.locfileid: "92895572"
---
## <a name="grant-role-based-access-for-users-to-azure-maps"></a>Przyznaj użytkownikom dostęp oparty na rolach, aby Azure Maps

*Kontrolę dostępu opartą na rolach (Azure RBAC)* można przypisywać do co najmniej jednej roli Azure Maps lub grupy zabezpieczeń usługi Azure AD. Aby wyświetlić definicje ról platformy Azure, które są dostępne dla Azure Maps, przejdź do obszaru **Kontrola dostępu (IAM)** . Wybierz pozycję **role** , a następnie wyszukaj role zaczynające się od *Azure Maps* .

* Aby efektywnie zarządzać dużą ilością dostępu użytkowników do Azure Maps, zobacz [grupy usługi Azure AD](../../active-directory/fundamentals/active-directory-manage-groups.md).
* Aby użytkownicy mogli uwierzytelniać się w aplikacji, użytkownicy muszą być utworzeni w usłudze Azure AD. Zobacz [Dodawanie lub usuwanie użytkowników przy użyciu usługi Azure AD](../../active-directory/fundamentals/add-users-azure-active-directory.md).

Przeczytaj więcej na temat [usługi Azure AD](../../active-directory/fundamentals/index.yml) , aby efektywnie zarządzać katalogiem dla użytkowników.

1. Przejdź do swojego **konta Azure Maps** . Wybierz pozycję **Kontrola dostępu (IAM)**  >  **przypisanie roli** .

    ![Udzielanie dostępu przy użyciu funkcji RBAC platformy Azure](../media/how-to-manage-authentication/how-to-grant-rbac.png)

2. Na karcie **przypisania roli** w obszarze **rola** wybierz wbudowaną definicję roli Azure Maps, na przykład **Azure Maps czytnika danych** lub **Azure Maps współautor danych** . W obszarze **Przypisywanie dostępu do** wybierz pozycję **użytkownik, Grupa lub nazwa główna usługi Azure AD** . Wybierz nazwę główną według nazwy. Następnie wybierz pozycję **Zapisz** .

   * Zobacz szczegóły dotyczące [dodawania lub usuwania przypisań ról](../../role-based-access-control/role-assignments-portal.md).

> [!WARNING]
> Azure Maps wbudowane definicje ról zapewniają bardzo duże uprawnienia dostępu do wielu Azure Maps interfejsów API REST. Aby ograniczyć interfejsy API dla użytkowników do minimum, zobacz [Tworzenie niestandardowej definicji roli i przypisywanie użytkowników](../../role-based-access-control/custom-roles.md) do definicji roli niestandardowej. Pozwoli to użytkownikom na posiadanie najniższych uprawnień niezbędnych dla aplikacji.