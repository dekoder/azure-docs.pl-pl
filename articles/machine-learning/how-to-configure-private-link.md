---
title: Skonfiguruj prywatny punkt końcowy
titleSuffix: Azure Machine Learning
description: Użyj prywatnego linku platformy Azure, aby bezpiecznie uzyskać dostęp do obszaru roboczego Azure Machine Learning z sieci wirtualnej.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to, devx-track-azurecli
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 09/30/2020
ms.openlocfilehash: 2953f85a5c21cdd670d6e133d09ffacf06f178ef
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/18/2020
ms.locfileid: "94842706"
---
# <a name="configure-azure-private-link-for-an-azure-machine-learning-workspace"></a>Konfigurowanie prywatnego linku platformy Azure dla obszaru roboczego Azure Machine Learning

W tym dokumencie dowiesz się, jak korzystać z prywatnego linku platformy Azure z obszarem roboczym Azure Machine Learning. Aby uzyskać informacje na temat tworzenia sieci wirtualnej dla Azure Machine Learning, zobacz [Omówienie izolacji i prywatności w sieci wirtualnej](how-to-network-security-overview.md)

Link prywatny platformy Azure umożliwia nawiązanie połączenia z obszarem roboczym przy użyciu prywatnego punktu końcowego. Prywatny punkt końcowy to zestaw prywatnych adresów IP w sieci wirtualnej. Następnie można ograniczyć dostęp do obszaru roboczego tylko w przypadku prywatnych adresów IP. Link prywatny pomaga ograniczyć ryzyko związane z eksfiltracji danych. Aby dowiedzieć się więcej o prywatnych punktach końcowych, zobacz artykuł [prywatny link do platformy Azure](../private-link/private-link-overview.md) .

> [!IMPORTANT]
> Łącze prywatne platformy Azure nie wpływa na płaszczyznę kontroli platformy Azure (operacje zarządzania), takie jak usuwanie obszaru roboczego lub zarządzanie zasobami obliczeniowymi. Na przykład tworzenie, aktualizowanie lub usuwanie elementu docelowego obliczeń. Te operacje są wykonywane za pośrednictwem publicznej sieci Internet jako normalne. Operacje płaszczyzny danych, takie jak używanie Azure Machine Learning Studio, interfejsów API (w tym opublikowanych potoków) lub zestawu SDK korzystają z prywatnego punktu końcowego.
>
> W przypadku korzystania z przeglądarki Mozilla Firefox mogą wystąpić problemy podczas próby uzyskania dostępu do prywatnego punktu końcowego dla obszaru roboczego. Ten problem może dotyczyć systemu DNS za pośrednictwem protokołu HTTPS w Mozilla. Zalecamy korzystanie z przeglądarki Google Chrome firmy Microsoft jako obejście problemu.

## <a name="prerequisites"></a>Wymagania wstępne

