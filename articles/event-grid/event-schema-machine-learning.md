---
title: Azure Machine Learning jako źródło Event Grid
description: Opisuje właściwości, które są dostępne dla zdarzeń Obszar roboczy usługi Machine Learning z Azure Event Grid
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: fb8cd76829622962b642580bbda7f2a655604c2f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2020
ms.locfileid: "87458046"
---
# <a name="azure-machine-learning-as-an-event-grid-source"></a>Azure Machine Learning jako źródło Event Grid

Ten artykuł zawiera właściwości i schemat zdarzeń obszaru roboczego usługi Machine Learning. Aby zapoznać się z wprowadzeniem do schematów zdarzeń, zobacz [Azure Event Grid schemacie zdarzeń](event-schema.md).

## <a name="event-grid-event-schema"></a>Schemat zdarzeń usługi Event Grid

### <a name="available-event-types"></a>Dostępne typy zdarzeń

Azure Machine Learning emituje następujące typy zdarzeń:

| Typ zdarzenia | Opis |
| ---------- | ----------- |
| Microsoft. MachineLearningServices. ModelRegistered | Uruchamiany, gdy nowy model lub wersja modelu zostały pomyślnie zarejestrowane. |
| Microsoft. MachineLearningServices. ModelDeployed | Uruchamiany, gdy modele zostały pomyślnie wdrożone w punkcie końcowym. |
| Microsoft. MachineLearningServices. RunCompleted | Uruchamiany, gdy przebieg został pomyślnie ukończony. |
| Microsoft. MachineLearningServices. DatasetDriftDetected | Uruchamiany, gdy monitor dryfowania zestawu danych wykryje dryf. |
| Microsoft. MachineLearningServices. RunStatusChanged | Uruchamiany, gdy zmieni się stan uruchomienia. |

### <a name="the-contents-of-an-event-response"></a>Zawartość odpowiedzi na zdarzenie

Po wyzwoleniu zdarzenia usługa Event Grid wysyła dane dotyczące tego zdarzenia w celu subskrybowania punktu końcowego.

Ta sekcja zawiera przykład sposobu, w jaki będą wyglądały dane dla każdego zdarzenia.

