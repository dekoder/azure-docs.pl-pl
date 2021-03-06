---
title: dołączanie pliku
description: dołączanie pliku
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 10/21/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 083ab61d5a20bfb8e38747ae0694b1176c0a0fd1
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2020
ms.locfileid: "92521541"
---
1. Otwórz witrynę [Azure Portal](https://portal.azure.com). Przejdź do maszyny wirtualnej, z którą chcesz nawiązać połączenie, a następnie wybierz pozycję **Połącz**. Wybierz pozycję **bastionu** z listy rozwijanej.

   :::image type="content" source="./media/bastion-vm-rdp/connect-vm.png" alt-text="Wybierz bastionu":::

1. Po wybraniu bastionu z listy rozwijanej zostanie wyświetlony pasek boczny z trzema kartami: RDP, SSH i bastionu. Ponieważ bastionu został zainicjowany dla sieci wirtualnej, karta bastionu jest domyślnie aktywna. Wybierz pozycję **Użyj bastionu**.

   :::image type="content" source="./media/bastion-vm-rdp/select-use-bastion.png" alt-text="Wybierz bastionu":::

1. Na stronie **nawiązywanie połączenia przy użyciu usługi Azure bastionu** wprowadź nazwę użytkownika i hasło dla maszyny wirtualnej, a następnie wybierz pozycję **Połącz**.

   :::image type="content" source="./media/bastion-vm-rdp/connect-vm-host.png" alt-text="Wybierz bastionu":::

1. Połączenie RDP z tą maszyną wirtualną za pośrednictwem bastionu zostanie otwarte bezpośrednio w Azure Portal (za pośrednictwem HTML5) przy użyciu portu 443 i usługi bastionu.

   :::image type="content" source="./media/bastion-vm-rdp/connection.png" alt-text="Wybierz bastionu":::
