---
title: Obsługiwane funkcje platformy Azure Application Insights Azure Functions
description: Application Insights obsługiwane funkcje Azure Functions
ms.topic: reference
author: TimothyMothra
ms.author: tilee
ms.date: 4/23/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 36c5fc93886327c0e3261418343d900ee66cb4eb
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2020
ms.locfileid: "92168583"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Application Insights Azure Functions obsługiwanych funkcji

Azure Functions oferuje [wbudowaną integrację](../../azure-functions/functions-monitoring.md) z usługą Application Insights, która jest dostępna za pomocą interfejsu ILogger. Poniżej znajduje się lista obecnie obsługiwanych funkcji. Zapoznaj się z tematem "Przewodnik po [rozpoczęciu](../../azure-functions/configure-monitoring.md#enable-application-insights-integration)Azure Functions".

Aby uzyskać więcej informacji na temat wersji środowiska uruchomieniowego usługi Functions, zobacz [tutaj](../../azure-functions/functions-versions.md).

Aby uzyskać więcej informacji na temat zgodnych wersji Application Insights, zobacz [zależności](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/).

## <a name="supported-features"></a>Obsługiwane funkcje

| Azure Functions                       | Wersja 1                | WERSJA 2 & V3   | 
|-----------------------------------    |---------------    |------------------ |
| | | | 
| **Automatyczne zbieranie**        |                 |                   |               
| &bull; Żądań                     | Tak             | Tak               | 
| &bull; Wyłączenia                   | Tak             | Tak               | 
| &bull; Liczniki wydajności         | Tak             | Tak               |
| &bull; Tamten                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; PROTOKOŁY      |                 | Tak               | 
| &nbsp;&nbsp;&nbsp;&mdash; ServiceBus|                 | Tak               | 
| &nbsp;&nbsp;&nbsp;&mdash; EventHub  |                 | Tak               | 
| &nbsp;&nbsp;&nbsp;&mdash; Server       |                 | Tak               | 
| | | | 
| **Obsługiwane funkcje**                |                   |                   |               
| &bull; QuickPulse/LiveMetrics       | Tak             | Tak               | 
| &nbsp;&nbsp;&nbsp;&mdash; Bezpieczny kanał kontroli|                 | Tak               | 
| &bull; Sond                     | Tak             | Tak               | 
| &bull; Pulsów                   |                 | Tak               | 
| | | | 
| **Korelacja**                       |                   |                   |               
| &bull; ServiceBus                     |                   | Tak               | 
| &bull; EventHub                       |                   | Tak               | 
| | | | 
| **Konfigurowalne**                      |                   |                   |           
| &bull;W pełni konfigurowalne.<br/>Aby uzyskać instrukcje, zobacz [Azure Functions](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) .<br/>Więcej opcji można znaleźć w [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) .               |                   | Tak                   | 


## <a name="performance-counters"></a>Liczniki wydajności

Automatyczne zbieranie liczników wydajności działa tylko na maszynach z systemem Windows.


## <a name="live-metrics--secure-control-channel"></a>Metryki na żywo & kanale bezpiecznego sterowania

Określone kryteria filtrów niestandardowych są wysyłane z powrotem do składnika metryki na żywo w zestawie Application Insights SDK. Filtry mogą potencjalnie zawierać informacje poufne, takie jak customerID. Kanał można zabezpieczyć przy użyciu klucza tajnego interfejsu API. Instrukcje można znaleźć w temacie [Zabezpieczenia kanału kontroli](./live-stream.md#secure-the-control-channel) .

## <a name="sampling"></a>Próbkowanie

Azure Functions domyślnie włącza próbkowanie w ich konfiguracji. Aby uzyskać więcej informacji, zobacz [Konfigurowanie próbkowania](../../azure-functions/configure-monitoring.md#configure-sampling).

Jeśli projekt przyjmuje zależność od zestawu SDK Application Insights do ręcznego śledzenia danych telemetrycznych, może wystąpić dziwne zachowanie, jeśli konfiguracja próbkowania różni się od konfiguracji próbkowania funkcji. 

Zalecamy używanie tej samej konfiguracji co funkcje. Z **funkcjami w wersji 2**można uzyskać tę samą konfigurację przy użyciu iniekcji zależności w konstruktorze:

```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Extensibility;

public class Function1 
{

    private readonly TelemetryClient telemetryClient;

    public Function1(TelemetryConfiguration configuration)
    {
        this.telemetryClient = new TelemetryClient(configuration);
    }

    [FunctionName("Function1")]
    public async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, ILogger logger)
    {
        this.telemetryClient.TrackTrace("C# HTTP trigger function processed a request.");
    }
}
```