### <a name="microsoftmachinelearningservicesmodelregistered-event"></a>Zdarzenie Microsoft. MachineLearningServices. ModelRegistered

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "models/sklearn_regression_model:20",
  "eventType": "Microsoft.MachineLearningServices.ModelRegistered",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ModelName": "sklearn_regression_model",
    "ModelVersion": 20,
    "ModelTags": {
        "area": "diabetes",
        "type": "regression"
    },
    "ModelProperties": {
        "type": "test"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesmodeldeployed-event"></a>Zdarzenie Microsoft. MachineLearningServices. ModelDeployed

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "endpoints/my-sklearn-service",
  "eventType": "Microsoft.MachineLearningServices.ModelDeployed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ServiceName": "my-sklearn-service",
    "ServiceComputeType": "ACI",
    "ModelIds": "sklearn_regression_model:1,sklearn_regression_model:2",
    "ServiceTags": {
        "area": "diabetes",
        "type": "regression"
    },
    "ServiceProperties": {
        "type": "test"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesruncompleted-event"></a>Zdarzenie Microsoft. MachineLearningServices. RunCompleted

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "experiments/0fa9dfaa-cba3-4fa7-b590-23e48548f5c1/runs/AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
  "eventType": "Microsoft.MachineLearningServices.RunCompleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "experimentId": "0fa9dfaa-cba3-4fa7-b590-23e48548f5c1",
    "experimentName": "automl-local-regression",
    "runId": "AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
    "runType": null,
    "runTags": {},
    "runProperties": {
        "runTemplate": "automl_child",
        "pipeline_id": "5adc0a4fe02504a586f09a4fcbb241f9a4012062",
        "pipeline_spec": "{\"objects\": [{\"class_name\": \"StandardScaler\", \"module\": \"sklearn.preprocessing\", \"param_args\": [], \"param_kwargs\": {\"with_mean\": true, \"with_std\": false}, \"prepared_kwargs\": {}, \"spec_class\": \"preproc\"}, {\"class_name\": \"LassoLars\", \"module\": \"sklearn.linear_model\", \"param_args\": [], \"param_kwargs\": {\"alpha\": 0.001, \"normalize\": true}, \"prepared_kwargs\": {}, \"spec_class\": \"sklearn\"}], \"pipeline_id\": \"5adc0a4fe02504a586f09a4fcbb241f9a4012062\"}",
        "training_percent": "100",
        "predicted_cost": "0.062226144097381045",
        "iteration": "5",
        "run_template": "automl_child",
        "run_preprocessor": "StandardScalerWrapper",
        "run_algorithm": "LassoLars",
        "conda_env_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/conda_env_v_1_0_0.yml",
        "model_name": "AutoMLad912b2d65",
        "scoring_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/scoring_file_v_1_0_0.py",
        "model_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/model.pkl"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesdatasetdriftdetected-event"></a>Zdarzenie Microsoft. MachineLearningServices. DatasetDriftDetected

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "datadrifts/{}/runs/{}",
  "eventType": "Microsoft.MachineLearningServices.DatasetDriftDetected",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "DataDriftId": "01d29aa4-e6a4-470a-9ef3-66660d21f8ef",
    "DataDriftName": "myDriftMonitor",
    "RunId": "01d29aa4-e6a4-470a-9ef3-66660d21f8ef_1571590300380",
    "BaseDatasetId": "3c56d136-0f64-4657-a0e8-5162089a88a3",
    "TargetDatasetId": "d7e74d2e-c972-4266-b5fb-6c9c182d2a74",
    "DriftCoefficient": 0.83503490684792081,
    "StartTime": "2019-07-04T00:00:00+00:00",
    "EndTime": "2019-07-05T00:00:00+00:00"
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesrunstatuschanged-event"></a>Zdarzenie Microsoft. MachineLearningServices. RunStatusChanged

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "experiments/0fa9dfaa-cba3-4fa7-b590-23e48548f5c1/runs/AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
  "eventType": "Microsoft.MachineLearningServices.RunStatusChanged",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "experimentId": "0fa9dfaa-cba3-4fa7-b590-23e48548f5c1",
    "experimentName": "automl-local-regression",
    "runId": "AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
    "runType": null,
    "runTags": {},
    "runProperties": {
        "runTemplate": "automl_child",
        "pipeline_id": "5adc0a4fe02504a586f09a4fcbb241f9a4012062",
        "pipeline_spec": "{\"objects\": [{\"class_name\": \"StandardScaler\", \"module\": \"sklearn.preprocessing\", \"param_args\": [], \"param_kwargs\": {\"with_mean\": true, \"with_std\": false}, \"prepared_kwargs\": {}, \"spec_class\": \"preproc\"}, {\"class_name\": \"LassoLars\", \"module\": \"sklearn.linear_model\", \"param_args\": [], \"param_kwargs\": {\"alpha\": 0.001, \"normalize\": true}, \"prepared_kwargs\": {}, \"spec_class\": \"sklearn\"}], \"pipeline_id\": \"5adc0a4fe02504a586f09a4fcbb241f9a4012062\"}",
        "training_percent": "100",
        "predicted_cost": "0.062226144097381045",
        "iteration": "5",
        "run_template": "automl_child",
        "run_preprocessor": "StandardScalerWrapper",
        "run_algorithm": "LassoLars",
        "conda_env_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/conda_env_v_1_0_0.yml",
        "model_name": "AutoMLad912b2d65",
        "scoring_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/scoring_file_v_1_0_0.py",
        "model_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/model.pkl"
    },
   "runStatus": "failed"
   },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="event-properties"></a>Właściwości zdarzenia

Zdarzenie ma następujące dane najwyższego poziomu:

| Właściwość | Type | Opis |
| -------- | ---- | ----------- |
| temat | ciąg | Pełna ścieżka zasobu do źródła zdarzeń. To pole nie umożliwia zapisu. Ta wartość jest podawana przez usługę Event Grid. |
| subject | ciąg | Zdefiniowana przez wydawcę ścieżka do tematu zdarzenia. |
| eventType | ciąg | Jeden z zarejestrowanych typów zdarzeń dla tego źródła zdarzeń. |
| eventTime | ciąg | Czas generowania zdarzenia na podstawie czasu UTC dostawcy. |
| identyfikator | ciąg | Unikatowy identyfikator zdarzenia. |
| dane | object | Dane zdarzenia magazynu obiektów BLOB. |
| dataVersion | ciąg | Wersja schematu obiektu danych. Wydawca definiuje wersję schematu. |
| metadataVersion | ciąg | Wersja schematu metadanych zdarzenia. Usługa Event Grid definiuje schemat właściwości najwyższego poziomu. Ta wartość jest podawana przez usługę Event Grid. |

Obiekt danych ma następujące właściwości dla każdego typu zdarzenia:

### <a name="microsoftmachinelearningservicesmodelregistered"></a>Microsoft. MachineLearningServices. ModelRegistered

| Właściwość | Type | Opis |
| -------- | ---- | ----------- |
| ModelName | ciąg | Nazwa zarejestrowanego modelu. |
| ModelVersion | ciąg | Wersja zarejestrowanego modelu. |
| ModelTags | object | Tagi modelu, który został zarejestrowany. |
| ModelProperties | object | Właściwości modelu, który został zarejestrowany. |

### <a name="microsoftmachinelearningservicesmodeldeployed"></a>Microsoft. MachineLearningServices. ModelDeployed

| Właściwość | Type | Opis |
| -------- | ---- | ----------- |
| ServiceName | ciąg | Nazwa wdrożonej usługi. |
| Servicecomputetype | ciąg | Typ obliczeń (np. ACI, AKS) wdrożonej usługi. |
  | ModelIds | ciąg | Rozdzielana przecinkami lista identyfikatorów modeli. Identyfikatory modeli wdrożonych w usłudze. |
| Tagi | object | Tagi wdrożonej usługi. |
| Właściwości serviceproperties | object | Właściwości wdrożonej usługi. |

### <a name="microsoftmachinelearningservicesruncompleted"></a>Microsoft. MachineLearningServices. RunCompleted

| Właściwość | Type | Opis |
| -------- | ---- | ----------- |
| experimentId | ciąg | Identyfikator eksperymentu, do którego należy uruchomienie. |
| eksperymentname | ciąg | Nazwa eksperymentu, do którego należy uruchomienie. |
| runId | ciąg | Identyfikator przebiegu, który został ukończony. |
| runType | ciąg | Typ uruchomienia ukończonego przebiegu. |
| runTags | object | Tagi ukończonego przebiegu. |
| runProperties | object | Właściwości ukończonego uruchomienia. |

### <a name="microsoftmachinelearningservicesdatasetdriftdetected"></a>Microsoft. MachineLearningServices. DatasetDriftDetected

| Właściwość | Type | Opis |
| -------- | ---- | ----------- |
| DataDriftId | ciąg | Identyfikator monitora dryfowania danych, który wyzwolił zdarzenie. |
| Datadryfname | ciąg | Nazwa monitora dryfowania danych, który wyzwolił zdarzenie. |
| RunId | ciąg | Identyfikator uruchomienia, który wykrył dryf danych. |
| BaseDatasetId | ciąg | Identyfikator podstawowego zestawu danych używanego do wykrywania dryfu. |
| TargetDatasetId | ciąg | Identyfikator docelowego zestawu danych używanego do wykrywania dryfu. |
| DriftCoefficient | double | Wynik współczynnika, który wyzwolił zdarzenie. |
| StartTime | datetime | Godzina rozpoczęcia serii czasu docelowej zestawu danych, która spowodowała wykrywanie dryfu.  |
| EndTime | datetime | Godzina zakończenia serii czasu docelowej zestawu danych, która spowodowała wykrywanie dryfu. |

### <a name="microsoftmachinelearningservicesrunstatuschanged"></a>Microsoft. MachineLearningServices. RunStatusChanged

| Właściwość | Type | Opis |
| -------- | ---- | ----------- |
| experimentId | ciąg | Identyfikator eksperymentu, do którego należy uruchomienie. |
| eksperymentname | ciąg | Nazwa eksperymentu, do którego należy uruchomienie. |
| runId | ciąg | Identyfikator przebiegu, który został ukończony. |
| runType | ciąg | Typ uruchomienia ukończonego przebiegu. |
| runTags | object | Tagi ukończonego przebiegu. |
| runProperties | object | Właściwości ukończonego uruchomienia. |
| runStatus | ciąg | Stan uruchomienia. |

## <a name="tutorials-and-how-tos"></a>Samouczki i poradniki
| Tytuł | Opis |
| ----- | ----- |
| [Korzystanie z zdarzeń Azure Machine Learning](../machine-learning/how-to-use-event-grid.md) | Omówienie integrowania Azure Machine Learning z Event Grid. |

## <a name="next-steps"></a>Następne kroki

* Aby zapoznać się z wprowadzeniem do Azure Event Grid, zobacz [co to jest Event Grid?](overview.md)
* Aby uzyskać więcej informacji na temat tworzenia subskrypcji Azure Event Grid, zobacz [Event Grid schematu subskrypcji](subscription-creation-schema.md)
* Aby zapoznać się z wprowadzeniem do korzystania z Azure Event Grid z Azure Machine Learning, zobacz temat [Korzystanie z zdarzeń Azure Machine Learning](../machine-learning/how-to-use-event-grid.md)
* Przykład korzystania z Azure Event Grid z Azure Machine Learning można znaleźć w temacie [Tworzenie przepływów pracy uczenia maszynowego](../machine-learning/how-to-use-event-grid.md) na podstawie zdarzeń
