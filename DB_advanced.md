# T-SQL (Microsoft SQL server)
## Pages
Čtení z disku probíhá po stránkách, ne po řádcích - přečtení stránky = logical read (execution plan)
- 1 page => 8KB
- 1 extent => 8 pages (64KB)
- Typy
    - Data Page
        - Struktura
    - Index Page
        - Indexové struktury
    - IAM Page
        - Informace o alokaci
## Clustered index
Fyzické pořadí dat v tabulce
- B+Tree
- Tabulka může mít pouze jeden clustered index
- ``CREATE CLUSTERED INDEX name ON Tabulka(TabulkaID)``
- +Rychlý range scan, order by
- -Můžou vznikat splits
- SSMS vytvoří clustered nad PK
- Nemusí ale být v tabulce

## Nonclustered index
- B+Tree
- Obsahuje
    - Klíč indexu
    - Pointer na data
        - Varianta Clustered index: Pointer = id clustered indexu
        - Varianta Heap: Pointer = RID (1:523:17)
- ``CREATE INDEX name ON Tabulka(Atribute)``
## Heap
- Tabulka bez clustered indexu
- Data jsou ukládaná tam kde je místo
- Index na heap tabulce ukazuje na RID (Row ID) (Vyhledání = RID lookup)
    - FileID
    - PageID
    - SlotID
### Forwarding record
- Pouze u Heapu
- Řádek se zvětší a nevejde se na původní stránku
- Přesune řádek jinam, na původním místě nechá ukazatel na nové místo
## Covering Index
- SQL dokáže vrátít všechny sloupce z indexu, bez přístupu do tabulky
- ``CREATE INDEX name ON Tabulka(Atribute) INCLUDE (Atribute2,Atribute3)``
- Leaf level B+Tree obsahuje i Atributy 2 a 3
    - Nemusí se provádět Key Lookup
- ``ON Tabulka(Atribute) INCLUDE (Atribute2)`` vs ``ON Tabulka(Atribute,Atribute2)``
    - Leaf Nodes stejné, nonleaf nodes u include menší 
- *Key lookup vysoký cost? -> Přidat Include*
## Seek vs Scan
- Index seek
    - Použije index efektivně
    - ``WHERE TableID = 10``
    - LIKE "abc%"
- Index scan
    - Projde celý index
    - Nefektivní
    - ``WHERE YEAR(TableDate) = 2026``
    - LIKE "%abc"
- Table scan
    - Nejhorší
    - Sekvenčne prochází celou tabulku
## SARGability
- SARGable -> Search ARGument Able
- Možnost použít index
- V případě indexu na TableDate
    - ``WHERE YEAR(TableDate) = 2026``
        - Nelze použít index bo funkce
    - ``WHERE TableDate >= '20240101' AND TableDate < '20250101'``
        - Může použít index seek
## Execution plan
- V SSMS = **Ctrl + M**
- Nebo **``Include Actual Execution Plan``**
- Obsahuje
    - Index Scan
    - Table Scan
    - Key Lookup
    - Sort
    - Hash Match
    - Nested Loop
    - Merge Join
## Cost
- Operator Cost 80%
    - Pouze odhad optimizeru
## Statistics
- Statistiky používá optimizer
- Kolik řádků, Distribuce hodnot...
- Aktualizace
    - ``UPDATE STATISTICS dbo.Table``
    - ``sp_updatestats``
## Cardinality Estimation
- Odhad řádků
    - Kolik řádků vrátí operace
    - Kolik řádků projde join
    - Kolik dat jde do sortu, agregace, joinu...
- Hledá kde ``status = 'ACTIVE'``
- Špatný odhad = špatné O.o
    - Počet řádků ovlivní Joins (Neted, Hash,...)
    - Alokace paměti
        - Alokuje se méně paměti => spill do tempdb
    - Zbytečný paralelní plán, nebo žádný, je-li potřeba
- Vznik
    - Zastaralé statistiky
    - Nekorelované predikáty
        - ``WHERE Country = 'CZ' AND City = 'Liberec'``
        - Plán provede: ``P(A ∩ B) = P(A) × P(B)``, protože neví, že Liberec je jen v CZ
    - non-SARGable
    - Parametr Sniffing
    - Skewed data
        - Optimizer očekává rovnoměrné status = 'ACTIVE' x Ostatní
## Joins
 Nested pro malý dataset, Merge pokud jsou data sorted, Hash jako else možnost
- Nested loop
    - Rychlé pro malé množství dat
    - Pro každý řádek z outer tabulky hledej v inner
    - ``FOR each row in A: find matching rows in B``
    - Outer bývá často malá množina
    - Innder index, ideálně seek
    - Když není v inner tabulce index, vzniká spousta lookup -> NONO
- Hash join
    - Pro více řádků
    - Menší tabulka
        - Vytvoří hash table v paměti
    - Větší tabulka
        - Počítá hash a hledá shodu
    - Když jsou tabulky velké a bez vhodného indexu
    - Potřebuje RAM
    - Může přetéct (spill to tempdb)
        - Operace neměla dost paměti RAM -> Přetekla na disk
- Merge Join
    - Nejrychlejší
    - Požaduje setřízená data (ideal na indexy)
    - Lineární čas O(n)
    - Pokud nejsou indexy, sort je drahá operace
## Fragmentace indexů
- Vzniká při
    - INSERT, UPDATE, DELETE
- ``sys.dm_db_index_physical_stats``
- ~5-30%
    - Reorganize
    -  ```
        ALTER INDEX ALL
        ON Orders
        REORGANIZE;
        ```
- ~>30%
    - Rebuild
    -  ```
        ALTER INDEX ALL
        ON Orders
        REBUILD;
        ```
