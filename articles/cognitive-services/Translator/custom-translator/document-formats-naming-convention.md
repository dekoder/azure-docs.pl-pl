---
title: Formaty dokumentów i konwencje nazewnictwa — translator niestandardowy
titleSuffix: Azure Cognitive Services
description: Jest to przewodnik dotyczący formatów dokumentów i konwencji nazewnictwa w Translatoru niestandardowym. Pojęcie to ułatwia zarządzanie nazwami dokumentów i unikanie konfliktów nazw.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 08/17/2020
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 46a19cd4396e598a74f9099727ac80d44c0a44f5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2020
ms.locfileid: "88507250"
---
# <a name="document-formats-and-naming-convention-guidance"></a>Wskazówki dotyczące formatów dokumentów i konwencji nazewnictwa

Każdy plik używany do tłumaczenia niestandardowego musi mieć co najmniej **cztery** znaki.

Ta tabela zawiera wszystkie obsługiwane formaty plików, których można użyć do kompilowania systemu Tłumaczenia:

| Format            | Rozszerzenia   | Opis                                                                                                                                                                                                                                                                    |
|-------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| XLIFF             | . XLF, . XLIFF | Format dokumentu równoległego, eksportowanie systemów pamięci translacji. Używane języki są zdefiniowane w pliku.                                                                                                                                                              |
| TMX               | . TMX         | Format dokumentu równoległego, eksportowanie systemów pamięci translacji. Używane języki są zdefiniowane w pliku.                                                                                                                                                              |
| KODU               | . KODU         | ZIP to format pliku archiwum.                                                                                                                                                                                                        |
| Locstudio         | . LCL         | Format Microsoft dla dokumentów równoległych                                                                                                                                                                                                                                      |
| Microsoft Word    | . DOCX        | Dokument programu Microsoft Word                                                                                                                                                                                                                                                        |
| Program Adobe Acrobat     | . FORMATACH         | Przenośny dokument programu Adobe Acrobat                                                                                                                                                                                                                                                |
| HTML              | . HTML,. HTM  | Dokument HTML                                                                                                                                                                                                                                                                  |
| Plik tekstowy         | . ZAWIERAJĄCEGO         | Pliki tekstowe kodowane w formacie UTF-16 lub UTF-8. Nazwa pliku nie może zawierać znaków japońskich.                                                                                                                                                                                        |
| Wyrównany plik tekstowy | . DOSTOSOWANIA       | Rozszerzenie `.ALIGN` jest specjalnym rozszerzeniem, którego można użyć, Jeśli wiesz, że zdania w parze dokumentu są idealnie wyrównane. Jeśli postanowisz `.ALIGN` plik, translator niestandardowy nie będzie wyrównuje zdań. |
| Plik programu Excel        | . XLSX        | Plik programu Excel (2013 lub nowszy). Pierwszym wierszem/wierszem arkusza kalkulacyjnego powinien być kod języka.                                                                                                                                                                                                                                                      |

## <a name="dictionary-formats"></a>Formaty słownika

W przypadku słowników translator niestandardowy obsługuje wszystkie formaty plików, które są obsługiwane w przypadku zestawów szkoleniowych. Jeśli używasz słownika programu Excel, pierwszym wierszem/wierszem arkusza kalkulacyjnego powinien być kod języka.

## <a name="zip-file-formats"></a>Formaty plików zip

Dokumenty mogą być pogrupowane w jednym pliku zip i przekazywane. Niestandardowa aplikacja obsługuje formaty plików zip (ZIP, GZ i TGZ).

Każdy dokument w pliku zip z rozszerzeniem TXT, HTML, HTM, PDF, DOCX, ALIGN musi być zgodny z tą konwencją nazewnictwa:

{Nazwa dokumentu} \_ {Language Code}, gdzie {Document Name} jest nazwą dokumentu, {language Code} to ISO LanguageID (dwa znaki) wskazujący, że dokument zawiera zdania w tym języku. Przed kodem języka musi istnieć podkreślenie (_).

Na przykład aby przekazać dwa dokumenty równoległe w pliku ZIP dla języka angielskiego do hiszpańskiego, pliki powinny mieć nazwę "data_en" i "data_es".

Pliki pamięci translacji (TMX, XLF, XLIFF, LCL, XLSX) nie są wymagane do przestrzegania określonej konwencji nazewnictwa języka.  

## <a name="next-steps"></a>Następne kroki

- Przeczytaj o [projekcie](workspace-and-project.md#what-is-a-custom-translator-project) , aby utworzyć i zarządzać nimi.
