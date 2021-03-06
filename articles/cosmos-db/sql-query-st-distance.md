---
title: ST_DISTANCE w języku zapytań Azure Cosmos DB
description: Dowiedz się więcej na temat funkcji systemu SQL ST_DISTANCE w Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 11/23/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 736cf488efbdc095823a1fa603f191442d907de3
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/24/2020
ms.locfileid: "96004470"
---
# <a name="st_distance-azure-cosmos-db"></a>ST_DISTANCE (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Zwraca odległość między dwoma wyrażeniami GEOJSON, Wielokąt, MultiPolygon lub LineString. Aby dowiedzieć się więcej, zobacz artykuł [dane lokalizacji geograficznej i GEOJSON](sql-query-geospatial-intro.md) .
  
## <a name="syntax"></a>Składnia
  
```sql
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Argumenty
  
*spatial_expr*  
   Jest dowolnym prawidłowym wyrażeniem GEOJSON, wielokąta lub obiektem LineString.  
  
## <a name="return-types"></a>Typy zwracane
  
  Zwraca wyrażenie liczbowe zawierające odległość. Jest to wyrażane w metrach dla domyślnego systemu referencyjnego.  
  
## <a name="examples"></a>Przykłady
  
  Poniższy przykład pokazuje, jak zwrócić wszystkie dokumenty rodziny, które znajdują się w zakresie 30 km od określonej lokalizacji przy użyciu `ST_DISTANCE` wbudowanej funkcji. .  
  
```sql
SELECT f.id
FROM Families f
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 Tutaj znajduje się zestaw wyników.  
  
```json
[{  
  "id": "WakefieldFamily"  
}]  
```

## <a name="remarks"></a>Uwagi

Ta funkcja systemowa będzie korzystać z [indeksu geoprzestrzennego](index-policy.md#spatial-indexes).

## <a name="next-steps"></a>Następne kroki

- [Funkcje przestrzenne Azure Cosmos DB](sql-query-spatial-functions.md)
- [Azure Cosmos DB funkcje systemowe](sql-query-system-functions.md)
- [Wprowadzenie do usługi Azure Cosmos DB](introduction.md)
