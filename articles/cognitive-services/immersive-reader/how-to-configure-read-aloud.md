---
title: Konfigurowanie czytania na głos
titleSuffix: Azure Cognitive Services
description: W tym artykule opisano sposób konfigurowania różnych opcji odczytywania na głos.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: metang
ms.openlocfilehash: 648227521e5e4e8feecd864d3e572a4758e551ca
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2020
ms.locfileid: "92633267"
---
# <a name="how-to-configure-read-aloud"></a>Jak skonfigurować odczytywanie na głos

W tym artykule pokazano, jak skonfigurować różne opcje odczytu na głos w czytniku immersyjny.

## <a name="automatically-start-read-aloud"></a>Automatycznie Rozpocznij odczytywanie na głos

`options`Parametr zawiera wszystkie flagi, których można użyć do skonfigurowania odczytywania na głos. Ustaw `autoplay` , aby `true` włączyć automatyczne Rozpoczynanie odczytywania na głos po uruchomieniu czytnika immersyjny.

```typescript
const options = {
    readAloudOptions: {
        autoplay: true
    }
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

> [!NOTE]
> Ze względu na ograniczenia przeglądarki przeglądarka Safari nie obsługuje automatycznego odtwarzania.

## <a name="configure-the-voice"></a>Konfigurowanie głosu

Ustaw wartość `voice` `male` lub `female` . Nie wszystkie języki obsługują oba głosy. Aby uzyskać więcej informacji, zobacz stronę [Obsługa języka](./language-support.md) .

```typescript
const options = {
    readAloudOptions: {
        voice: 'female'
    }
};
```

## <a name="configure-playback-speed"></a>Konfigurowanie szybkości odtwarzania

Ustaw `speed` liczbę z zakresu od `0.5` (50%) i `2.5` (250%) Pobiera. Wartości spoza tego zakresu zostaną załączone do 0,5 lub 2,5.

```typescript
const options = {
    readAloudOptions: {
        speed: 1.5
    }
};
```

## <a name="next-steps"></a>Następne kroki

* Poznaj [Kompendium zestawu SDK czytnika immersyjny](./reference.md)