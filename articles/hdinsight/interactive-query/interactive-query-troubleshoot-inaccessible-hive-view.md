---
title: Apache Hive połączeń z platformą Apache dozorcy — Azure HDInsight
description: Niedostępność widoku Apache Hive ze względu na problemy z platformą Apache dozorcy w usłudze Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: 1ac237edeea777225b7cf99946d359c921331b18
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2020
ms.locfileid: "93288917"
---
# <a name="scenario-apache-hive-fails-to-establish-a-connection-to-apache-zookeeper-in-azure-hdinsight"></a>Scenariusz: Apache Hive nie może nawiązać połączenia z platformą Apache dozorcy w usłudze Azure HDInsight

W tym artykule opisano kroki rozwiązywania problemów oraz możliwe rozwiązania problemów występujących w przypadku używania interakcyjnych składników zapytań w klastrach usługi Azure HDInsight.

## <a name="issue"></a>Problem

Widok Hive jest niedostępny, a dzienniki w programie `/var/log/hive` pokazują błąd podobny do poniższego:

```
ERROR [Curator-Framework-0]: curator.ConnectionState (ConnectionState.java:checkTimeouts(200)) - Connection timed out for connection string (zk0-cluster.cloud.wbmi.com:2181,zk1-cluster.cloud.wbmi.com:2181,zk2-cluster.cloud.wbmi.com:2181) and timeout (15000) / elapsed (21852)
```

## <a name="cause"></a>Przyczyna

Istnieje możliwość, że w usłudze Hive nie można nawiązać połączenia z usługą dozorcy, co uniemożliwia uruchomienie widoku programu Hive.

## <a name="resolution"></a>Rozwiązanie

1. Sprawdź, czy usługa dozorcy jest w dobrej kondycji.

1. Sprawdź, czy usługa dozorcy ma wpis ZNode dla elementu Hive serwer2. Brak wartości lub jest ona niepoprawna.

    ```
    /usr/hdp/2.6.2.25-1/zookeeper/bin/zkCli.sh -server zk1-wbwdhs
    [zk: zk0-cluster(CONNECTED) 0] ls /hiveserver2-hive2
    ```

1. Aby ponownie nawiązać połączenie, uruchom ponownie węzły dozorcy i uruchom ponownie serwera hiveserver2.

## <a name="next-steps"></a>Następne kroki

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]