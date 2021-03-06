---
title: Tworzenie zależności zadań do uruchamiania zadań
description: Utwórz zadania, które zależą od ukończenia innych zadań przetwarzania stylu MapReduce i podobnych obciążeń danych Big Data w Azure Batch.
ms.topic: how-to
ms.date: 05/22/2017
ms.custom: H1Hack27Feb2017, devx-track-csharp
ms.openlocfilehash: 05b1bb289c215208be448d8ca7de144c82b313b8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2020
ms.locfileid: "88936983"
---
# <a name="create-task-dependencies-to-run-tasks-that-depend-on-other-tasks"></a>Tworzenie zależności zadań w celu uruchamiania zadań zależnych od innych zadań

Można zdefiniować zależności zadań do uruchamiania zadania lub zestawu zadań dopiero po zakończeniu zadania nadrzędnego. Niektóre scenariusze, w których są przydatne zależności zadań, obejmują:

- Obciążenia w stylu MapReduce w chmurze.
- Zadania, których zadania przetwarzania danych mogą być wyrażone jako ukierunkowane wykresy acykliczne (DAG).
- Procesy przed renderowaniem i po renderingu, gdzie każde zadanie musi zakończyć się przed rozpoczęciem następnego zadania.
- Wszystkie inne zadania, w których zadania podrzędne są zależne od danych wyjściowych zadań nadrzędnych.

Za pomocą zależności zadań wsadowych można tworzyć zadania zaplanowane do wykonania w węzłach obliczeniowych po zakończeniu jednego lub kilku zadań nadrzędnych. Można na przykład utworzyć zadanie, które renderuje każdą ramkę filmu 3W z oddzielnymi zadaniami równoległymi. Zadanie końcowe — "zadanie scalania" — Scala renderowane ramki do kompletnego filmu dopiero po pomyślnym wyrenderowaniu wszystkich ramek.