Jeśli planujesz używanie obszaru roboczego z włączoną obsługą linku prywatnego z kluczem zarządzanym przez klienta, musisz zażądać tej funkcji przy użyciu biletu pomocy technicznej. Aby uzyskać więcej informacji, zobacz [Zarządzanie i zwiększanie limitów przydziału](how-to-manage-quotas.md#private-endpoint-and-private-dns-quota-increases).

## <a name="limitations"></a>Ograniczenia

Korzystanie z obszaru roboczego Azure Machine Learning z linkiem prywatnym nie jest dostępne w regionach Azure Government ani w regionach 21Vianet platformy Azure w Chinach.

## <a name="create-a-workspace-that-uses-a-private-endpoint"></a>Tworzenie obszaru roboczego korzystającego z prywatnego punktu końcowego

Użyj jednej z następujących metod, aby utworzyć obszar roboczy z prywatnym punktem końcowym. Każda z tych metod __Wymaga istniejącej sieci wirtualnej__:

> [!TIP]
> Jeśli chcesz utworzyć obszar roboczy, prywatny punkt końcowy i sieć wirtualną w tym samym czasie, zobacz [Tworzenie obszaru roboczego dla Azure Machine Learning za pomocą szablonu Azure Resource Manager](how-to-create-workspace-template.md).

# <a name="python"></a>[Python](#tab/python)

Zestaw Azure Machine Learning Python SDK udostępnia klasę [PrivateEndpointConfig](/python/api/azureml-core/azureml.core.privateendpointconfig?view=azure-ml-py) , która może być używana z elementem [Workspace. Create ()](/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---tags-none--friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--adb-workspace-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--private-endpoint-config-none--private-endpoint-auto-approval-true--exist-ok-false--show-output-true-) w celu utworzenia obszaru roboczego z prywatnym punktem końcowym. Ta klasa wymaga istniejącej sieci wirtualnej.

```python
from azureml.core import Workspace
from azureml.core import PrivateEndPointConfig

pe = PrivateEndPointConfig(name='myprivateendpoint', vnet_name='myvnet', vnet_subnet_name='default')
ws = Workspace.create(name='myworkspace',
    subscription_id='<my-subscription-id>',
    resource_group='myresourcegroup',
    location='eastus2',
    private_endpoint_config=pe,
    private_endpoint_auto_approval=True,
    show_output=True)
```

# <a name="azure-cli"></a>[Interfejs wiersza polecenia platformy Azure](#tab/azure-cli)

[Rozszerzenie interfejsu wiersza polecenia platformy Azure dla usługi Machine Learning](reference-azure-machine-learning-cli.md) udostępnia polecenie [AZ ml Workspace Create](/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext_azure_cli_ml_az_ml_workspace_create) . Następujące parametry dla tego polecenia mogą służyć do tworzenia obszaru roboczego z siecią prywatną, ale wymaga istniejącej sieci wirtualnej:

* `--pe-name`: Nazwa tworzonego prywatnego punktu końcowego.
* `--pe-auto-approval`: Czy połączenia prywatnego punktu końcowego z obszarem roboczym powinny być automatycznie zatwierdzane.
* `--pe-resource-group`: Grupa zasobów, w której ma zostać utworzony prywatny punkt końcowy. Musi być tą samą grupą, która zawiera sieć wirtualną.
* `--pe-vnet-name`: Istniejąca sieć wirtualna do utworzenia prywatnego punktu końcowego w programie.
* `--pe-subnet-name`: Nazwa podsieci, w której ma zostać utworzony prywatny punkt końcowy. Wartość domyślna to `default`.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Karta __Sieć__ w programie Azure Machine Learning Studio umożliwia skonfigurowanie prywatnego punktu końcowego. Jednak wymaga istniejącej sieci wirtualnej. Aby uzyskać więcej informacji, zobacz [Tworzenie obszarów roboczych w portalu](how-to-manage-workspace.md).

---

## <a name="add-a-private-endpoint-to-a-workspace"></a>Dodawanie prywatnego punktu końcowego do obszaru roboczego

Aby dodać prywatny punkt końcowy do istniejącego obszaru roboczego, użyj jednej z następujących metod:

> [!IMPORTANT]
>
> Aby utworzyć prywatny punkt końcowy w programie, musisz mieć istniejącą sieć wirtualną. Przed dodaniem prywatnego punktu końcowego należy również [wyłączyć zasady sieciowe dla prywatnych punktów końcowych](../private-link/disable-private-endpoint-network-policy.md) .

> [!WARNING]
>
> Jeśli masz jakieś istniejące obiekty docelowe obliczeń skojarzone z tym obszarem roboczym i nie są one za tą samą siecią wirtualną, określona prywatny punkt końcowy jest tworzony w programie, nie będą one działały.

# <a name="python"></a>[Python](#tab/python)

```python
from azureml.core import Workspace
from azureml.core import PrivateEndPointConfig

pe = PrivateEndPointConfig(name='myprivateendpoint', vnet_name='myvnet', vnet_subnet_name='default')
ws = Workspace.from_config()
ws.add_private_endpoint(private_endpoint_config=pe, private_endpoint_auto_approval=True, show_output=True)
```

Aby uzyskać więcej informacji na temat klas i metod używanych w tym przykładzie, zobacz [PrivateEndpointConfig](/python/api/azureml-core/azureml.core.privateendpointconfig?view=azure-ml-py) i [Workspace.add_private_endpoint](/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#add-private-endpoint-private-endpoint-config--private-endpoint-auto-approval-true--location-none--show-output-true--tags-none-).

# <a name="azure-cli"></a>[Interfejs wiersza polecenia platformy Azure](#tab/azure-cli)

[Rozszerzenie interfejsu wiersza polecenia platformy Azure dla usługi Machine Learning](reference-azure-machine-learning-cli.md) udostępnia polecenie [AZ ml Workspace Private-Endpoint Add](/cli/azure/ext/azure-cli-ml/ml/workspace/private-endpoint?view=azure-cli-latest#ext_azure_cli_ml_az_ml_workspace_private_endpoint_add) .

```azurecli
az ml workspace private-endpoint add -w myworkspace  --pe-name myprivateendpoint --pe-auto-approval true --pe-vnet-name myvnet
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

W obszarze roboczym Azure Machine Learning w portalu wybierz pozycję __połączenia prywatnego punktu końcowego__ , a następnie wybierz pozycję __+ prywatny punkt końcowy__. Użyj pól, aby utworzyć nowy prywatny punkt końcowy.

* W przypadku wybrania __regionu__ wybierz ten sam region, w którym znajduje się Twoja sieć wirtualna. 
* Podczas wybierania __typu zasobu__ Użyj __Microsoft. MachineLearningServices/Workspaces__. 
* Ustaw __zasób__ na nazwę obszaru roboczego.

Na koniec wybierz pozycję __Utwórz__ , aby utworzyć prywatny punkt końcowy.

---

## <a name="remove-a-private-endpoint"></a>Usuwanie prywatnego punktu końcowego

Aby usunąć prywatny punkt końcowy z obszaru roboczego, użyj jednej z następujących metod:

# <a name="python"></a>[Python](#tab/python)

Użyj [Workspace.delete_private_endpoint_connection](/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#delete-private-endpoint-connection-private-endpoint-connection-name-) , aby usunąć prywatny punkt końcowy.

```python
from azureml.core import Workspace

ws = Workspace.from_config()
# get the connection name
_, _, connection_name = ws.get_details()['privateEndpointConnections'][0]['id'].rpartition('/')
ws.delete_private_endpoint_connection(private_endpoint_connection_name=connection_name)
```

# <a name="azure-cli"></a>[Interfejs wiersza polecenia platformy Azure](#tab/azure-cli)

[Rozszerzenie interfejsu wiersza polecenia platformy Azure dla usługi Machine Learning](reference-azure-machine-learning-cli.md) udostępnia polecenie [AZ ml Workspace Private-Endpoint Delete](/cli/azure/ext/azure-cli-ml/ml/workspace/private-endpoint?view=azure-cli-latest#ext_azure_cli_ml_az_ml_workspace_private_endpoint_delete) .

# <a name="portal"></a>[Portal](#tab/azure-portal)

W obszarze roboczym Azure Machine Learning w portalu wybierz pozycję __połączenia prywatnego punktu końcowego__, a następnie wybierz punkt końcowy, który chcesz usunąć. Na koniec wybierz pozycję __Usuń__.

---

## <a name="using-a-workspace-over-a-private-endpoint"></a>Używanie obszaru roboczego w prywatnym punkcie końcowym

Ponieważ komunikacja z obszarem roboczym jest dozwolona tylko w sieci wirtualnej, wszystkie środowiska deweloperskie korzystające z obszaru roboczego muszą należeć do sieci wirtualnej. Na przykład maszyna wirtualna w sieci wirtualnej.

> [!IMPORTANT]
> Aby uniknąć tymczasowego zakłócenia łączności, firma Microsoft zaleca opróżnianie pamięci podręcznej DNS na komputerach łączących się z obszarem roboczym po włączeniu linku prywatnego. 

Aby uzyskać informacje na temat usługi Azure Virtual Machines, zobacz [dokumentację Virtual Machines](../virtual-machines/index.yml).


## <a name="next-steps"></a>Następne kroki

* Aby uzyskać więcej informacji na temat zabezpieczania obszaru roboczego Azure Machine Learning, zobacz artykuł [Omówienie izolacji i prywatności w sieci wirtualnej](how-to-network-security-overview.md) .

* Jeśli planujesz korzystać z niestandardowego rozwiązania DNS w sieci wirtualnej, zobacz [jak używać obszaru roboczego z niestandardowym serwerem DNS](how-to-custom-dns.md).