## Fill factor
- Kolik místa nechat na stránce
- ``FILLFACTOR = 80`` (zaplní 80%, 20% je volné)
    - Proti page split
        - Vložení hodnoty doprostřed indexu
## Pohledy
- Statistiky
    - sys.dm_exec_query_stats
- Text SQL
    - sys.dm_exec_sql_text
- Execution plan
    - sys.dm_exec_query_plan
- Použité Indexy
    - sys.dm_db_index_usage_stats
- Chybějící indexy
    - sys.dm_db_missing_index_details
#### SET STATISTICS IO
- ``SET STATISTICS IO ON;``
- Výstup: ``logical reads = 5000``
    - Čím méně, tím lépe
#### SET STATISTICS TIME
- ``SET STATISTICS TIME ON;``
- CPU time, elapsed time
## Parameter Sniffing
- Procedura: EXEC GetOrders 1
    - Optimizer vytvoří plán
- Potom: EXEC GetOrders 9999
    - Použije stejný plán, který může být pomalý
- Řešení
    - OPTION (RECOMPILE)
    - OPTIMIZE FOR
## CTE
- Common table expresion
- Od jeho vytvoření do konce SQL dotazu
- "Pojmeovaný subquerry"
- ```
    WITH cte as (
        SELECT * FROM Orders
    )
    SELECT * FROM cte;
    ```
## View
## TABLE
| Kategorie | Operátor | Co dělá | Poznámka |
|----------|----------|----------|----------|
| Access | Table Scan | Čte celou tabulku | Nejhorší pro velké tabulky |
| Access | Index Scan | Čte celý index | OK jen když vracíš většinu dat |
| Access | Index Seek | Cílené hledání v indexu | Nejlepší varianta |
| Access | Clustered Index Scan | Full scan tabulky (clustered) | Data jsou v clustered indexu |
| Access | Clustered Index Seek | Cílené hledání v clustered indexu | Velmi efektivní |
| Access | RID Lookup | Lookup v heap tabulce | Bez clustered indexu |
| Access | Key Lookup | Lookup z NCI do CI | Častý performance problém |

| Kategorie | Operátor | Co dělá | Poznámka |
|----------|----------|----------|----------|
| Join | Nested Loops | Pro každý řádek hledá match | Ideální pro malé dataset |
| Join | Hash Match (Join) | Hash tabulka + probing | Velké dataset joiny |
| Join | Merge Join | Slévání sorted inputů | Nejrychlejší při sorted datech |

| Kategorie | Operátor | Co dělá | Poznámka |
|----------|----------|----------|----------|
| Aggregate / Sort | Sort | Řazení dat | Často způsobí spill |
| Aggregate / Sort | Stream Aggregate | GROUP BY na sorted datech | Velmi efektivní |
| Aggregate / Sort | Hash Match (Aggregate) | GROUP BY přes hash | Používá hash strukturu |

| Kategorie | Operátor | Co dělá | Poznámka |
|----------|----------|----------|----------|
| Memory / Warning | Hash Warning | Nedostatek paměti pro hash | Risk spill do tempdb |
| Memory / Warning | Sort Warning | Nedostatek paměti pro sort | Často vede ke spill |
| Memory / Warning | Spill to tempdb | Operace jde na disk | Velký performance problém |

| Kategorie | Operátor | Co dělá | Poznámka |
|----------|----------|----------|----------|
| Flow | Compute Scalar | Výpočty výrazů | CASE, aritmetika |
| Flow | Filter | Filtrace řádků | Aplikuje WHERE podmínky |
| Flow | Top | TOP N výsledků | Omezení výstupu |
| Flow | Concatenation | UNION ALL | Spojuje result sety |

| Kategorie | Operátor | Co dělá | Poznámka |
|----------|----------|----------|----------|
| Parallelism | Gather Streams | Slučuje paralelní vlákna | Paralelní execution |
| Parallelism | Distribute Streams | Rozděluje data mezi vlákna | Load balancing |
| Parallelism | Repartition Streams | Přerozdělení dat | Optimalizace parallel plan |

| Kategorie | Operátor | Co dělá | Poznámka |
|----------|----------|----------|----------|
| Spool | Table Spool | Dočasná cache dat | Často u nested loops |
| Spool | Eager Spool | Materializace okamžitě | Memory heavy |
| Spool | Lazy Spool | Materializace podle potřeby | On-demand cache |

| Kategorie | Operátor | Co dělá | Poznámka |
|----------|----------|----------|----------|
| Windowing | Sequence Project | Window funkce | OVER() operace |
| Windowing | Window Spool | Podpora window funkcí | Interní buffer |
| Windowing | Segment | Rozdělení partition | ROW_NUMBER, RANK |

## Advanced SQL
- **Windows**
    - Funkce která počítá hodnotu pro každý řádek, ale bere v úvahu ostatní řádky
    - ``OVER (PARTITION BY ... ORDER BY ...)``
    - ``SELECT *, SUM(Amount) OVER (PARTITION BY CustomerID) FROM orders``
        - Jako group by, ale nezredukuje řádky

- **ROW_NUMBER**
- **RANK**
    - Stejné hodnoty = stejné pořadí
    - Mezery v pořadí
- **DENSE-RANK**
    - Nejsou mezery
- **LAG**
    - Předchozí řádek
    - ``SELECT OrderId, OrderDate, LAG(OrderDate) (PARTITION BY CustomerID ORDER BY OrderDate) AS PreviousOrder``
- **LEAD**
    - Následující řádek
- **PIVOT**
    - Otočení řádků na sloupce
- **UNPIVOT**
    - Otočení sloupců na řádky