Domyślnie zadania zależne są planowane do wykonania dopiero po pomyślnym ukończeniu zadania nadrzędnego. Można określić akcję zależności, aby zastąpić zachowanie domyślne i uruchamiać zadania, gdy zadanie nadrzędne zakończy się niepowodzeniem. Aby uzyskać szczegółowe informacje, zobacz sekcję [Akcje zależności](#dependency-actions) .  

Można tworzyć zadania, które są zależne od innych zadań w relacji jeden-do-jednego lub jeden-do-wielu. Można również utworzyć zależność zakresu, w którym zadanie zależy od ukończenia grupy zadań w określonym zakresie identyfikatorów zadań. Te trzy podstawowe scenariusze można połączyć, aby utworzyć relacje wiele-do-wielu.

## <a name="task-dependencies-with-batch-net"></a>Współzależności zadań z usługą Batch .NET

W tym artykule omówiono sposób konfigurowania zależności zadań przy użyciu biblioteki usługi Batch dla [platformy .NET][net_msdn] . Najpierw pokazano, jak [włączyć zależność zadań](#enable-task-dependencies) względem zadań, a następnie zademonstrować sposób [konfigurowania zadania z zależnościami](#create-dependent-tasks). Opisano również sposób określania akcji zależności do uruchamiania zadań zależnych w przypadku niepowodzenia elementu nadrzędnego. Na koniec omówiono [scenariusze zależności](#dependency-scenarios) obsługiwane przez partię.

## <a name="enable-task-dependencies"></a>Włącz zależności zadań

Aby korzystać z zależności zadań w aplikacji wsadowej, należy najpierw skonfigurować zadanie tak, aby korzystało z zależności zadań. W usłudze Batch .NET Włącz ją na [CloudJob][net_cloudjob] , ustawiając jej właściwość [UsesTaskDependencies][net_usestaskdependencies] na `true` :

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

W poprzednim fragmencie kodu "batchClient" jest wystąpieniem klasy [batchClient][net_batchclient] .

## <a name="create-dependent-tasks"></a>Tworzenie zadań zależnych

Aby utworzyć zadanie, które zależy od ukończenia jednego lub kilku zadań nadrzędnych, można określić, że zadanie "zależy od" innych zadań. W usłudze Batch .NET Skonfiguruj [CloudTask][net_cloudtask]. Właściwość [DependsOn][net_dependson] z wystąpieniem klasy [TaskDependencies][net_taskdependencies] :

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Ten fragment kodu tworzy zadanie zależne z IDENTYFIKATORem zadania "kwiaty". Zadanie "kwiaty" zależy od zadań "deszcz" i "Sun". Zadanie "kwiaty" zostanie zaplanowane do uruchomienia w węźle obliczeniowym dopiero po pomyślnym ukończeniu zadań "deszcz" i "Sun".

> [!NOTE]
> Domyślnie zadanie jest uznawane za zakończone pomyślnie, gdy jest w stanie **ukończone** i jego **Kod zakończenia** to `0` . W usłudze Batch .NET oznacza to [CloudTask][net_cloudtask]. Wartość właściwości [State][net_taskstate] `Completed` i [TaskExecutionInformation][net_taskexecutioninformation]CloudTask.[ ][net_exitcode] Wartość właściwości ExitCode to `0` . Aby to zmienić, zobacz sekcję [Akcje zależności](#dependency-actions) .

## <a name="dependency-scenarios"></a>Scenariusze zależności

Istnieją trzy podstawowe scenariusze zależności zadań, których można użyć w Azure Batch: jeden do jednego, jeden-do-wielu i zakres identyfikatorów zadań. Można je łączyć, aby zapewnić czwarty scenariusz, wiele-do-wielu.

| Scenariusz&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Przykład | Ilustracji |
|:---:| --- | --- |
|  [Jeden do jednego](#one-to-one) |*zadaniab* zależy od *zadania* <p/> *zadaniab* nie zostanie zaplanowana do wykonania przed pomyślnym zakończeniem *zadania* |![Diagram: zależność zadania jeden do jednego][1] |
|  [Jeden do wielu](#one-to-many) |*zadaniec* zależy od *zadania* i *zadaniab* <p/> *zadaniec* nie zostanie zaplanowana do wykonania, dopóki *zadania* i *zadaniab* nie zostaną ukończone pomyślnie |![Diagram: zależność zadania jeden-do-wielu][2] |
|  [Zakres identyfikatorów zadań](#task-id-range) |*zadania* podrzędne są zależne od zakresu zadań <p/> *zadanie* nie zostanie zaplanowane do wykonania, dopóki zadania o identyfikatorach od *1* do *10* nie zakończą się pomyślnie |![Diagram: zależność zakresu identyfikatora zadania][3] |

> [!TIP]
> Można utworzyć relacje **wiele-do-wielu** , takie jak zadania C, D, E i F, które są zależne od zadań a i B. Jest to przydatne na przykład w przypadku równoległych scenariuszy przetwarzania wstępnego, w których zadania podrzędne zależą od danych wyjściowych wielu zadań nadrzędnych.
> 
> W przykładach w tej sekcji zadanie zależne jest uruchamiane dopiero po pomyślnym ukończeniu zadań nadrzędnych. To zachowanie jest zachowaniem domyślnym dla zadania zależnego. Zadanie zależne można uruchomić po niepowodzeniu zadania nadrzędnego, określając akcję zależności, która zastąpi zachowanie domyślne. Aby uzyskać szczegółowe informacje, zobacz sekcję [Akcje zależności](#dependency-actions) .

### <a name="one-to-one"></a>Jeden do jednego

W relacji jeden do jednego zadanie jest zależne od pomyślnego ukończenia jednego zadania nadrzędnego. Aby utworzyć zależność, podaj jeden identyfikator zadania do [TaskDependencies][net_taskdependencies]. Metoda statyczna [OnId][net_onid] po wypełnieniu właściwości [DependsOn][net_dependson] elementu [CloudTask][net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>Jeden do wielu

W relacji jeden do wielu zadanie zależy od ukończenia wielu zadań nadrzędnych. Aby utworzyć zależność, podaj kolekcję identyfikatorów zadań do [TaskDependencies][net_taskdependencies]. Metoda statyczna [OnIds][net_onids] po wypełnieniu właściwości [DependsOn][net_dependson] elementu [CloudTask][net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
``` 

### <a name="task-id-range"></a>Zakres identyfikatorów zadań

W zależności od zakresu zadań nadrzędnych zadanie zależy od ukończenia zadań, których identyfikatory mieszczą się w zakresie.
Aby utworzyć zależność, podaj pierwsze i ostatnie identyfikatory zadań w zakresie do [TaskDependencies][net_taskdependencies]. Metoda statyczna [OnIdRange][net_onidrange] po wypełnieniu właściwości [DependsOn][net_dependson] elementu [CloudTask][net_cloudtask].

> [!IMPORTANT]
> W przypadku korzystania z zakresów identyfikatorów zadań w zależności od zakresu będą wybierane tylko zadania z identyfikatorami reprezentującymi wartości całkowite. Dlatego zakres `1..10` będzie wybierać zadania `3` i `7` , ale nie `5flamingoes` . 
>
> Zera wiodące nie są istotne podczas oceniania zależności zakresu, dlatego zadania z identyfikatorami `4` ciągów `04` i `004` wszystkie będą znajdować się *w* zakresie, a wszystkie będą traktowane jako zadania `4` , więc pierwszy z nich będzie spełniał zależność.
>
> Każde zadanie w zakresie musi spełniać zależność, przez pomyślne zakończenie lub przez zakończenie z powodu błędu zamapowanego na akcję zależności ustawioną na **spełnienie**. Aby uzyskać szczegółowe informacje, zobacz sekcję [Akcje zależności](#dependency-actions) .

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="dependency-actions"></a>Akcje zależności

Domyślnie zależne zadanie lub zestaw zadań jest uruchamiany dopiero po pomyślnym ukończeniu zadania nadrzędnego. W niektórych scenariuszach można uruchamiać zadania zależne nawet wtedy, gdy zadanie nadrzędne zakończy się niepowodzeniem. Zachowanie domyślne można przesłonić, określając akcję zależności. Akcja zależności określa, czy zadanie zależne jest uprawnione do uruchomienia, na podstawie sukcesu lub niepowodzenia zadania nadrzędnego. 

Załóżmy na przykład, że zadanie zależne oczekuje na dane po zakończeniu zadania nadrzędnego. Jeśli zadanie nadrzędne zakończy się niepowodzeniem, zadanie zależne może nadal być uruchamiane przy użyciu starszych danych. W takim przypadku akcja zależności może określać, że zadanie zależne jest uprawnione do uruchomienia niezależnie od błędu zadania nadrzędnego.

Akcja zależności jest oparta na warunku wyjścia zadania nadrzędnego. Można określić akcję zależności dla dowolnego z następujących warunków wyjścia; Aby uzyskać szczegółowe informacje na temat platformy .NET, zobacz [ExitConditions][net_exitconditions] klasy:

- Gdy wystąpi błąd podczas przetwarzania wstępnego.
- Gdy wystąpi błąd przekazywania pliku. Jeśli zadanie zostanie zakończone z kodem zakończenia, który został określony za pośrednictwem **exitCodes** lub **exitCodeRanges**, a następnie napotka błąd przekazywania pliku, Akcja określona przez kod zakończenia ma pierwszeństwo.
- Gdy zadanie zostanie zakończone z kodem zakończenia zdefiniowanym przez właściwość **ExitCodes** .
- Gdy zadanie zostanie zakończone z kodem zakończenia, który mieści się w zakresie określonym przez właściwość **ExitCodeRanges** .
- Przypadek domyślny, jeśli zadanie zostanie zakończone z kodem zakończenia niezdefiniowanym przez **ExitCodes** lub **ExitCodeRanges**, lub jeśli zadanie zakończy się z błędem przetwarzania wstępnego, a właściwość **PreProcessingError** nie została ustawiona lub jeśli zadanie zakończy się niepowodzeniem z błędem przekazywania pliku i Właściwość **FileUploadError** nie jest ustawiona. 

Aby określić akcję zależności w programie .NET, należy ustawić [ExitOptions][net_exitoptions]. Właściwość [DependencyAction][net_dependencyaction] dla warunku zakończenia. Właściwość **DependencyAction** przyjmuje jedną z dwóch wartości:

- Ustawienie właściwości **DependencyAction** na wartość **spełnia** wskazuje, że zależne zadania są uprawnione do uruchamiania, jeśli zadanie nadrzędne zostanie zakończone z określonym błędem.
- Ustawienie właściwości **DependencyAction** na **Block** wskazuje, że zadania zależne nie mogą być uruchamiane.

Ustawienie domyślne właściwości **DependencyAction** jest **zgodne** dla kodu zakończenia 0 i **bloku** dla wszystkich pozostałych warunków zakończenia.

Poniższy fragment kodu ustawia właściwość **DependencyAction** dla zadania nadrzędnego. Jeśli zadanie nadrzędne zostanie zakończone z błędem przetwarzania wstępnego lub z określonymi kodami błędów, zadanie zależne zostanie zablokowane. Jeśli zadanie nadrzędne zostanie zakończone z jakimkolwiek innym błędem o wartości innej niż zero, zadanie zależne może zostać uruchomione.

```csharp
// Task A is the parent task.
new CloudTask("A", "cmd.exe /c echo A")
{
    // Specify exit conditions for task A and their dependency actions.
    ExitConditions = new ExitConditions
    {
        // If task A exits with a pre-processing error, block any downstream tasks (in this example, task B).
        PreProcessingError = new ExitOptions
        {
            DependencyAction = DependencyAction.Block
        },
        // If task A exits with the specified error codes, block any downstream tasks (in this example, task B).
        ExitCodes = new List<ExitCodeMapping>
        {
            new ExitCodeMapping(10, new ExitOptions() { DependencyAction = DependencyAction.Block }),
            new ExitCodeMapping(20, new ExitOptions() { DependencyAction = DependencyAction.Block })
        },
        // If task A succeeds or fails with any other error, any downstream tasks become eligible to run 
        // (in this example, task B).
        Default = new ExitOptions
        {
            DependencyAction = DependencyAction.Satisfy
        }
    }
},
// Task B depends on task A. Whether it becomes eligible to run depends on how task A exits.
new CloudTask("B", "cmd.exe /c echo B")
{
    DependsOn = TaskDependencies.OnId("A")
},
```

## <a name="code-sample"></a>Przykład kodu

Przykładowy projekt [TaskDependencies][github_taskdependencies] jest jednym z [przykładów kodu Azure Batch][github_samples] w witrynie GitHub. To rozwiązanie programu Visual Studio ilustruje:

- Jak włączyć zależność zadań względem zadania
- Tworzenie zadań, które są zależne od innych zadań
- Sposób wykonywania tych zadań w puli węzłów obliczeniowych.

## <a name="next-steps"></a>Następne kroki

- Funkcja [pakietów aplikacji](batch-application-packages.md) usługi Batch umożliwia łatwe wdrażanie i przechowywanie aplikacji wykonywanych przez zadania w węzłach obliczeniowych.
- Zapoznaj się z tematem [Instalowanie aplikacji i danych przemieszczania w węzłach obliczeniowych usługi Batch][forum_post] na forum Azure Batch, aby zapoznać się z omówieniem metod przygotowywania węzłów do uruchamiania zadań. Na podstawie jednego z członków zespołu Azure Batch ten wpis jest dobrym systemem na różne sposoby kopiowania aplikacji, danych wejściowych zadań i innych plików do węzłów obliczeniowych.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: /dotnet/api/microsoft.azure.batch.batchclient
[net_cloudjob]: /dotnet/api/microsoft.azure.batch.cloudjob
[net_cloudtask]: /dotnet/api/microsoft.azure.batch.cloudtask
[net_dependson]: /dotnet/api/microsoft.azure.batch.cloudtask
[net_exitcode]: /dotnet/api/microsoft.azure.batch.taskexecutioninformation
[net_exitconditions]: /dotnet/api/microsoft.azure.batch.exitconditions
[net_exitoptions]: /dotnet/api/microsoft.azure.batch.exitoptions
[net_dependencyaction]: /dotnet/api/microsoft.azure.batch.exitoptions
[net_msdn]: /dotnet/api/microsoft.azure.batch
[net_onid]: /dotnet/api/microsoft.azure.batch.taskdependencies
[net_onids]: /dotnet/api/microsoft.azure.batch.taskdependencies
[net_onidrange]: /dotnet/api/microsoft.azure.batch.taskdependencies
[net_taskexecutioninformation]: /dotnet/api/microsoft.azure.batch.taskexecutioninformation
[net_taskstate]: /dotnet/api/microsoft.azure.batch.common.taskstate
[net_usestaskdependencies]: /dotnet/api/microsoft.azure.batch.cloudjob
[net_taskdependencies]: /dotnet/api/microsoft.azure.batch.taskdependencies

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagram: zależność jeden do jednego"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagram: zależność jeden do wielu"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagram: zależność zakresu identyfikatora zadania"
