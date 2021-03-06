---
author: dominicbetts
ms.author: dobett
ms.service: iot-pnp
ms.topic: include
ms.date: 03/17/2020
ms.custom: references_regions, devx-track-azurecli
ms.openlocfilehash: 3ac72360db5fb3d075b4caa3208f372f22441c7b
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2020
ms.locfileid: "92755858"
---
## <a name="prepare-an-iot-hub"></a>Przygotowywanie Centrum IoT Hub

Aby wykonać kroki opisane w tym artykule, potrzebujesz usługi Azure IoT Hub w ramach subskrypcji platformy Azure. Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

Jeśli używasz interfejsu wiersza polecenia platformy Azure lokalnie, najpierw Zaloguj się do subskrypcji platformy Azure za pomocą programu `az login` . Jeśli uruchamiasz te polecenia w Azure Cloud Shell, nastąpi automatyczne logowanie.

Jeśli używasz interfejsu wiersza polecenia platformy Azure lokalnie, `az` wersja powinna być **2.8.0** lub nowsza, a Azure Cloud Shell używa najnowszej wersji. Użyj `az --version` polecenia, aby sprawdzić wersję zainstalowaną na komputerze.

Uruchom następujące polecenie, aby dodać rozszerzenie IoT Microsoft Azure dla interfejsu wiersza polecenia platformy Azure do wystąpienia:

```azurecli-interactive
az extension add --name azure-iot
```

Jeśli nie masz jeszcze usługi IoT Hub do użycia, uruchom następujące polecenia, aby utworzyć grupę zasobów i bezpłatne centrum IoT w Twojej subskrypcji. Zamień na `<YourIoTHubName>` wybraną nazwę centrum:

```azurecli-interactive
az group create --name my-pnp-resourcegroup \
    --location centralus
az iot hub create --name <YourIoTHubName> \
    --resource-group my-pnp-resourcegroup --sku F1
```

> [!NOTE]
> Plug and Play IoT jest obecnie dostępna dla centrów IoT utworzonych w regionach Środkowe stany USA, Europa Północna i Japonia Wschodnia. Obsługa Plug and Play IoT nie jest uwzględniona w centrach IoT warstwy Podstawowa.

Uruchom następujące polecenie, aby utworzyć tożsamość urządzenia w centrum IoT Hub. Zastąp `<YourIoTHubName>` `<YourDeviceID>` symbole zastępcze i własnymi _nazwami IoT Hub_ i wybranym przez siebie _identyfikatorem urządzenia_ .

```azurecli-interactive
az iot hub device-identity create --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```
