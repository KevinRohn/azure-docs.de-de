---
title: Primärschlüssel, Fremdschlüssel und eindeutiger Schlüssel
description: Unterstützung von Tabellenconstraints im Synapse SQL-Pool in Azure Synapse Analytics
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 09/05/2019
ms.author: xiaoyul
ms.reviewer: nibruno; jrasnick
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: f97163d02836442430037e18439bcf0724046332
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80990768"
---
# <a name="primary-key-foreign-key-and-unique-key-in-synapse-sql-pool"></a>Primärschlüssel, Fremdschlüssel und eindeutiger Schlüssel im Synapse SQL-Pool

Hier erfahren Sie mehr über Tabellenconstraints im Synapse SQL-Pool, einschließlich Primärschlüssel, Fremdschlüssel und eindeutigem Schlüssel.

## <a name="table-constraints"></a>Tabellenconstraints

Der Synapse SQL-Pool unterstützt die folgenden Tabellenconstraints: 
- PRIMARY KEY wird nur unterstützt, wenn sowohl NONCLUSTERED als auch NOT ENFORCED verwendet werden.    
- Der UNIQUE-Constraint wird nur unterstützt, wenn NOT ENFORCED verwendet wird.

Informationen zur Syntax finden Sie unter [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) und [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse). 

Der FOREIGN KEY-Constraint wird im Synapse SQL-Pool nicht unterstützt.  


## <a name="remarks"></a>Bemerkungen

Mit einem Primärschlüssel und/oder einem eindeutigen Schlüssel kann die Synapse SQL-Pool-Engine einen optimalen Ausführungsplan für eine Abfrage erstellen.  Alle Werte in einer Primärschlüsselspalte oder einer Spalte für einen eindeutigen Constraint müssen eindeutig sein.

Nach dem Erstellen einer Tabelle mit Hauptschlüssel oder eindeutigem Constraint im Synapse SQL-Pool müssen Benutzer sicherstellen, dass alle Werte in den betreffenden Spalten eindeutig sind.  Eine Verletzung dieser Bedingung kann dazu führen, dass die Abfrage ungenaue Ergebnisse zurückgibt.  Dieses Beispiel zeigt, wie eine Abfrage ungenaue Ergebnisse zurückgeben kann, falls die Hauptschlüsselspalte oder die Spalte mit dem Eindeutig-Contraint doppelte Werte enthält.  

```sql
 -- Create table t1
CREATE TABLE t1 (a1 INT NOT NULL, b1 INT) WITH (DISTRIBUTION = ROUND_ROBIN)

-- Insert values to table t1 with duplicate values in column a1.
INSERT INTO t1 VALUES (1, 100)
INSERT INTO t1 VALUES (1, 1000)
INSERT INTO t1 VALUES (2, 200)
INSERT INTO t1 VALUES (3, 300)
INSERT INTO t1 VALUES (4, 400)

-- Run this query.  No primary key or unique constraint.  4 rows returned. Correct result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
1           2
2           1
3           1
4           1

(4 rows affected)
*/

-- Add unique constraint
ALTER TABLE t1 ADD CONSTRAINT unique_t1_a1 unique (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Incorrect result.
SELECT a1, count(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
4           1
1           1
3           1
1           1

(5 rows affected)
*/

-- Drop unique constraint.
ALTER TABLE t1 DROP CONSTRAINT unique_t1_a1

-- Add primary key constraint
ALTER TABLE t1 add CONSTRAINT PK_t1_a1 PRIMARY KEY NONCLUSTERED (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Incorrect result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
4           1
1           1
3           1
1           1

(5 rows affected)
*/

-- Manually fix the duplicate values in a1
UPDATE t1 SET a1 = 0 WHERE b1 = 1000

-- Verify no duplicate values in column a1 
SELECT * FROM t1

/*
a1          b1
----------- -----------
2           200
3           300
4           400
0           1000
1           100

(5 rows affected)
*/

-- Add unique constraint
ALTER TABLE t1 add CONSTRAINT unique_t1_a1 UNIQUE (a1) NOT ENFORCED  

-- Re-run this query.  5 rows returned.  Correct result.
SELECT a1, COUNT(*) as total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
3           1
4           1
0           1
1           1

(5 rows affected)
*/

-- Drop unique constraint.
ALTER TABLE t1 DROP CONSTRAINT unique_t1_a1

-- Add primary key contraint
ALTER TABLE t1 ADD CONSTRAINT PK_t1_a1 PRIMARY KEY NONCLUSTERED (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Correct result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
3           1
4           1
0           1
1           1

(5 rows affected)
*/

```

## <a name="examples"></a>Beispiele

Erstellen einer Synapse SQL-Pooltabelle mit einem Primärschlüssel: 

```sql 
CREATE TABLE mytable (c1 INT PRIMARY KEY NONCLUSTERED NOT ENFORCED, c2 INT);
```
Erstellen einer Synapse SQL-Pooltabelle mit einem eindeutigen Constraint:

```sql
CREATE TABLE t6 (c1 INT UNIQUE NOT ENFORCED, c2 INT);
```

## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen der Tabellen für den Synapse SQL-Pool werden im nächsten Schritt Daten in die Tabelle geladen. Ein Tutorial zum Laden von Daten finden Sie unter [Laden von Daten in den Synapse SQL-Pool](load-data-wideworldimportersdw.md).
