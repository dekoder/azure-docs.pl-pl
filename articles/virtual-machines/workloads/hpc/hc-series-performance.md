---
title: Wydajność maszyn wirtualnych z serii HC
description: Dowiedz się więcej o wynikach testowania wydajności dla rozmiarów maszyn wirtualnych z serii HC na platformie Azure.
author: vermagit
ms.service: virtual-machines
ms.subservice: workloads
ms.topic: article
ms.date: 09/10/2020
ms.author: amverma
ms.reviewer: cynthn
ms.openlocfilehash: e1abe4b87bd5be98dad8e43d604f833eae3854e7
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2020
ms.locfileid: "94966959"
---
# <a name="hc-series-virtual-machine-sizes"></a>Rozmiary maszyn wirtualnych z serii HC

Kilka testów wydajności zostało uruchomionych w rozmiarach z serii HC. Poniżej przedstawiono niektóre wyniki tego testu wydajnościowego.

| Obciążenie                                        | HB                    |
|-------------------------------------------------|-----------------------|
| Triad strumienia                                    | 190 GB/s (Intel MLC AVX-512)  |
| High-Performance Linpack (HPL)                  | 3520 GigaFLOPS (Rpeak), 2970 GigaFLOPS (RMAX) |
| Opóźnienie RDMA & przepustowość                        | 1,05 mikrosekund, 96,8 GB/s   |
| FIO na lokalnym dysku SSD interfejsu NVMe                           | 1,3 GB/s odczyty, 900 MB/s zapisów |  
| IOR na 4 SSD w warstwie Premium platformy Azure (P30 Managed Disks, RAID0) * *  | 780 MB/s odczyty, 780 MB/zapisy |

## <a name="mpi-latency"></a>Opóźnienie MPI

Test opóźnienia MPI z zestawu OSU mikrotestu jest uruchamiany. Przykładowe skrypty są w serwisie [GitHub](https://github.com/Azure/azhpc-images/blob/04ddb645314a6b2b02e9edb1ea52f079241f1297/tests/run-tests.sh)

```bash
./bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./osu_latency 
```

:::image type="content" source="./media/latency-hc.png" alt-text="MPI opóźnienie na platformie Azure HC.":::

## <a name="mpi-bandwidth"></a>MPI przepustowość

Test przepustowości MPI z zestawu OSU mikrotestu jest uruchamiany. Przykładowe skrypty są w serwisie [GitHub](https://github.com/Azure/azhpc-images/blob/04ddb645314a6b2b02e9edb1ea52f079241f1297/tests/run-tests.sh)

```bash
./mvapich2-2.3.install/bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./mvapich2-2.3/osu_benchmarks/mpi/pt2pt/osu_bw
```

:::image type="content" source="./media/bandwidth-hc.png" alt-text="MPI przepustowość na platformie Azure HC.":::


## <a name="mellanox-perftest"></a>Perftest Mellanox

[Pakiet Mellanox Perftest](https://community.mellanox.com/s/article/perftest-package) ma wiele testów InfiniBand, takich jak opóźnienie (ib_send_lat) i przepustowość (ib_send_bw). Przykładowe polecenie jest poniżej.

```console
numactl --physcpubind=[INSERT CORE #]  ib_send_lat -a
```

## <a name="next-steps"></a>Następne kroki

- Zapoznaj się z najnowszymi powiadomieniami i niektórymi przykładami obliczeń o wysokiej wydajności (HPC) i z wynikami na [blogach społecznościowych obliczeń na platformie Azure](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute).
- Aby zapoznać się z ogólnym widokiem architektury uruchamiania obciążeń HPC, zobacz [wysoka wydajność obliczeń (HPC) na platformie Azure](/azure/architecture/topics/high-performance-computing/).
