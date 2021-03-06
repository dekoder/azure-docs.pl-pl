---
title: Jak wdrożyć szablon aplikacji IoT Central analiza filmów wideo — obiekt i ruch
description: Ten przewodnik zawiera podsumowanie kroków związanych z wdrażaniem aplikacji IoT Central platformy Azure przy użyciu szablonu wideo Analiza obiektów i funkcji wykrywania ruchu.
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: how-to
ms.author: nandab
author: KishorIoT
ms.date: 07/31/2020
ms.openlocfilehash: decfa7020be7778e8ca64a9fb0cb4aac1657da27
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91873340"
---
# <a name="how-to-deploy-an-iot-central-application-using-the-video-analytics---object-and-motion-detection-application-template"></a>Jak wdrożyć aplikację IoT Central przy użyciu szablonu wideo Analiza obiektów i wykrywania ruchu

Aby zapoznać się z omówieniem składników aplikacji Key *Analytics — obiekty i wykrywanie ruchu* , zobacz [Architektura aplikacji Analiza wideo dotycząca obiektów i ruchu](architecture-video-analytics.md).

Poniższy film wideo zawiera Przewodnik dotyczący sposobu używania _szablonu aplikacji do analizy wideo i wykrywania ruchu_ w celu wdrożenia rozwiązania IoT Central:

> [!VIDEO https://www.youtube.com/embed/Bo3FziU9bSA]

## <a name="deploy-the-application"></a>Wdrażanie aplikacji

Wykonaj następujące kroki, aby wdrożyć aplikację IoT Central przy użyciu szablonu aplikacji wideo Analytics:

1. Wypełnij instrukcje [tworzenia aplikacji do analizy filmów wideo w usłudze azure IoT Central (Yolo v3)](tutorial-video-analytics-create-app-yolo-v3.md) lub samouczka [Tworzenie wideo Analytics w usłudze Azure IoT Central (OpenVINO &trade; )](tutorial-video-analytics-create-app-openvino.md) , aby:
    - Tworzenie konta usługi Azure Media Services.
    - Utwórz aplikację IoT Central przy użyciu szablonu wideo Analiza obiektów i wykrywania ruchu.
    - Skonfiguruj urządzenie bramy w aplikacji IoT Central. Brama umożliwia urządzeniom kamer łączenie się z aplikacją.

1. Ukończ [Tworzenie wystąpienia IoT Edge na potrzeby analizy filmów wideo (maszyna wirtualna z systemem Linux)](tutorial-video-analytics-iot-edge-vm.md) lub [samouczka: Utwórz wystąpienie IoT Edge dla programu Video Analytics (Intel NUC)](tutorial-video-analytics-iot-edge-nuc.md) , aby:
    - Utwórz maszynę wirtualną platformy Azure z zainstalowanym środowiskiem uruchomieniowym Azure IoT Edge.-Przygotuj instalację IoT Edge do hostowania modułu wideo Analytics.
    - Podłącz urządzenie IoT Edge do aplikacji IoT Central.

1. Wykonaj czynności [monitorowane i Zarządzaj samouczkiem aplikacji wideo Analytics](tutorial-video-analytics-manage.md) , aby:
    - Dodaj aparaty wykrywania obiektów i ruchu do bramy w aplikacji IoT Central.
    - Rozpocznij przetwarzanie aparatu.
    - Zainstaluj lokalny odtwarzacz multimedialny, aby wyświetlić przechwycone wideo w AMS.
    - Wyświetl przechwycone wideo pokazujące wykryte obiekty.
    - Uporządkowanego.

## <a name="next-steps"></a>Następne kroki

Teraz możesz zapoznać się z instrukcjami wdrażania i używania szablonu aplikacji do analizy wideo, zobacz [Tworzenie aplikacji do analizy wideo na platformie azure IoT Central (Yolo v3)](tutorial-video-analytics-create-app-yolo-v3.md) lub [Tworzenie analiz wideo na platformie Azure IoT Central (OpenVINO &trade; )](tutorial-video-analytics-create-app-openvino.md) , aby rozpocząć pracę.
