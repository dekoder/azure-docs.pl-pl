---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.custom: devx-track-javascript
ms.openlocfilehash: dfca8bb6f897f532081df7d141112ddbb190d109
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87406552"
---
[!INCLUDE [Prerequisites](prerequisites-nodejs.md)]

[!INCLUDE [Setup and use environment variables](setup-env-variables.md)]

## <a name="create-a-project-and-import-required-modules"></a>Tworzenie projektu i importowanie wymaganych modułów

Utwórz nowy projekt przy użyciu ulubionego środowiska IDE lub edytora. Następnie skopiuj ten fragment kodu do swojego projektu do pliku o nazwie `detect.js`.

```javascript
const request = require('request');
const uuidv4 = require('uuid/v4');
```

> [!NOTE]
> Jeśli nie korzystano z tych modułów, konieczne będzie ich zainstalowanie przed uruchomieniem programu. Aby zainstalować te pakiety, uruchom polecenie `npm install request uuidv4`.

Te moduły są wymagane do utworzenia żądania HTTP i unikatowego identyfikatora dla nagłówka `'X-ClientTraceId'`.

## <a name="set-the-subscription-key-and-endpoint"></a>Ustawianie klucza subskrypcji i punktu końcowego

Ten przykład spróbuje odczytać klucz subskrypcji usługi Translator i punkt końcowy z tych zmiennych środowiskowych: `TRANSLATOR_TEXT_SUBSCRIPTION_KEY` i `TRANSLATOR_TEXT_ENDPOINT` . Jeśli nie znasz zmiennych środowiskowych, możesz ustawić `subscriptionKey` i `endpoint` jako ciągi i dodać komentarz do instrukcji warunkowych.

Skopiuj ten kod do projektu:

```javascript
var key_var = 'TRANSLATOR_TEXT_SUBSCRIPTION_KEY';
if (!process.env[key_var]) {
    throw new Error('Please set/export the following environment variable: ' + key_var);
}
var subscriptionKey = process.env[key_var];
var endpoint_var = 'TRANSLATOR_TEXT_ENDPOINT';
if (!process.env[endpoint_var]) {
    throw new Error('Please set/export the following environment variable: ' + endpoint_var);
}
var endpoint = process.env[endpoint_var];
```

## <a name="configure-the-request"></a>Konfigurowanie żądania

Metoda `request()`, udostępniona przez moduł żądania, umożliwia nam przekazanie metody HTTP, parametrów żądania adresu URL, nagłówków i treści w formacie JSON w postaci obiektu `options`. W poniższym fragmencie kodu skonfigurujemy żądanie:

```javascript
let options = {
    method: 'POST',
    baseUrl: endpoint,
    url: 'detect',
    qs: {
      'api-version': '3.0',
    },
    headers: {
      'Ocp-Apim-Subscription-Key': subscriptionKey,
      'Content-type': 'application/json',
      'X-ClientTraceId': uuidv4().toString()
    },
    body: [{
          'text': 'Salve, mondo!'
    }],
    json: true,
};
```
Najprostszym sposobem uwierzytelniania żądania jest przekazanie klucza subskrypcji jako nagłówka `Ocp-Apim-Subscription-Key`. Ta metoda jest używana w tym przykładzie. Alternatywnie można wymienić klucz subskrypcji na token dostępu i przekazać go dalej jako nagłówek `Authorization` w celu zweryfikowania żądania.

Jeśli używasz subskrypcji usługi Cognitive Services, musisz także uwzględnić `Ocp-Apim-Subscription-Region` w nagłówkach żądania.

Aby uzyskać więcej informacji, zobacz [Authentication](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication) (Uwierzytelnianie).

## <a name="make-the-request-and-print-the-response"></a>Wysyłanie żądania i wyświetlanie odpowiedzi

Następnie utworzymy żądanie przy użyciu metody `request()`. Pobiera ona obiekt `options` utworzony w poprzedniej sekcji jako pierwszy argument, a następnie wyświetla ulepszoną odpowiedź w formacie JSON.

```javascript
request(options, function(err, res, body){
    console.log(JSON.stringify(body, null, 4));
});
```

>[!NOTE]
> W tym przykładzie definiujemy żądanie HTTP w obiekcie `options`. Jednak moduł żądania obsługuje również metody ułatwiające, takie jak `.post` i `.get`. Aby uzyskać więcej informacji, zobacz [Convenience methods](https://github.com/request/request#convenience-methods) (Metody ułatwiające).

## <a name="put-it-all-together"></a>Zebranie wszystkich elementów

Jest to również prosty program, który wywoła translator i zwróci odpowiedź JSON. Teraz nadszedł czas, aby uruchomić program:

```console
node detect.js
```

## <a name="sample-response"></a>Przykładowa odpowiedź

Po uruchomieniu przykładu na terminalu powinny zostać wyświetlone następujące elementy:

> [!NOTE]
> Znajdź skrót kraju/regionu na [liście języków](https://docs.microsoft.com/azure/cognitive-services/translator/language-support).

```json
[
    {
        "alternatives": [
            {
                "isTranslationSupported": true,
                "isTransliterationSupported": false,
                "language": "pt",
                "score": 1.0
            },
            {
                "isTranslationSupported": true,
                "isTransliterationSupported": false,
                "language": "en",
                "score": 1.0
            }
        ],
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "it",
        "score": 1.0
    }
]
```

## <a name="clean-up-resources"></a>Czyszczenie zasobów

Jeśli klucz subskrypcji umieszczono na stałe w kodzie programu, pamiętaj, aby usunąć ten klucz subskrypcji po zakończeniu pracy z przewodnikiem Szybki start.

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z dokumentacją interfejsu API, aby zrozumieć wszystko, co możesz zrobić z translatorem.

> [!div class="nextstepaction"]
> [Dokumentacja interfejsu API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
