 Esso — Teljes Wiki (Magyar)

> Automatikusan lefordítva és összeállítva a deepwiki.com alapján.



 Tartalom

1. [Áttekintés](áttekintés)
2. [Első lépések](első-lépések)
3. [Architektúra és modulhatárok](architektúra-és-modulhatárok)
4. [Alapvető adatszerkezetek](alapvető-adatszerkezetek)
5. [OptimizationState](optimizationstate)
6. [EntanglementInfo és NodePairKey](entanglementinfo-és-nodepairkey)
7. [OptimizationStatistics](optimizationstatistics)
8. [Szimmetria alrendszer](szimmetria-alrendszer)
9. [SymmetryGroup és SymmetryTransform](symmetrygroup-és-symmetrytransform)
10. [SymmetryPattern és szimmetria detektálás](symmetrypattern-és-szimmetria-detektálás)
11. [Optimalizációs motor](optimalizációs-motor)
12. [EntangledStochasticSymmetryOptimizer: Konfiguráció és életciklus](entangledstochasticsymmetryoptimizer-konfiguráció-és-életciklus)
13. [Az optimalizációs ciklus](az-optimalizációs-ciklus)
14. [Lépéstípusok és az UndoLog](lépéstípusok-és-az-undolog)
15. [Elfogadási kritérium és hőmérséklet-ütemezés](elfogadási-kritérium-és-hőmérséklet-ütemezés)
16. [Célfüggvények](célfüggvények)
17. [Beépített célfüggvények](beépített-célfüggvények)
18. [Egyéni célfüggvények](egyéni-célfüggvények)
19. [Memóriakezelés és biztonság](memóriakezelés-és-biztonság)
20. [Tulajdonosi modell és mély másolás](tulajdonosi-modell-és-mély-másolás)
21. [UndoLog memóriabiztonság](undolog-memóriabiztonság)
22. [Szójegyzék](szójegyzék)



 

Az Entangled Stochastic Symmetry Optimizer (ESSO) egy Zig nyelven írt, nagy teljesítményű optimalizációs motor. Arra tervezték, hogy komplex, önhasonló gráfokként ábrázolt relációs struktúrákat optimalizáljon a szimulált hűtés (simulated annealing), a kvantummechanika és a fraktálgeometria technikáinak ötvözésével.

A projekt azt a kihívást kezeli, hogy globális minimumokat találjon olyan többdimenziós állapottérben, ahol a hagyományos gradiens ereszkedés (gradient descent) kudarcot vall a rögös energiatájképek miatt. A Symmetry Detection (szimmetria detektálás) kihasználásával az optimalizáló nagy léptékű kvantumugrásokat hajthat végre a keresési térben, míg az Entanglement (összefonódás) modellek hosszú távú korrelációkat biztosítanak a gráf csomópontjai között, amelyek irányítják a sztochasztikus keresési folyamatot.

 Tartományi integráció

Az ESSO négy fő elméleti tartomány metszéspontjában működik:

1. Simulated Annealing: A magmotor egy Metropolis-Hastings elfogadási kritériumot és egy hűtési ütemtervet használ a lokális minimumokból való kilépéshez.
2. Kvantummechanika: A csomópontokat Qubit-ként kezeli komplex amplitúdókkal és fázisokkal. Az optimalizációs lépések tartalmazhatnak kvantumállapot-transzformációkat.
3. Gráfelmélet: A rendszer SelfSimilarRelationalGraph struktúrákat optimalizál, módosítva az élsúlyokat és a topológiát egy célenergia minimalizálása érdekében.
4. Fraktálgeometria: Az optimalizáló nyomon követi és perturbálja a csomópontok fractal_dimension értékét, lehetővé téve az önhasonló strukturális minták kialakulását.

 Fő komponensek

 Az optimalizációs motor
Az elsődleges belépési pont az EntangledStochasticSymmetryOptimizer. Ez kezeli az optimalizációs életciklust, beleértve a hőmérséklet-szabályozást, az iterációs korlátokat és az optimize() ciklus meghívását.

 Állapot és összefonódás
Az OptimizationState tárolja a gráf és metaadatainak aktuális pillanatképét. Kritikus funkció az entanglement_map, amely a NodePairKey-t használja a csomópontok közötti EntanglementInfo nyomon követésére, reprezentálva a történelmi korrelációkat, amelyek befolyásolják a jövőbeli lépéseket.

 Szimmetria és transzformációk
A rendszer rendszeresen futtatja a detectSymmetries függvényt a strukturális minták, például forgatások vagy tükrözések azonosítására. Ezeket SymmetryPattern objektumokként tárolja, és arra használja, hogy SymmetryTransform műveleteket alkalmazzon a gráf kvantumállapotaira.



 Első lépések

Az Entangled Stochastic Symmetry Optimizer (ESSO) egy speciális optimalizációs motor, amelyet önhasonló relációs gráfok manipulálására terveztek a szimulált hűtés, a kvantumlogika és a geometriai szimmetria detektálás elveinek felhasználásával. Ez az oldal gyakorlati útmutatót nyújt az esso_optimizer.zig modul Zig projektbe történő integrálásához és egy alapvető optimalizációs ciklus végrehajtásához.

 Integráció és függőségek

Az ESSO használatához a projektnek tartalmaznia kell az esso_optimizer.zig fájlt és annak alapvető függőségeit. Az optimalizáló a gráfstruktúrákhoz az nsir_core modulra, az állapotmanipulációkhoz pedig a quantum_logic modulra támaszkodik.

 Szükséges fájlok
 esso_optimizer.zig: Az optimalizáló fő belépési pontja.
 nsir_core.zig: Biztosítja a SelfSimilarRelationalGraph, Node, Edge és Qubit típusokat.
 quantum_logic.zig: Biztosítja a QuantumState és RelationalQuantumLogic típusokat.

 Az optimalizáló példányosítása

Az EntangledStochasticSymmetryOptimizer az elsődleges motor. Szüksége van egy Allocator-ra a belső állapot kezeléséhez, beleértve az energiatörténetet, a hőmérséklettörténetet és a szimmetriamintákat.

 Inicializálási módszerek

Az optimalizáló inicializálására három elsődleges mód van:

1. initDefault(allocator): Szabványos heurisztikus értékekkel inicializál (pl. initial_temperature = 100.0, cooling_rate = 0.95).
2. init(allocator, params): Lehetővé teszi a hűtési ütemterv és a szimmetria intervallumok teljes manuális konfigurálását.
3. initWithSeed(allocator, seed): Hasonló az initDefault-hoz, de lehetővé teszi egy specifikus seed megadását a PRNG számára a reprodukálható optimalizációs futtatások biztosítása érdekében.

 Alapértelmezett konfigurációs konstansok
A következő alapértelmezések érvényesek az initDefault használatakor:
 DEFAULT_INITIAL_TEMP: 100.0
 DEFAULT_COOLING_RATE: 0.95
 DEFAULT_MAX_ITERATIONS: 10,000
 DEFAULT_SYMMETRY_INTERVAL: 100 iteráció

 Minimális használati példa

Az optimalizálás elvégzéséhez meg kell adni egy kiindulási SelfSimilarRelationalGraph-ot és egy ObjectiveFunction-t. Az optimalizáló egy új, optimalizált gráfpéldányt ad vissza.

 Az optimalizációs folyamat
Az optimize() függvény mély másolatot (deep copy) készít a bemeneti gráfról, biztosítva, hogy az eredeti érintetlen maradjon.

 Alapvető funkcionalitás referencia

 OptimizationState
Az OptimizationState struct adatkonténerként működik az optimize() ciklus során. Nyomon követi az aktuális gráfot, a kiszámított energiát és az entanglement_map-et, amely a csomópontok közötti kvantumkorrelációkat tárolja.

 optimize() végrehajtás
Amikor az optimize() meghívásra kerül, a motor:
1. Klónozza a bemeneti gráfot egy belső OptimizationState-be.
2. Szimmetriákat detektál a kezdeti SymmetryPattern lista feltöltéséhez.
3. Iterál a lépéseken, amelyek magukban foglalják az élperturbációkat, fáziseltolásokat és szimmetriatranszformációkat.
4. Elfogadja/Elutasítja a lépéseket az acceptMove függvény segítségével, amely a Metropolis-Hastings kritériumot valósítja meg: P(accept) = exp(-ΔE / T).



 Architektúra és modulhatárok

Az ESSO (Entangled Stochastic Symmetry Optimizer) elsősorban egyetlen fájlból álló motorként, az esso_optimizer.zig-ben van implementálva, amely magas szintű optimalizációs logikát hangszerel a gráfelméleti struktúrák és a kvantumlogikai definíciók integrálásával. Az architektúra egy Single-File Engine mintát követ, ahol az optimalizáló állapota, a szimmetria detektáló gépezet és a sztochasztikus keresési ciklus együtt helyezkedik el, hogy megkönnyítse a szoros csatolást az energiaértékelés és az állapotátmenetek között.

 Modulfüggőségek és importok

Az esso_optimizer.zig fájl integrációs rétegként szolgál az nsir_core és a quantum_logic modulok számára. Alacsony szintű adatszerkezeteket használ fel egy magasabb szintű optimalizációs kontextus felépítéséhez.

 Külső modul integráció
Az optimalizáló két elsődleges belső függőségre támaszkodik:
1. nsir_core.zig: Biztosítja a rendszer strukturális alapját.
  SelfSimilarRelationalGraph: Az optimalizálandó elsődleges adatszerkezet.
  Node, Edge, EdgeKey, EdgeQuality: Primitív gráfkomponensek.
  Qubit: Az állapot alapvető egysége a csomópontokon belül.
2. quantum_logic.zig: Biztosítja a matematikai és logikai keretrendszert a kvantumállapot-manipulációhoz.
  QuantumState: A csomópont komplex amplitúdóját és fázisát képviseli.
  RelationalQuantumLogic és LogicGate: Transzformációk és állapotkonzisztencia értékelésére szolgál.

 Nyilvános API felület

A nyilvános API-t minimálisra tervezték, csak a konfigurációhoz és a végrehajtáshoz szükséges típusokat teszi közzé. Az elsődleges belépési pont az EntangledStochasticSymmetryOptimizer struct.

 Fő nyilvános entitások
| Entitás | Szerep |
| : | : |
| EntangledStochasticSymmetryOptimizer | A fő motor, amely az optimize() ciklusért felelős. |
| OptimizationState | A gráf és kvantumtulajdonságainak pillanatképe egy adott iterációban. |
| ObjectiveFunction | Egy függvénymutató típus, amelyet az energiatájék definiálására használnak. |
| SymmetryGroup | Egy enum, amely meghatározza a támogatott geometriai transzformációkat (Tükrözés, Forgatás stb.). |

 Modulhatárok és adatfolyam

Az optimalizáló és a gráf közötti határt az OptimizationState kezeli. Ez a struct burkolóként (wrapper) működik a SelfSimilarRelationalGraph körül, kiegészítve azt optimalizáció-specifikus metaadatokkal, mint például az entanglement_map.

 Adatfolyam jellemzői
1. Tulajdonjog (Ownership): Az optimalizáló jellemzően klónozza a bemeneti gráfot a cloneGraph használatával, hogy biztosítsa, az eredeti adatok nem mutálódnak a sztochasztikus keresés során.
2. Szimmetria alkalmazása: A szimmetria detektáló logika azonosítja a SymmetryPattern objektumokat, amelyek SymmetryTransform példányokat tartalmaznak. Ezeket a transzformációkat a QuantumState objektumokra (a quantum_logic.zig-ből) alkalmazzák a megoldástér felfedezéséhez.
3. Állapot visszaállítása: Az UndoLog biztosítja, hogy az Aktuális állapot és a Javasolt állapot közötti határ szigorúan fennmaradjon. Ha egy lépést a Metropolis kritérium elutasít, az UndoLog visszaállítja a specifikus mezőket (élsúlyok, csomópont állapotok) vagy visszaállít egy teljes gráf pillanatképet.



 Alapvető adatszerkezetek

Ez a szakasz áttekintést nyújt az ESSO (Entangled Stochastic Symmetry Optimizer) motorban használt elsődleges adatszerkezetekről. Ezek a struktúrák képviselik az optimalizációs folyamat állapotát, a csomópontok közötti kvantum-összefonódások metaadatait, valamint a szimulált hűtési folyamat statisztikai nyomon követését.

 Strukturális áttekintés

Az alapvető entitások közötti kapcsolat az OptimizationState köré összpontosul, amely konténerként működik az optimalizált gráf és a hozzá tartozó kvantum metaadatok számára.



 OptimizationState

Az OptimizationState a központi struktúra, amely nyomon követi a SelfSimilarRelationalGraph aktuális konfigurációját egy optimalizációs futtatás során. Kezeli az aktuális konfiguráció energiaértékét, az iterációszámot és az entanglement_map-et, amely a csomópontpárok közötti kvantumkorrelációkat követi nyomon.

Főbb felelősségek:
 Gráf tulajdonjog: Kezeli, hogy az állapot birtokolja-e a gráf memóriáját, amelyre az owns_graph jelzőn keresztül mutat.
 Összefonódás kezelése: Kiszámítja és frissíti az entanglement_percentage értéket a teljes gráfon.
 Állapot klónozása: Mély másolási (deep-copy) funkcionalitást biztosít a clone() segítségével a legjobb eddigi állapot nyomon követésének támogatására.



 EntanglementInfo és NodePairKey

A csomópontok közötti kvantumkorrelációkat nem az alap gráfstruktúra tárolja, hanem metaadatként kezelik az optimalizálón belül.

 EntanglementInfo
Ez a struct tárolja egy kvantum-összefonódási kapcsolat fizikai tulajdonságait, beleértve:
 correlation_strength: Az összefonódás nagysága.
 phase_difference: A relatív fázis a két összefonódott qubit között.
 creation_time: Az időbeli bomlás kiszámításához használatos.

 NodePairKey
Annak biztosítására, hogy az A és B csomópont közötti összefonódást ugyanúgy kezeljék, mint a B és A csomópont közöttit, a NodePairKey lexikografikus normalizálást valósít meg. Biztosítja, hogy a kisebb csomópont ID mindig elsőként legyen tárolva, kanonikus kulcsot biztosítva az entanglement_map hash map számára.



 OptimizationStatistics

Az OptimizationStatistics struct átfogó telemetriai csomagot biztosít az optimalizációs folyamathoz. Az optimize() ciklus minden iterációjában frissül, hogy betekintést nyújtson a motor teljesítményébe és konvergencia viselkedésébe.

| Metrika | Cél |
| : | : |
| acceptance_rate | Az elfogadott lépések aránya az összes lépéshez képest; adaptív hűtéshez használatos. |
| energy (Legjobb/Aktuális) | Nyomon követi a célfüggvény minimalizálásának előrehaladását. |
| convergence_delta | Az energiaváltozás egy csúszóablak felett a fennsíkok (plateaus) észlelésére. |
| symmetries_detected | A gráfban talált geometriai minták száma. |



 OptimizationState

Az OptimizationState struct az elsődleges adatkonténer, amely a megoldástér egyetlen pontját képviseli az optimalizációs folyamat során. Magában foglalja a gráf strukturális topológiáját, kvantumállapot-tulajdonságait és a csomópontok közötti összefonódások komplex hálóját. Bemenetként szolgál az ObjectiveFunction értékelésekhez és az átalakítás tárgya a szimulált hűtési ciklus során.

 Alapvető struktúra és mezők

Az OptimizationState kezeli mind a fizikai adatokat (a gráfot), mind a felmerülő metaadatokat (energia és összefonódási metrikák), amelyek az optimalizálás előrehaladásának nyomon követéséhez szükségesek.

| Mező | Típus | Leírás |
| : | : | : |
| graph | SelfSimilarRelationalGraph | Mutató a csomópontokat és éleket tartalmazó alapul szolgáló gráfstruktúrára. |
| energy | f64 | A kiszámított skaláris érték, amely az aktuális állapot költségét képviseli. |
| entanglement_percentage | f64 | Egy normalizált érték (0.0-tól 1.0-ig), amely az összefonódott párok sűrűségét képviseli. |
| iteration | usize | Az aktuális lépésszám az optimalizációs életcikluson belül. |
| owns_graph | bool | Életciklus jelző, amely meghatározza, hogy a struct felelős-e a graph memória felszabadításáért. |
| entanglement_map | AutoHashMap(NodePairKey, EntanglementInfo) | Egy térkép, amely a specifikus csomópontpárok közötti kvantumkorrelációs adatokat tárolja. |



 Életciklus és memóriatulajdonjog

Az OptimizationState egy explicit tulajdonosi modellt alkalmaz a SelfSimilarRelationalGraph potenciálisan nagy memóriaigényének kezelésére.

 Inicializálás és de-inicializálás
 init: Inicializálja az entanglement_map-et és nullára állítja a kezdeti metrikákat. Nem foglalja le magát a gráfot, hanem egy mutatót vesz át rá.
 deinit: Törli az entanglement_map-et. Ha az owns_graph értéke true, akkor meghívja a graph.deinit() függvényt is, és felszabadítja a gráf mutatót a megadott allokátor segítségével.

 Mély másolás a clone segítségével
A szimulált hűtés legjobb állapot nyomon követésének támogatására a clone metódus az állapot mély másolatát (deep copy) készíti el. A globális cloneGraph segédprogramot használja a gráf topológiájának és metaadatainak replikálására, biztosítva, hogy az új OptimizationState teljesen független legyen a forrástól.
 Implementáció: Lefoglal egy új gráfot, átmásolja az entanglement_map bejegyzéseit, és az owns_graph értékét true-ra állítja az új példányhoz.



 Összefonódás kezelése

Az entanglement_map-et egy sor metódus kezeli, amelyek a csomópontok közötti kvantumkorrelációk életciklusát kezelik. Ezeket a korrelációkat a NodePairKey indexeli, amely biztosítja a csomópont ID-k kanonikus rendezését.

 Főbb metódusok
 addEntanglement: Létrehoz egy új EntanglementInfo bejegyzést egy csomópontpárhoz. Ha a pár már létezik, frissíti a meglévő bejegyzést.
 updateEntanglement: Frissíti egy meglévő pár korrelációs erősségét és fázisát. Automatikusan meghívja a refreshEntanglementPercentage függvényt a metrikák szinkronban tartásához.
 getEntanglement: Lekéri az EntanglementInfo-t egy adott párhoz, vagy null-t ad vissza, ha nincs korreláció.
 hasEntanglement: Logikai ellenőrzés két csomópont közötti korreláció meglétére.

 Metrika számítás
 averageEntanglement: Kiszámítja az átlagos correlation_strength értéket az entanglement_map összes párjára. 0.0-t ad vissza, ha a térkép üres.
 refreshEntanglementPercentage: Frissíti az entanglement_percentage mezőt. Ezt az aktív összefonódott párok és a gráfban lévő összes lehetséges pár arányaként számítják ki.



 EntanglementInfo és NodePairKey

Ez az oldal dokumentálja azokat az adatszerkezeteket, amelyeket az EntangledStochasticSymmetryOptimizer használ a csomópontok közötti kvantum-ihletésű korrelációk kezelésére és nyomon követésére. Ezek a struktúrák lehetővé teszik a rendszer számára, hogy fenntartsa az összefonódás tartós állapotát, amely befolyásolja az optimalizációs pályákat és az energiaszámításokat.

 NodePairKey és lexikografikus normalizálás

A NodePairKey struct egy egyedi, irányítatlan kapcsolatot képvisel két csomópont között a relációs gráfban. Mivel az ESSO rendszerben az összefonódás szimmetrikus, a rendszernek biztosítania kell, hogy egy $(A, B)$ párt azonos módon kezeljenek, mint egy $(B, A)$ párt.

Ezt a NodePairKey.init metóduson belüli lexikografikus normalizálással érik el.

 Implementációs részletek

A NodePairKey két csomópont azonosítót tárol. A NodePairKeyContext biztosítja a szükséges logikát a std.AutoHashMap számára a hash-eléshez és az egyenlőség-ellenőrzéshez ezeken a kulcsokon.

| Mező | Típus | Leírás |
| : | : | : |
| node_a_id | [32]u8 | Az első csomópont SHA-256 azonosítója (lexikografikusan kisebbre normalizálva). |
| node_b_id | [32]u8 | A második csomópont SHA-256 azonosítója (lexikografikusan nagyobbra normalizálva). |

 Főbb függvények

 init(id1, id2): Összehasonlítja a két azonosítót a std.mem.order használatával. A kisebb azonosítót a node_a_id-hez, a nagyobbat a node_b_id-hez rendeli. Ez biztosítja, hogy a getEntanglement(A, B) és a getEntanglement(B, A) mindig ugyanarra a hash map bejegyzésre oldódjon fel.
 hash(self, context): Kiszámítja mindkét azonosító kombinált hash-ét az entanglement_map-ben való használatra.
 eql(self, other, context): Konstans idejű memória-összehasonlítást végez mindkét azonosítón a kulcsok egyenlőségének meghatározására.



 EntanglementInfo

Az EntanglementInfo struct magában foglalja a két csomópont közötti korreláció fizikai és időbeli tulajdonságait. Nyomon követi a kötés erősségét, a fáziskapcsolatot és a pár történelmi aktivitását.

 Adatszerkezet

| Mező | Típus | Leírás |
| : | : | : |
| correlation_strength | f64 | Az összefonódás nagysága (0.0-tól 1.0-ig). |
| phase_difference | f64 | A relatív kvantumfázis a két csomópont között. |
| creation_time | i64 | Időbélyeg (ms), amikor az összefonódás először létrejött. |
| last_update_time | i64 | A legutóbbi interakció/megerősítés időbélyege (ms). |
| interaction_count | u64 | Azon alkalmak száma, ahányszor ez a pár részt vett egy optimalizációs lépésben. |

 Életciklus és dinamika

Az összefonódás dinamikus. Az init segítségével inicializálódik, az update segítségével erősödik, és idővel természetesen gyengül a getDecayFactor mechanizmuson keresztül.

1. init(strength, phase): Beállítja a kezdeti állapotot és rögzíti az aktuális rendszeridőt a std.time.milliTimestamp() használatával.
2. update(strength_delta, phase_new): Növeli az interaction_count értékét, frissíti a last_update_time-ot, és a correlation_strength értékét 0.0 és 1.0 közé szorítja.
3. getAge(current_time): Kiszámítja az utolsó frissítés óta eltelt időt.



 Bomlási mechanizmus

Annak megakadályozására, hogy az optimalizáló elavult korrelációk csapdájába essen, az ESSO motor egy exponenciális bomlási modellt valósít meg az összefonódásra. A getDecayFactor metódus kiszámítja, hogy az eredeti erősség mekkora része maradt meg az utolsó interakció óta eltelt idő alapján.

 Exponenciális bomlási képlet
A bomlást a következő logika alapján számítják ki:
Faktor = e^(-ln(2)  (Delta t / t_1/2))
Ahol:
 Delta t: A last_update_time óta eltelt idő.
 t_1/2: A half_life_ms paraméter (az optimalizáló konfigurációja biztosítja).

 Kontextuális használat az optimalizálásban
Az OptimizationState ezeket a struktúrákat az updateEntanglement és getEntanglement metódusaiban használja. Amikor a getEntanglement meghívásra kerül, nem csak a nyers correlation_strength-et adja vissza; alkalmazza az EntanglementInfo.getDecayFactor által kiszámított bomlási tényezőt, hogy biztosítsa, az optimalizáló a friss, aktív korrelációkat részesíti előnyben.



 OptimizationStatistics

Az OptimizationStatistics struct egy átfogó telemetriai konténer, amelyet az EntangledStochasticSymmetryOptimizer használ az optimalizációs folyamat egészségének, előrehaladásának és teljesítményének nyomon követésére. Rögzíti mind a pillanatnyi értékeket (mint az aktuális hőmérséklet), mind a kumulatív metrikákat (mint az összes energiaértékelés), hogy granuláris képet nyújtson a szimulált hűtési életciklusról.

 Adatszerkezet definíció

A struct az esso_optimizer.zig-ben van definiálva, és mezőket tartalmaz az algoritmikus nyomon követéshez, a fizikai szimulációs metrikákhoz és a teljesítmény benchmarkinghoz.

| Mező | Típus | Leírás |
| : | : | : |
| iterations_completed | usize | A végrehajtott fő ciklusok teljes száma. |
| moves_accepted | usize | Azon állapotátmenetek száma, amelyek megfeleltek a Metropolis kritériumnak. |
| moves_rejected | usize | Azon javasolt lépések száma, amelyeket az UndoLog segítségével visszavontak. |
| best_energy | f64 | Az optimalizálás kezdete óta talált legalacsonyabb energiaállapot. |
| current_energy | f64 | Az aktuális rendszerállapot energiája. |
| symmetries_detected | usize | A gráfban talált egyedi SymmetryPattern példányok száma. |
| entangled_pairs | usize | Az aktív bejegyzések száma az entanglement_map-ben. |
| elapsed_time_ms | i64 | Az optimize() függvényben eltöltött teljes falióra idő. |
| acceptance_rate | f32 | Az elfogadott lépések aránya az összes lépéshez képest (0.0-tól 1.0-ig). |
| cooling_factor_applied | f64 | A hőmérsékletre jelenleg alkalmazott tényleges szorzó. |
| local_minima_escapes | usize | Azon alkalmak száma, amikor a reheat_factor aktiválódott stagnálás miatt. |
| convergence_delta | f64 | Az energiaváltozás az utolsó két sikeres lépés között. |
| temperature | f64 | Az aktuális termodinamikai hőmérséklet, amely a lépések elfogadását szabályozza. |
| total_energy_evaluations | usize | Azon alkalmak száma, ahányszor az ObjectiveFunction meghívásra került. |
| average_move_delta | f64 | A javasolt lépések energiaváltozásainak (ΔE) mozgóátlaga. |



 Segédmetódusok

Az OptimizationStatistics struct számos metódust biztosít a nyers számlálók cselekvésre alkalmas optimalizációs adatokká történő feldolgozásához.

 updateAcceptanceRate
Kiszámítja az aktuális elfogadási arányt. Jellemzően egy iterációs blokk után hívják meg annak eldöntésére, hogy az adaptív hűtést aktiválni kell-e.
- Képlet: moves_accepted / (moves_accepted + moves_rejected)
- Biztonság: 0.0-t ad vissza, ha nem kíséreltek meg lépést, a nullával való osztás elkerülése érdekében.

 updateElapsedTime
Frissíti az elapsed_time_ms mezőt egy kezdő időbélyeg használatával.
- Implementáció: std.time.milliTimestamp() - start_time

 iterationsPerSecond
Kiszámítja az optimalizáló áteresztőképességét.
- Képlet: (iterations_completed  1000) / elapsed_time_ms
- Használati eset: Teljesítmény benchmarkinghoz és a befejezésig hátralévő idő becsléséhez használatos nagy max_iterations értékek esetén.

 isConverged
Meghatározza, hogy az optimalizálás elért-e egy stabil állapotot egy megadott küszöbérték alapján.
- Logika: true-t ad vissza, ha a convergence_delta kisebb, mint a küszöbérték ÉS a temperature közel van a min_temperature padlóhoz.

 Konvergencia és stagnálás
A statisztikák kritikus szerepet játszanak a motor adaptív viselkedésében:
1. Stagnálás észlelése: Ha a moves_accepted nem növekszik egy beállított ablakon keresztül, a local_minima_escapes számláló inkrementálódik, és a hőmérsékletet a reheat_factor használatával visszaállítják.
2. Adaptív hűtés: Az acceptance_rate-et nyomon követik; ha meghaladja a 0.6-ot, a cooling_factor_applied növekszik a konvergencia felgyorsítása érdekében. Ha 0.2 alá esik, a hűtés lelassul, hogy több felfedezést tegyen lehetővé.



 Szimmetria alrendszer

A Szimmetria alrendszer biztosítja azt a geometriai és algebrai gépezetet, amelyet az ESSO optimalizáló használ a SelfSimilarRelationalGraph-on belüli strukturális szabályosságok detektálására, reprezentálására és kiaknázására. A szimmetriák (tükrözések, forgatások és eltolások) azonosításával a rendszer összehangolt szimmetria-tudatos lépéseket alkalmazhat, amelyek hatékonyabban fedezik fel a megoldásteret, mint a véletlenszerű perturbációk önmagukban.

 Architekturális szerep

A szimmetria detektálás közvetlenül integrálva van az optimalizációs ciklusba. Minden symmetry_detection_interval iterációban a rendszer elemzi az aktuális gráfállapotot, hogy azonosítsa a csomópontpozíciók és kvantumfázisok ismétlődő mintáit. Ezeket a mintákat azután az applyMove (kifejezetten a 3-as lépéstípus) tájékoztatására használják, amely a kvantumállapotokat a detektált szimmetriacsoportok szerint alakítja át.



 Szimmetria reprezentáció

Az alrendszer két elsődleges adatszerkezetre támaszkodik a geometriai transzformációk és az általuk érintett csomóponthalmazok reprezentálására.

| Entitás | Szerep | Főbb komponensek |
| : | : | : |
| SymmetryGroup | A szimmetria típusának kategorikus osztályozása. | reflection, rotation_90, translation stb. |
| SymmetryTransform | Egy matematikai operátor, amely leképezi a koordinátákat és fázisokat. | origin_x, origin_y, scale_factor, parameters. |
| SymmetryPattern | Egy szimmetria konkrét példánya, amely a gráfban található. | pattern_id (SHA-256), nodes lista, symmetry_score. |



 Detektálás és életciklus

A szimmetria detektálás nem egy egyszeri esemény, hanem egy ismétlődő folyamat, amely alkalmazkodik, ahogy a gráf fejlődik a szimulált hűtés során. A detectSymmetries függvény a gráf qubit komponenseinek súlypontját (centroid) használja referenciapontként az invariáns tulajdonságok ellenőrzésére.



 Integráció az optimalizálással

A szimmetria alrendszert elsősorban az EntangledStochasticSymmetryOptimizer használja fel a fő ciklusa során.

1. Kezdeti detektálás: Egyszer hívódik meg az optimize() elején.
2. Időszakos újra-detektálás: Minden symmetry_detection_interval iterációban aktiválódik.
3. Szimmetria-tudatos lépések: Az applyMove-ban a 3-as lépéstípus kiválaszt egy véletlenszerű SymmetryPattern-t, és alkalmazza annak SymmetryTransform-ját az összes alkotó csomópontra.



 SymmetryGroup és SymmetryTransform

Ez az oldal dokumentálja a geometriai szimmetria reprezentációt és a transzformációs motort az ESSO (Entangled Stochastic Symmetry Optimizer) rendszeren belül. Ezek a komponensek felelősek a térbeli és kvantum transzformációk definiálásáért, azonosításáért és alkalmazásáért a relációs gráfra az optimalizálás során.

 SymmetryGroup Enum

A SymmetryGroup enumeráció meghatározza az optimalizáló által támogatott szimmetriák alapvető típusait. Tartalmaz segédmetódusokat a karakterlánc-konverzióhoz, a szögtulajdonságokhoz és a csoportelméleti rendhez.

| Tag | Érték | Leírás |
| : | : | : |
| identity | 0 | Nincs transzformáció alkalmazva. |
| reflection | 1 | Tükrözés a parameters[3] által meghatározott tengelyen. |
| rotation_90 | 2 | 90 fokos forgatás az óramutató járásával megegyező irányba. |
| rotation_180 | 3 | 180 fokos forgatás. |
| rotation_270 | 4 | 270 fokos forgatás az óramutató járásával megegyező irányba (vagy 90 fokos ellentétes irányba). |
| translation | 5 | Lineáris eltolás az XY síkban. |

 Segédmetódusok

 toString(): Visszaadja az enum tag karakterlánc reprezentációját.
 fromString(s:[]const u8): Egy karakterláncot SymmetryGroup taggá elemez, null-t adva vissza, ha nincs egyezés.
 getAngle(): Visszaadja a forgatás radián egyenértékét. Az identity, reflection és translation 0.0-t ad vissza.
 getOrder(): Visszaadja a csoport rendjét (pl. a rotation_90 rendje 4, ami azt jelenti, hogy 4 alkalmazás visszatér az identitáshoz). A translation 0-t ad vissza, mivel végtelen rendű egy folytonos térben.



 SymmetryTransform Struct

A SymmetryTransform struct magában foglal egy specifikus geometriai műveletet, beleértve annak origópontját, skálázási tényezőjét és specifikus paramétereit (például tükrözési szögeket vagy eltolási vektorokat).

 Mező definíciók

| Mező | Típus | Leírás |
| : | : | : |
| group | SymmetryGroup | A szimmetria művelet típusa. |
| origin_x | f64 | A transzformáció középpontjának X-koordinátája. |
| origin_y | f64 | A transzformáció középpontjának Y-koordinátája. |
| parameters | [4]f64 | Kiegészítő adatok: [0,1] az eltoláshoz/origóhoz, [2] a skálához, [3] a szöghöz. |
| scale_factor | f64 | A transzformáció során alkalmazott skálázási szorzó. |

 Konstrukció

 init(group): Létrehoz egy alapértelmezett transzformációt egy csoporthoz (0,0) origóval, 1.0 skálával és [0, 0, 1, 0] alapértelmezett paraméterekkel.
 initWithParams(group, params): Létrehoz egy transzformációt egy paramétertömb használatával. Automatikusan beállítja az origin_x és origin_y értékeket a params[0] és params[1] alapján, valamint a scale_factor-t a params[2] alapján (alapértelmezés szerint 1.0, ha nem pozitív).



 Térbeli és kvantum alkalmazás

A transzformációs motor áthidalja az absztrakt szimmetriacsoportokat a konkrét adatszerkezetekkel az nsir_core és a quantum_logic modulokban.

 Komplex és kvantumállapot leképezés

1. applyToComplex(z: Complex(f64)): Burkolja az apply-t a std.math.Complex típusok kezelésére, a re-t x-ként, az im-et y-ként kezelve.
2. applyToQuantumState(state: const QuantumState): Alkalmazza a szimmetriát egy QuantumState-re. Ez kifejezetten a phase mezőt módosítja. Forgatások esetén az új fázis az eredeti fázis plusz a forgatási szög és bármilyen további szögeltolás a parameters[3]-ban, $[0, 2\pi)$ tartományra normalizálva.



 Algebrai műveletek

A SymmetryTransform támogatja az algebrai manipulációt az inverzek és kompozíciók megtalálásához.

 Inverz számítás
Az inverse() metódus egy SymmetryTransform-ot ad vissza, amely megfordítja az eredeti műveletet.
 Forgatások: A rotation_90 rotation_270-né válik és fordítva.
 Skála: Az inverz az 1.0 / scale_factor-t használja.
 Tükrözés/Identitás: Ezek öninverzálóak (ugyanazokat a paramétereket feltételezve).



 SymmetryPattern és szimmetria detektálás

A Szimmetria alrendszer az ESSO (Entangled Stochastic Symmetry Optimizer) rendszerben biztosítja a gépezetet a relációs gráfon belüli geometriai és topológiai szabályosságok azonosítására, reprezentálására és kiaknázására. Ezen minták azonosításával az optimalizáló nagy léptékű állapotátmeneteket hajthat végre, amelyek megőrzik vagy javítják a rendszer strukturális integritását, túllépve az egyszerű lokális perturbációkon.

 A SymmetryPattern Struct

A SymmetryPattern struct az elsődleges adatkonténer egy detektált szimmetriához. Magában foglalja a transzformációs logikát, az érintett csomópontok halmazát, valamint a minta jelentőségére és stabilitására vonatkozó metrikákat.

 Életciklus és adatszerkezet
Egy SymmetryPattern egy allokátorral és egy specifikus SymmetryTransform-mal inicializálódik. Fenntart egy belső listát a szimmetriában részt vevő csomópont ID-król.

 Inicializálás: Az init beállítja az ArrayList(u64)-et a csomópontokhoz, és hozzárendel egy létrehozási időbélyeget a std.time.milliTimestamp() használatával.
 Csomópont kezelés: Az addNode hozzáfűz egy csomópont ID-t a minta taglistájához.
 Azonosítás: A getPatternIdHex egy egyedi SHA-256 ujjlenyomatot generál a transzformáció paraméterei és a tagcsomópontok rendezett listája alapján.
 Tisztítás: A deinit felszabadítja a csomópontlistához lefoglalt memóriát.

 Főbb mezők
| Mező | Típus | Leírás |
| : | : | : |
| pattern_id | [32]u8 | SHA-256 hash, amely egyedileg azonosítja a specifikus minta konfigurációt. |
| transform | SymmetryTransform | A szimmetriát meghatározó geometriai művelet (forgatás, tükrözés stb.). |
| nodes | ArrayList(u64) | Azon csomópontok ID-jainak listája, amelyek a transzformáció alatt egymásra képeződnek le. |
| symmetry_score | f64 | Kvantitatív mérőszám arra vonatkozóan, hogy a szimmetria mennyire tökéletesen fejeződik ki. |
| resonance_frequency | f64 | A detektálás gyakoriságából vagy az iterációk során mutatott stabilitásból származtatott érték. |



 Szimmetria detektáló algoritmus

A detectSymmetries függvény egy többlépcsős heurisztikát valósít meg a geometriai minták megtalálására a qubitek komplex amplitúdóinak és a gráf topológiai tulajdonságainak elemzésével.

 1. Súlypont számítás
Az algoritmus a gráf geometriai súlypontjának (centroid) kiszámításával kezdődik. Minden csomópont Qubit-jének re (valós) és im (képzetes) komponensét térbeli koordinátákként $(x, y)$ kezeli.

 2. Minta heurisztikák
A detektor végigiterál a csomópontokon, és specifikus triggerek alapján értékeli a potenciális transzformációkat:

 Tükrözés detektálása: A csomópontfokok és a frekvenciakomponensek közötti egyensúly váltja ki. Értékeli, hogy a csomópontok tükrözhetők-e egy a súlyponton áthaladó tengelyen.
 Forgatás (90°/270°) detektálása: Kifejezetten olyan csomópontokat keres, ahol a fok négy többszöröse, ami 4-szeres forgási szimmetriára utal. Érvényesíti, hogy egy 90 fokos forgatás alkalmazása egy csomópont komplex amplitúdóját egy másik meglévő csomópont állapotára képezi-e le.
 Forgatás (180°) detektálása: A csomópontfázisok körkörös átlagával számítva. Ha a fáziseloszlás erős 2-szeres torzítást mutat, egy 180 fokos transzformációt tesztel.

 3. Deduplikáció és tárolás
Az újonnan detektált mintákat összehasonlítják az OptimizationState-ben lévő meglévő mintákkal az SHA-256 ujjlenyomatuk alapján. Ha egy minta egyedi, hozzáadódik az állapot symmetries listájához; ellenkező esetben a meglévő minta resonance_frequency értéke inkrementálódik.



 Integráció az optimalizációs ciklusban

A szimmetria detektálást nem hajtják végre minden iterációban az SHA-256 hash-elés és a geometriai ellenőrzés számítási költsége miatt.

 Időszakos detektálás
Az optimize ciklus minden symmetry_detection_interval iterációban (alapértelmezés szerint 100) elindítja a detectSymmetries függvényt. Ez biztosítja, hogy ahogy a gráf fejlődik a sztochasztikus lépéseken keresztül, az új strukturális szabályosságok rögzítésre kerüljenek.

 Szimmetria-alapú lépések
Miután a mintákat detektálták, az applyMove (3-as lépéstípus) felhasználja őket. Véletlenszerű perturbáció helyett az optimalizáló kiválaszt egy véletlenszerű SymmetryPattern-t, és alkalmazza annak SymmetryTransform-ját az adott mintához társított összes csomópont QuantumState-jére.

 Implementációs részletek: Minta hash-elés
A getPatternIdHex metódus kritikus a deduplikációhoz. Biztosítja, hogy ugyanaz a szimmetria ne legyen többször tárolva a következők révén:
1. A SymmetryGroup és a parameters beírása egy pufferbe.
2. A csomópont ID-k rendezése a nodes listában annak biztosítására, hogy a hash invariáns legyen a csomópontok felfedezési sorrendjére.
3. A rendezett csomópontlista betáplálása a std.crypto.hash.sha2.Sha256-ba.



 Optimalizációs motor

Az Optimalizációs motor az Esso projekt alapvető számítási komponense, amely az EntangledStochasticSymmetryOptimizer köré épül. Egy hibrid metaheurisztikát valósít meg, amely ötvözi a szimulált hűtést a kvantum-ihletésű mechanikával, mint például az összefonódás és a szimmetria-alapú állapottranszformációk.

A motor feltérképezi egy SelfSimilarRelationalGraph konfigurációs terét egy felhasználó által definiált ObjectiveFunction minimalizálása érdekében. Kezeli az optimalizációs folyamat életciklusát, a kezdeti szimmetria detektálástól a gráfállapot végső konvergenciájáig.



 Konfiguráció és életciklus
Az optimalizálót az EntangledStochasticSymmetryOptimizer struct konfigurálja, amely olyan paramétereket tartalmaz, mint az initial_temperature, cooling_rate és max_iterations. Több inicializálási mintát támogat:
 initDefault: Szabványos konstansokat használ, mint a DEFAULT_INITIAL_TEMP (100.0).
 initWithSeed: Lehetővé teszi a reprodukálható futtatásokat a belső PRNG seed-elésével.

 Az optimalizációs ciklus
Az optimize() függvény hajtja végre az elsődleges életciklust. A bemeneti gráf klónozásával és egy kezdeti szimmetria detektálás elvégzésével kezdődik. A fő ciklus iteratívan alkalmazza a perturbációkat, értékeli az energiaváltozásokat, és frissíti az entanglement_map-et.

 Lépéstípusok és az UndoLog
A motor hét különböző lépéstípus (move type) segítségével fedezi fel a megoldásteret az applyMove()-on belül. Ezek az egyszerű élsúly perturbációtól a komplex szimmetria-alapú kvantumállapot transzformációkig terjednek. Az atomicitás biztosítása érdekében az UndoLog rögzíti a változásokat, lehetővé téve az undoMove() számára az állapot visszaállítását, ha egy lépést elutasítanak.

 Elfogadás és hűtés
A motor egy Metropolis elfogadási kritériumot alkalmaz. Az energiát növelő lépéseket $P = \exp(-\Delta E / T)$ valószínűséggel fogadja el. A $T$ hőmérséklet idővel csökken geometriai hűtés vagy egy adaptive_cooling stratégia használatával, amely reagál az aktuális acceptance_rate-re.



 EntangledStochasticSymmetryOptimizer: Konfiguráció és életciklus

Ez az oldal dokumentálja az EntangledStochasticSymmetryOptimizer struct-ot, az ESSO projekt elsődleges motorját. Részletezi a konfigurációs paramétereket, az optimalizáló objektum életciklusát, valamint a hibrid szimulált hűtéshez és a kvantum-ihletésű optimalizáláshoz szükséges belső állapotkezelést.

 Optimalizáló konfiguráció és konstansok

Az optimalizálót egy sor paraméter konfigurálja, amelyek szabályozzák a szimulált hűtési ütemtervet, a szimmetria detektálás gyakoriságát és az összefonódás bomlási mechanizmusának viselkedését.

 Alapértelmezett konstansok
A következő konstansok határozzák meg az optimalizáló alapértelmezett viselkedését, ha az initDefault segítségével inicializálják:

| Konstans | Érték | Leírás |
| : | : | : |
| DEFAULT_INITIAL_TEMP | 100.0 | Kezdeti hőmérséklet a hűtési folyamathoz. |
| DEFAULT_COOLING_RATE | 0.95 | A hőmérsékletre minden iterációban alkalmazott szorzó (geometriai hűtés). |
| DEFAULT_MAX_ITERATIONS | 10000 | A megkísérelendő lépések maximális száma. |
| DEFAULT_MIN_TEMP | 0.001 | A hőmérsékleti padló, ahol a hűtés leáll. |
| DEFAULT_REHEAT_FACTOR | 1.5 | Szorzó a hőmérséklethez, ha stagnálást észlel. |
| DEFAULT_SYMMETRY_INTERVAL | 100 | Iterációk a globális szimmetria detektálási menetek között. |
| DEFAULT_CONVERGENCE_THRESHOLD | 1e-6 | Energia delta, amely alatt a rendszer konvergáltnak tekintendő. |
| DEFAULT_DECAY_HALF_LIFE | 5000.0 | Idő ms-ban, amíg az összefonódás erőssége a felére csökken. |

 Struct mezők
Az EntangledStochasticSymmetryOptimizer a következő belső állapotot tartja fenn:

 initial_temperature: A kezdő $T$ a Metropolis kritériumhoz.
 cooling_rate: A termikus bomlás sebessége.
 max_iterations: Szigorú korlát az optimalizációs ciklusra.
 min_temperature: A minimálisan megengedett $T$.
 reheat_factor: A lokális minimumokból való kilépéshez használatos stagnálás során.
 entanglement_decay_half_life: Szabályozza az EntanglementInfo időbeli bomlását.
 symmetry_detection_interval: A detectSymmetries hívások gyakorisága.
 convergence_threshold: Kritériumok a korai befejezéshez.
 adaptive_cooling: Logikai jelző a dinamikus hűtési sebesség beállítások engedélyezéséhez.
 prng: Egy std.rand.DefaultPrng példány a sztochasztikus lépésekhez.
 energy_history: ArrayList(f64) az energia időbeli nyomon követésére.
 temperature_history: ArrayList(f64) a $T$ időbeli nyomon követésére.

 Életciklus kezelés

Az optimalizáló egy szigorú allokáció-alapú életciklust követ. Szüksége van egy std.mem.Allocator-ra a történeti pufferek és a belső szimmetriaminta-tárolás kezeléséhez.

 Konstruktor változatok

Az optimalizáló három módot biztosít a motor példányosítására:

1. init(allocator, initial_temp, cooling_rate, max_iterations): Az elsődleges hűtési paraméterek teljes manuális konfigurálása.
2. initDefault(allocator): Inicializál a fent felsorolt DEFAULT_ konstansokkal.
3. initWithSeed(allocator, seed): Hasonló az initDefault-hoz, de lehetővé teszi egy specifikus seed megadását a prng számára a determinisztikus optimalizációs futtatások biztosítása érdekében.

 Konfigurációs beállítók (Setters)

Az inicializálás után a specifikus viselkedések fluent-stílusú beállítókkal hangolhatók:

 setObjectiveFunction(func): Hozzárendeli az energia kiszámításához használt ObjectiveFunction mutatót.
 setAdaptiveCooling(enabled): Átkapcsolja az adaptív hűtési logikát, amely az elfogadási statisztikák alapján módosítja a cooling_rate-et.
 setMinTemperature(temp): Felülbírálja az alapértelmezett padlót.
 setReheatFactor(factor): Beállítja a szorzót a lokális minimumból való kilépéshez.
 setSymmetryDetectionInterval(interval): Beállítja, hogy a motor milyen gyakran keressen geometriai mintákat.

 De-inicializálás
A deinit() függvényt meg kell hívni az energy_history és a temperature_history listák felszabadításához. Vegye figyelembe, hogy az optimalizáló nem birtokolja az optimize hívás során átadott SelfSimilarRelationalGraph-ot; csak a belső nyomon követési adatait kezeli.

 Belső történet nyomon követése

Az optimalizáló két elsődleges történeti puffert tart fenn az optimalizálás utáni elemzés és a konvergencia vizualizációjának lehetővé tétele érdekében.

| Mező | Típus | Cél |
| : | : | : |
| energy_history | ArrayList(f64) | Tárolja az ObjectiveFunction eredményét minden iterációban. Stagnálás észlelésére használatos. |
| temperature_history | ArrayList(f64) | Tárolja az aktuális $T$ hőmérsékletet. Vizualizálja a hűtési ütemtervet és az újramelegítési tüskéket. |

Ezek az optimize cikluson belül töltődnek fel:
1. Az energia az acceptMove logika után fűződik hozzá.
2. A hőmérséklet a hűtési lépés után fűződik hozzá.



 Az optimalizációs ciklus

Az optimize() függvény az EntangledStochasticSymmetryOptimizer alapvető végrehajtó motorja. Egy speciális szimulált hűtési algoritmust valósít meg, amely integrálja a kvantum-összefonódási dinamikát és a geometriai szimmetria detektálást, hogy egy SelfSimilarRelationalGraph-ot egy globális energiaminimum felé fejlesszen.

 Ciklus életciklus áttekintése

Az optimalizációs folyamat az inicializálás, az iteratív perturbáció és a konvergencia-ellenőrzések strukturált sorrendjét követi. A ciklus egy stagnation_counter-t használ az újramelegítési (reheating) események kiváltására, megakadályozva, hogy a rendszer lokális minimumok csapdájába essen.

 Magas szintű végrehajtási folyamat

1. Inicializálás: A bemeneti gráf mély klónozása és a kezdeti OptimizationState beállítása.
2. Kezdeti elemzés: A szimmetria detektálás és az összefonódás feltérképezésének első menetének futtatása.
3. A fő ciklus: Iterálás a max_iterations eléréséig vagy amíg a convergence_threshold nem teljesül.
  Perturbáció: Egy sztochasztikus lépés kiválasztása és alkalmazása (pl. élsúly eltolás, szimmetria transzformáció).
  Értékelés: Az új energia kiszámítása az ObjectiveFunction segítségével.
  Kiválasztás: A Metropolis kritérium alkalmazása a lépés elfogadására vagy elutasítására.
  Karbantartás: A hőmérséklet hűtése és a szimmetriák időszakos újra-detektálása.
4. Véglegesítés: A legjobbnak talált állapot kinyerése és az ideiglenes erőforrások tisztítása.



 Lépésről lépésre történő implementáció

 1. Beállítás és kezdeti állapot
A ciklus a gráf egy munkapéldányának létrehozásával kezdődik, hogy biztosítsa, az eredeti érintetlen marad, ha a folyamat meghiúsul. Inicializálja az OptimizationState-et, amely nyomon követi az aktuális gráfot, az energiát és az összefonódási metaadatokat.

 Gráf klónozás: A cloneGraph mély másolatot készít az összes csomópontról és élről.
 Állapot beállítás: Az OptimizationState.init meghívásra kerül owns_graph = true értékkel.
 Kezdeti metrikák: A ciklus kiszámítja a kezdeti energiát és feltölti a kezdeti összefonódási térképet.

 2. Szimmetria és összefonódás dinamika
Minden iteráció elején az optimalizáló frissíti a csomópontok közötti kapcsolatot.
 Összefonódás frissítések: Az updateEntanglementMap meghívásra kerül a régi korrelációk bomlasztására és az aktívak megerősítésére.
 Időszakos szimmetria detektálás: Minden symmetry_detection_interval (alapértelmezés szerint 100 iteráció) után a detectSymmetries függvény meghívásra kerül. Ez feltölti a symmetries listát, amelyet a 3-as lépéstípus (Szimmetria-alapú transzformációk) használ.

 3. Lépés alkalmazása és Undo logika
Az optimalizáló kiválaszt egyet a hét lépéstípus közül az applyMove segítségével. Annak érdekében, hogy lehetővé tegye a lépések hatékony elutasítását drága gráf újra-klónozás nélkül, egy UndoLog-ot használ.

| Entitás | Szerep |
| : | : |
| UndoLog | Tárolja az élsúlyok, csomópont állapotok vagy a gráf topológia korábbi értékeit. |
| applyMove() | Módosítja az OptimizationState-et és rögzíti a változásokat az UndoLog-ban. |
| undoMove() | Visszaállítja az állapotot az UndoLog-ból, ha egy lépést elutasítanak. |

 4. Energia értékelés és elfogadás
Az új állapot energiáját az objective_fn segítségével számítják ki.
 Metropolis kritérium: Az acceptMove határozza meg, hogy a változás megmarad-e.
  Ha ΔE < 0 (javulás), a lépés mindig elfogadásra kerül.
  Ha ΔE > 0, a lépés $P = e^{-\Delta E / T}$ valószínűséggel kerül elfogadásra.

 5. Hűtés és stagnálás kezelése
A $T$ hőmérséklet a cooling_rate szerint csökken.
 Stagnálás számláló: Ha egy bizonyos számú iterációig nem fogadnak el lépést, a stagnation_counter inkrementálódik.
 Újramelegítés: Amikor a stagnation_counter >= stagnation_limit, a hőmérsékletet megszorozzák a reheat_factor-ral, hogy kiugorjanak a lokális minimumokból.
 Konvergencia: Ha az energia delta több iteráción keresztül a convergence_threshold alatt marad, a ciklus korán befejeződik.

 Végső kinyerés
Amint a ciklus befejeződik (max iterációk, konvergencia vagy kézi leállítás révén), az optimalizáló a következőket hajtja végre:
1. Legjobb állapot lekérése: Azonosítja azt az OptimizationState-et, amely a legalacsonyabb best_energy-t produkálta.
2. Gráf kinyerése: Meghívja a best_state.cloneGraph() függvényt, hogy a hívónak egy tiszta, optimalizált gráfpéldányt biztosítson.
3. Tisztítás: Meghívja a deinit() függvényt az összes köztes OptimizationState és UndoLog objektumon a memóriaszivárgások megelőzése érdekében.



 Lépéstípusok és az UndoLog

Ez az oldal dokumentálja az EntangledStochasticSymmetryOptimizer által a megoldástér felfedezésére használt perturbációs mechanizmusokat, valamint a tranzakcionális visszaállítási rendszert, amely biztosítja az állapot integritását az optimalizálás során.

 Lépéstípusok

Az applyMove függvény hét különböző perturbációs stratégiát (0-6) valósít meg. Ezeket a lépéseket véletlenszerűen választják ki az optimize ciklus minden iterációjában a gráf topológiájának, kvantumállapotainak vagy fraktál tulajdonságainak módosítására.

 Lépés osztályozási táblázat

| ID | Név | Leírás | Érintett adat |
| : | : | : | : |
| 0 | Élsúly perturbáció | Módosítja egy véletlenszerűen kiválasztott él weight értékét. | Edge.weight |
| 1 | Csomópont fázis perturbáció | Beállítja egy véletlenszerűen kiválasztott csomópont QuantumState-jének phase értékét. | Node.state.phase |
| 2 | Összefonódás létrehozása | Létrehoz egy új EntanglementInfo kapcsolatot két csomópont között. | OptimizationState.entanglement_map |
| 3 | Szimmetria transzformáció | Alkalmaz egy SymmetryTransform-ot egy csomópontra a detektált minták alapján. | Node.state |
| 4 | Amplitúdó perturbáció | Módosítja a qubit amplitúdókat, majd egységvektor normalizálást végez. | Node.state.amplitude_real/imag |
| 5 | Fraktál perturbáció | Véletlenszerűen eltolja egy csomópont fractal_dimension értékét. | Node.fractal_dimension |
| 6 | Él váltás (Toggle) | Hozzáad egy új élt vagy eltávolít egy meglévőt (topológiai változás). | SelfSimilarRelationalGraph |

 Implementációs részletek

 3-as lépés (Szimmetria): Ha rendelkezésre állnak symmetry_patterns, az optimalizáló kiválaszt egy véletlenszerű mintát, és alkalmazza annak SymmetryTransform-ját egy csomópont kvantumállapotára az applyToQuantumState használatával.
 4-es lépés (Normalizálás): A valós/képzetes amplitúdók perturbálása után a rendszer biztosítja, hogy a kvantumállapot érvényes maradjon a vektor 1.0 magnitúdóra történő normalizálásával.
 6-os lépés (Topológia): Ez a legdestruktívabb lépés. Teljes gráf pillanatképet igényel az UndoLog-ban, mert megváltoztatja a gráf alapul szolgáló AutoHashMap struktúráit.



 Az UndoLog Struct

Az UndoLog egy veremben lefoglalt struktúra (az optimize hatókörön belül), amelyet a módosított entitások korábbi állapotának tárolására használnak. Lehetővé teszi az undoMove függvény számára, hogy O(1) vagy O(N) visszaállításokat hajtson végre, amikor egy lépést a Metropolis kritérium elutasít.

 Főbb függvények

1. clear(): Visszaállítja a naplót a következő iterációhoz. Döntő fontosságú, hogy ha az old_graph jelen van (egy 6-os típusú lépésből), meghívja a deinit() függvényt a pillanatképen a memóriaszivárgások megelőzése érdekében.
2. undoMove(): A move_type alapján specifikus helyreállítási logikához irányít. Például a 0-s típus esetén végigiterál az edge_weights-en, és visszaállítja az eredeti értékeket.



 Kód referencia az állapot visszaállításához

Amikor az undoMove aktiválódik, a rendszer a következőket hajtja végre a move_type alapján:

 0, 1, 4, 5 típusok: Végigiterál a node_states vagy edge_weights elemeken, és újra hozzárendeli az értékeket.
 2-es típus: Eltávolítja azokat a kulcsokat az entanglement_map-ből, amelyeket a lépés során adtak hozzá.
 6-os típus: Kicseréli az aktuális gráfot a naplóban tárolt old_graph-ra. A sérült gráf ezután de-inicializálódik.



 Memóriabiztonság és szivárgások

Az UndoLog úgy van kialakítva, hogy szivárgásmentes legyen a következő minták révén:

1. Tulajdonjog csere: A 6-os típusú lépéseknél az OptimizationState feladja az aktuális gráfját, és átveszi az old_graph-ot a naplóból. Az UndoLog.clear() vagy UndoLog.deinit() ezután kezeli az elutasított gráf felszabadítását.
2. Errdefer használata: A lépés alkalmazása során, ha egy allokáció meghiúsul (pl. amikor a node_states-hez fűz hozzá), az errdefer biztosítja, hogy minden részlegesen módosított állapotot kezeljenek, mielőtt a hiba továbbterjedne.
3. Pillanatkép kezelés: Az old_graph mező egy Optional. Csak topológiai lépések során töltődik fel, biztosítva, hogy a cloneGraph magas költségét csak akkor fizessék meg, amikor szükséges.



 Elfogadási kritérium és hőmérséklet-ütemezés

Ez az oldal dokumentálja a termodinamikai döntéshozatali folyamatot és a hűtési logikát az EntangledStochasticSymmetryOptimizer-en belül. Ezek a mechanizmusok szabályozzák, hogyan navigál az optimalizáló az energiatájképen, egyensúlyozva a felfedezést (kilépés a lokális minimumokból) a kiaknázással (konvergálás egy globális minimumra).

 Metropolis elfogadási kritérium

Az optimalizáló a Metropolis-Hastings kritériumot alkalmazza az acceptMove függvényen belül annak meghatározására, hogy egy javasolt gráf transzformációt meg kell-e tartani. Ez a sztochasztikus döntés az energiaváltozáson ($\Delta E$) és az aktuális rendszerhőmérsékleten ($T$) alapul.

 Implementációs logika
A döntés a következő szabályokat követi:
1. Energiacsökkentő lépések: Ha $\Delta E \le 0$, a lépés mindig elfogadásra kerül (valószínűség = 1.0).
2. Energianövelő lépések: Ha $\Delta E > 0$, a lépés $P = \exp(-\Delta E / T)$ valószínűséggel kerül elfogadásra.

A kódban ez úgy van implementálva, hogy generál egy véletlenszerű lebegőpontos számot 0.0 és 1.0 között, és összehasonlítja a kiszámított valószínűséggel.

 Hőmérséklet-ütemezési stratégiák

Az EntangledStochasticSymmetryOptimizer két elsődleges hűtési stratégiát támogat a $T$ hőmérséklet csökkentésére az egymást követő iterációk során. A választást az adaptive_cooling logikai jelző szabályozza az optimalizáló konfigurációjában.

 1. Szabványos geometriai hűtés
Amikor az adaptive_cooling értéke false, a rendszer rögzített geometriai bomlást használ. Minden iterációban a hőmérsékletet megszorozzák egy konstans cooling_rate-tel (jellemzően 0.9 és 0.99 között).

$$T_{n+1} = T_n \times \text{cooling\_rate}$$

 2. Adaptív hűtés
Ha engedélyezve van, az adaptiveCoolTemperature függvény beállítja a hűtési sebességet a legutóbbi acceptance_rate alapján, amelyet az OptimizationStatistics követ nyomon. Ez megakadályozza, hogy a rendszer túl gyorsan lefagyjon vagy céltalanul vándoroljon.

| Elfogadási arány | Művelet | Logika |
| : | : | : |
| Magas (> 0.6) | Hűtés gyorsítása | $T = T \times (\text{cooling\_rate} \times 0.95)$ |
| Alacsony (< 0.2) | Hűtés lassítása | $T = T \times (1.0 - (1.0 - \text{cooling\_rate}) \times 0.5)$ |
| Normál (0.2 - 0.6) | Szabványos hűtés | $T = T \times \text{cooling\_rate}$ |

 Újramelegítés és stagnálás kezelése

Annak megakadályozására, hogy az optimalizáló olyan lokális minimumok csapdájába essen, ahol hosszú ideig nem fogadnak el lépéseket, egy újramelegítési (reheat) mechanizmus aktiválódik.

 Stagnálás trigger
Az optimize ciklus fenntart egy stagnation_counter-t. Ha az energia nem javul a stagnation_limit (alapértelmezés: 100) által meghatározott számú iteráción keresztül, a rendszer újramelegítést hajt végre.

 Újramelegítési mechanizmus
Egy újramelegítés során az aktuális hőmérsékletet megszorozzák a reheat_factor-ral (alapértelmezés: 1.5 vagy 2.0). Ez növeli az energianövelő lépések elfogadásának valószínűségét, lehetővé téve az optimalizáló számára, hogy kiugorjon az aktuális lokális minimumból.

 Hőmérsékleti padló
A hűtéstől vagy újramelegítéstől függetlenül a hőmérséklet soha nem eshet a min_temperature alá. Ez biztosítja, hogy a rendszer fenntartson egy alapvető sztochaszticitási szintet, amíg el nem éri a max_iterations-t vagy a konvergencia küszöböt.



 Célfüggvények

Az ESSO optimalizáló egy ObjectiveFunction-t használ egy adott állapot energiájának számszerűsítésére a szimulált hűtési folyamat során. Ez az érték határozza meg a gráf topológiájának és a kvantumállapotoknak a megfelelőségét (fitness), irányítva a sztochasztikus keresést egy optimális konfiguráció felé.

 Az ObjectiveFunction típus

Az esso_optimizer.zig-ben az ObjectiveFunction egy függvénymutatóként van definiálva, amely egy csak olvasható mutatót vesz át az aktuális OptimizationState-re, és egy f64 energiaértéket ad vissza.

Az alacsonyabb energiaértékek kívánatosabb állapotokat képviselnek. Az OptimizationState hozzáférést biztosít a függvény számára a teljes gráf topológiához, a csomópontfázisokhoz, a qubit amplitúdókhoz és az aktuális összefonódási térképhez ezen érték kiszámításához.



 Beépített célfüggvények

Az ESSO négy előre definiált célfüggvényt biztosít, amelyek különböző optimalizációs célokat céloznak meg. Ezek az általános célú gráfkiegyensúlyozástól a specifikus kvantumkoherencia metrikákig terjednek.

| Függvény | Elsődleges cél | Főbb metrikák |
| : | : | : |
| defaultGraphObjective | Általános egyensúly | Élsúlyok, fraktáldimenziók, csomópontfázisok. |
| connectivityObjective | Topológia | Él-csomópont arány és átlagos kapcsolati erősség. |
| quantumCoherenceObjective | Kvantum stabilitás | Qubit amplitúdó magnitúdó és összefonódási korrelációk. |
| fractalDimensionObjective | Önhasonlóság | Eltérés egy cél fraktáldimenziótól (1.5). |



 Egyéni célfüggvények

A felhasználók implementálhatják saját logikájukat, hogy az optimalizálót tartomány-specifikus célok felé irányítsák. Egy egyéni függvénynek meg kell egyeznie az ObjectiveFunction aláírással, és beinjektálható a motorba a setObjectiveFunction segítségével, vagy közvetlenül átadható az optimize metódusnak.

 Adathozzáférés
Egy egyéni függvényen belül az OptimizationState a következőket biztosítja:
 Gráf topológia: Hozzáférés az nsir_core.Node és nsir_core.Edge adatokhoz.
 Kvantum adatok: Qubit amplitúdók és fázisok.
 Összefonódás: Az entanglement_map, amely EntanglementInfo-t tartalmaz a csomópontpárokhoz.



 Beépített célfüggvények

Ez az oldal részletes technikai referenciát nyújt az ESSO (Entangled Stochastic Symmetry Optimizer) motor által biztosított beépített célfüggvényekhez. Ezek a függvények energiaértékelési lépésként szolgálnak a szimulált hűtési ciklusban, meghatározva egy gráfállapot megfelelőségét.

Az optimalizáló ezen függvények visszatérési értékét Energiaként ($E$) kezeli. Az optimalizációs folyamat célja ennek az értéknek a minimalizálása.

 Célfüggvény interfész áttekintése

Az esso_optimizer.zig-ben egy célfüggvény egy függvénymutatóként van definiálva, amely egy konstans mutatót fogad el az aktuális OptimizationState-re, és egy 64 bites lebegőpontos számot ad vissza.

Az OptimizationState hozzáférést biztosít a függvény számára a gráf topológiájához, a csomópont állapotokhoz és az összefonódási térképhez.



 1. defaultGraphObjective

A defaultGraphObjective egy általános célú energiafüggvény. Kiegyensúlyozza a gráf strukturális tulajdonságait (élsúlyok) a kvantumtulajdonságokkal (csomópontfázisok) és a geometriai komplexitással (fraktáldimenziók).

Matematikai képlet:
$$E = \sum \text{edge\_weights} + \sum \sin(\text{node\_phases}) + \sum |\text{fractal\_dimension} - 1.0|$$

Implementációs részletek:
 Élsúlyok: Végigiterál a gráf összes élén, és összegzi a weight értékeiket.
 Csomópontfázisok: Végigiterál az összes csomóponton, és hozzáadja a csomópont QuantumState-jének phase attribútumának szinuszát.
 Fraktáldimenzió: Bünteti azokat a csomópontokat, ahol a fractal_dimension eltér az 1.0-s alapvonaltól.

Adatfolyam:
1. Hozzáfér a state.graph.edges-hez a súlyok aggregálásához.
2. Hozzáfér a state.graph.nodes-hoz a fázisok és fraktál metrikák aggregálásához.



 2. connectivityObjective

Ezt a célt a gráf topológiájának optimalizálására tervezték, előnyben részesítve a jól kapcsolódó gráfokat kiváló minőségű kapcsolatokkal. Célja az energia minimalizálása az él-csomópont arány és ezen élek átlagos súlyának maximalizálásával.

Matematikai képlet:
$$E = \frac{1.0}{1.0 + (\text{edge\_count} / \text{node\_count})} + \frac{1.0}{1.0 + \text{avg\_edge\_weight}}$$

Főbb jellemzők:
 Sűrűség büntetés: Ahogy az élek és csomópontok aránya növekszik, az első tag a nulla felé közelít, csökkentve az energiát.
 Súly büntetés: Ahogy az élek átlagos súlya növekszik, a második tag a nulla felé közelít.
 Biztonság: Tartalmaz ellenőrzéseket a nullával való osztás megakadályozására, ha a gráf üres.



 3. quantumCoherenceObjective

Ez a függvény a gráfban lévő kvantumállapotok stabilitására és korrelációjára összpontosít. Akkor használatos, amikor az optimalizálási cél egy koherens összefonódott állapot elérése.

Matematikai képlet:
$$E = (1.0 - \text{avg\_amplitude}) + (1.0 - \text{entanglement\_percentage}) + \sum \text{phase\_variance}$$

Implementációs komponensek:
 Amplitúdó magnitúdó: Kiszámítja a qubit amplitúdók átlagos magnitúdóját ($\sqrt{re^2 + im^2}$). A magasabb amplitúdók csökkentik az energiát.
 Globális összefonódás: Közvetlenül használja az OptimizationState által kiszámított entanglement_percentage-et. A magasabb összefonódás csökkenti az energiát.
 Fázis koherencia: Méri a fázisok varianciáját az összes csomóponton. Az alacsonyabb variancia (szinkronizáltabb fázisok) alacsonyabb energiát eredményez.



 4. fractalDimensionObjective

Ez a cél kifejezetten a SelfSimilarRelationalGraph önhasonlósági aspektusait célozza meg. Az 1.5-ös fraktáldimenziót tekinti ideális állapotnak (amely gyakran az egyszerű euklideszi geometria és a komplex térkitöltő zaj közötti egyensúlyt képviseli).

Matematikai képlet:
$$E = \sum |\text{node.fractal\_dimension} - 1.5|$$

Implementációs részletek:
 Végigiterál minden csomóponton a state.graph.nodes-ban.
 Kiszámítja az abszolút különbséget a csomópont aktuális fractal_dimension értéke és az 1.5 konstans között.
 Ezen különbségek összegzése biztosítja, hogy az optimalizáló a teljes gráfot egy egységes fraktál komplexitás felé mozdítsa el.



 Végrehajtási folyamat az optimalizálón belül

A célfüggvények a fő optimalizációs ciklus során hívódnak meg az optimize()-on belül. Kifejezetten egy lépés (perturbáció) alkalmazása után hívják meg őket annak meghatározására, hogy az új állapot jobb-e, mint az előző.

Főbb kód referenciák a végrehajtáshoz:
 Függvény kiválasztása: A célfüggvény az optimalizáló konfigurációjában van tárolva.
 Hívás helye: A függvény az optimize cikluson belül fut le a current_energy kiszámításához.
 Állapot hozzáférés: A függvénynek átadott OptimizationState tartalmazza a SelfSimilarRelationalGraph-ot.



 Egyéni célfüggvények

Ez az oldal technikai útmutatót nyújt egyéni ObjectiveFunction logika implementálásához és injektálásához az EntangledStochasticSymmetryOptimizer-be. Egyéni energiatájképek definiálásával a felhasználók a szimulált hűtési folyamatot specifikus gráf topológiák vagy kvantumállapot konfigurációk felé irányíthatják.

 Áttekintés és cél

Az EntangledStochasticSymmetryOptimizer úgy működik, hogy minimalizálja az ObjectiveFunction által visszaadott energiaértéket. Bár a rendszer beépített függvényeket biztosít az általános gráf egészséghez és a kvantumkoherenciához, a komplex tartomány-specifikus problémák gyakran egyéni heurisztikákat igényelnek.

Egy egyéni célfüggvény teljes, csak olvasható hozzáféréssel rendelkezik az OptimizationState-hez, lehetővé téve a rendszer megfelelőségének értékelését a következők alapján:
 Gráf topológia: Szomszédság, élsúlyok és fraktáldimenziók.
 Kvantumállapotok: Csomópontfázisok és a qubitek komplex amplitúdói.
 Összefonódás: A csomópontpárok közötti korrelációk erőssége és eloszlása.
 Szimmetria: Detektált geometriai minták és azok rezonanciafrekvenciái.

 Implementációs részletek

 Az ObjectiveFunction típus
Az esso_optimizer.zig-ben egy célfüggvény konstans függvénymutatóként van definiálva.

 Hozzáférhető adatok: OptimizationState
Az OptimizationState struct szolgál elsődleges adatszolgáltatóként a célfüggvény számára. Tartalmazza az optimalizált rendszer aktuális pillanatképét.

| Mező | Típus | Leírás |
| : | : | : |
| graph | SelfSimilarRelationalGraph | A csomópontokat és éleket tartalmazó alap gráfstruktúra. |
| entanglement_map | AutoHashMap(NodePairKey, EntanglementInfo) | Nyomon követi az aktív kvantumkorrelációkat a csomópontok között. |
| entanglement_percentage | f64 | Az összefonódott párok aránya az összes lehetséges párhoz képest. |
| iteration | usize | Az aktuális lépés az optimalizációs ciklusban. |

 Az egyéni függvény injektálása

Két elsődleges módja van egy egyéni célfüggvény regisztrálásának az optimalizálóval.

 1. A setObjectiveFunction használata
A célfüggvényt bármikor megváltoztathatja az optimize() meghívása előtt a beállító metódus használatával.

 2. Átadás az optimize()-nak
Az optimize függvény elfogad egy opcionális ObjectiveFunction-t. Ha null-t ad át, akkor a példány self.objective_fn-jére alapértelmeződik (amely alapértelmezés szerint a defaultGraphObjective).

 Bevált gyakorlatok energiafüggvényekhez

Annak biztosítására, hogy a szimulált hűtési motor hatékonyan konvergáljon, az egyéni függvényeknek be kell tartaniuk ezeket a numerikus elveket:

1. Numerikus stabilitás: Kerülje az inf vagy nan visszaadását. Ha egy állapot érvénytelen, adjon vissza egy nagyon magas véges értéket (pl. 1e10).
2. Simaság: A csomópontfázisok vagy élsúlyok kis változásainak (0, 1 és 4-es lépéstípusok) ideális esetben kis energiaváltozásokat kell eredményezniük. A nem folytonos energiatájképek megnehezítik az optimalizáló számára a gradiensek megtalálását.
3. Normalizálás: Az optimalizáló alapértelmezett kezdeti hőmérséklete 100.0. Ha az energiaértékei az 1e-9 tartományban vannak, a hőmérséklet túl magas lesz a konvergencia lehetővé tételéhez. Törekedjen a 0.1 és 10.0 közötti energia deltákra.
4. Hatékonyság: A célfüggvény minden egyes iterációban meghívásra kerül (akár max_iterations-ig, alapértelmezés szerint 10,000). Kerülje az O(N²) műveleteket a függvényen belül, ha lehetséges. Használja az előre kiszámított entanglement_percentage-et a térkép újra-iterálása helyett.



 Memóriakezelés és biztonság

Az Entangled Stochastic Symmetry Optimizer (ESSO) egy explicit memóriakezelési modellt alkalmaz, amely a Zig Allocator interfészén alapul. Mivel az optimalizációs folyamat gyakori gráfmutációkat, mély másolatokat és potenciális visszaállításokat foglal magában, a rendszer szigorú tulajdonosi mintát használ a memóriaszivárgások és a lógó mutatók (dangling pointers) megelőzésére.

Ennek a modellnek a magja az allocator-per-struct minta, ahol minden dinamikus memóriát igénylő struktúra tárol egy hivatkozást a létrehozásához használt Allocator-ra. Ez biztosítja, hogy a struktúra függetlenül kezelhesse saját és gyermekei életciklusát.

 Memória életciklus áttekintése

Az ESSO memóriakezelése az OptimizationState és az EntangledStochasticSymmetryOptimizer köré összpontosul. Az optimalizáló fenntart egy legjobb állapotot és egy aktuális állapotot, gyakran klónozva a gráf topológiáját a megoldástér felfedezéséhez.

 Főbb biztonsági mechanizmusok

A kódbázis számos idiómára támaszkodik a memóriabiztonság biztosítása érdekében az optimize() ciklus nagyfrekvenciás iterációi során:

1. Explicit tulajdonjogi jelzők: Az OptimizationState struct tartalmaz egy owns_graph logikai értéket. Ha true-ra van állítva, a deinit() metódus meghívja a graph.deinit() függvényt. Ez lehetővé teszi az optimalizáló számára, hogy gráf hivatkozásokat adjon át az állapotok között véletlen dupla felszabadítások vagy szivárgások nélkül.
2. Hiba halasztások (errdefer): Az allokációs logika során, különösen a cloneGraph és az OptimizationState.clone esetében, az errdefer használatos a részlegesen lefoglalt struktúrák tisztítására, ha egy al-allokáció meghiúsul.
3. Pillanatkép készítés: Komplex lépések (mint a toggleRandomEdge) során az UndoLog rögzíti a gráf teljes mély másolatát a cloneGraph használatával, hogy tökéletes visszaállítási pontot biztosítson, ha a lépést elutasítják.

 Kód entitás leképezés

Ez a táblázat feltérképezi a természetes nyelvi memóriafogalmakat a kódban lévő specifikus implementációs entitásaikhoz.

| Fogalom | Kód entitás |
| : | : |
| Mély másolás segédprogram | cloneGraph(allocator, source) |
| Állapot duplikáció | OptimizationState.clone(allocator) |
| Minta perzisztencia | SymmetryPattern.clone(allocator) |
| Visszaállítási puffer | UndoLog |
| Tisztítási logika | deinit() (minden fő struct-on implementálva) |



 Gyermek oldalak

 Tulajdonosi modell és mély másolás
Ez az oldal részletezi az allocator-per-struct mintát és a cloneGraph() segédprogram mechanikáját. Elmagyarázza, hogyan kezeli az OptimizationState a belső entanglement_map-jét és az owns_graph jelzőt a memóriakorrupció megelőzése érdekében az állapotátmenetek során.

 UndoLog memóriabiztonság
Ez az oldal az UndoLog memóriabiztonsági garanciáira összpontosít az applyMove és undoMove ciklus során. Dokumentálja, hogyan kezelik a pillanatképeket a strukturális változásokhoz (6-os lépéstípus), és hogyan akadályozza meg a clear() metódus az elavult allokációk felhalmozódását az iterációk között.



 Tulajdonosi modell és mély másolás

Ez a szakasz részletezi az esso_optimizer.zig-ben alkalmazott memóriakezelési stratégiákat. A rendszer egy szigorú allocator-per-struct mintát, mély másolási segédprogramokat használ a komplex gráfstruktúrákhoz, és robusztus hibakezelést alkalmaz a memóriabiztonság biztosítása érdekében a sztochasztikus optimalizációs folyamatok során.

 Allocator-Per-Struct minta

A kódbázis egy szigorú konvenciót követ, ahol minden dinamikus memóriafoglalást igénylő struct tárol egy hivatkozást egy std.mem.Allocator-ra, és biztosít init és deinit metódusokat. Ez a minta biztosítja, hogy a lefoglalt memória élettartama közvetlenül az objektumpéldányhoz kötődjön.

Az ezt a mintát követő főbb entitások a következők:
 OptimizationState: Kezeli a gráfot és az összefonódási térképeket.
 SymmetryPattern: Kezeli a detektált szimmetriában részt vevő csomópontok listáit.
 EntangledStochasticSymmetryOptimizer: Kezeli a történeti puffereket és a PRNG állapotot.

 Gráf mély másolása a cloneGraph() segítségével

Mivel az optimalizációs folyamat magában foglalja a szomszédos állapotok felfedezését a gráf módosításával, a rendszer gyakran igényli a SelfSimilarRelationalGraph független másolatait. A cloneGraph() függvény a teljes gráf topológia mély másolatát (deep copy) biztosítja, beleértve az összes csomópontot és élt.

 Implementációs részletek
A függvény a következő lépéseket hajtja végre:
1. Inicializálás: Létrehoz egy új SelfSimilarRelationalGraph-ot a megadott allokátor használatával.
2. Csomópont klónozás: Végigiterál a forrásgráf összes csomópontján, és mindegyikre meghívja a node.clone(allocator) függvényt.
3. Él klónozás: Végigiterál az összes éllistán, és meghívja az edge.clone(allocator) függvényt minden élre, újra beillesztve őket az új gráfstruktúrába.
4. Metaadat átvitel: Átmásolja a topology_hash-t, hogy biztosítsa, a klón megkülönböztethetetlen a forrástól a hash-elési célokra.

 OptimizationState tulajdonjog és owns_graph

Az OptimizationState struct konténerként működik a gráf és a hozzá tartozó kvantum/összefonódási metaadatok számára. Egy kritikus mező ebben a struct-ban az owns_graph: bool jelző.

| Mező | Cél |
| : | : |
| graph | Mutató a SelfSimilarRelationalGraph példányra. |
| owns_graph | Meghatározza, hogy a deinit() meghívja-e a graph.deinit() függvényt. |
| entanglement_map | Egy hash map, amely az EntanglementInfo-t tárolja a csomópontpárokhoz. |

 Életciklus logika
 init(): Jellemzően true-ra állítja az owns_graph értékét, ha az állapot felelős a gráf memóriájáért.
 deinit(): Felszabadítja az entanglement_map-et. Ha az owns_graph igaz, akkor meghívja a graph.deinit() függvényt is.
 clone(): Létrehoz egy új OptimizationState-et egy mélyen másolt gráffal és egy duplikált összefonódási térképpel.

 Biztonsági minták és hibakezelés

A kódbázis kiterjedten használja a Zig errdefer és defer mintáit a memóriaszivárgások megelőzésére a komplex allokációs sorozatok során.

 Részleges allokációs hiba
Az OptimizationState.clone()-ban több allokáció történik (a gráf klónozása, az állapot inicializálása és a térkép feltöltése). Ha bármelyik lépés meghiúsul, az errdefer biztosítja a korábban lefoglalt erőforrások tisztítását.

 SymmetryPattern klónozás
Hasonlóképpen, a SymmetryPattern.clone() biztosítja, hogy ha az új csomópontlista allokációja meghiúsul, maga a struct ne szivárogjon.

| Függvény | Allokációs cél | Biztonsági mechanizmus |
| : | : | : |
| cloneGraph | new_graph | errdefer new_graph.deinit() |
| SymmetryPattern.clone | cloned.nodes | errdefer self.allocator.destroy(cloned) |
| OptimizationState.clone | new_state | errdefer new_state.deinit() |



 UndoLog memóriabiztonság

Az UndoLog az EntangledStochasticSymmetryOptimizer kritikus komponense, amely biztosítja a memóriabiztonságot és az állapotkonzisztenciát az iteratív optimalizációs folyamat során. Mivel az optimalizáló egy Metropolis-Hastings ihletésű megközelítést alkalmaz — ahol a lépéseket gyakran javasolják és ezt követően elutasítják —, az UndoLog mechanizmust biztosít a SelfSimilarRelationalGraph korábbi állapotának visszaállítására memóriaszivárgás vagy lógó mutatók hátrahagyása nélkül.

 Implementáció és adatfolyam

Az UndoLog minimális állapotot tárol, amely egy specifikus transzformáció visszafordításához szükséges. Nyomon követi a végrehajtott lépés típusát, és gyorsítótárazza a módosított entitások eredeti értékeit.

 Főbb struktúrák

Az UndoLog struct olyan mezőkkel van definiálva, amelyek a változások különböző granularitásait kezelik, az egyedi élsúlyoktól a teljes gráf topológia pillanatképekig.

 Adatfolyam: Lépés alkalmazása és visszaállítása

Az optimalizációs ciklus az optimize()-ban egy szigorú Alkalmaz-Értékel-Visszaállít mintát követ. Az UndoLog interakciót a defer minta kezeli a tisztítás biztosítása érdekében.

1. Inicializálás: Egy UndoLog inicializálódik, mielőtt egy lépést megkísérelnének.
2. Lépés alkalmazása: Az applyMove feltölti a naplót. Egyszerű perturbációk (0, 1, 4, 5 típusok) esetén a korábbi értékeket az edge_weights vagy node_states mezőkben tárolja. Topológiai változások (6-os típus) esetén a teljes gráfot az old_graph-ba klónozza.
3. Értékelés: Az acceptMove függvény meghatározza, hogy a lépés megmarad-e.
4. Visszaállítás: Ha az acceptMove false-t ad vissza, az undoMove meghívásra kerül az állapot visszaállítására a naplóból.
5. Tisztítás: A defer log.deinit() biztosítja, hogy a naplón belül lefoglalt összes memória (beleértve az old_graph pillanatképet is) felszabaduljon.



 A 6-os lépéstípus (toggleRandomEdge) kezelése

A 6-os lépéstípus (toggleRandomEdge) egyedi, mert a gráf topológiáját módosítja (élek hozzáadása vagy eltávolítása), nem pedig csak az attribútumokat perturbálja. Mivel a SelfSimilarRelationalGraph komplex belső térképeket használ a szomszédsághoz, egy egyszerű attribútum visszaállítás nem elegendő.

 Pillanatkép mechanizmus
Amikor egy 6-os típusú lépés elindul, az UndoLog létrehozza a gráf teljes mély másolatát a cloneGraph használatával. Ezt a pillanatképet az old_graph mező tárolja.

 Memóriabiztonság az undoMove-ban
Ha egy 6-os típusú lépést elutasítanak, az undoMove egy mutató cserét és tulajdonjog átruházást hajt végre:
1. Az aktuális (módosított) gráf de-inicializálódik a szivárgások megelőzése érdekében.
2. Az OptimizationState.graph mutató frissül, hogy az old_graph pillanatképre mutasson.
3. A naplóban lévő old_graph mező null-ra van állítva, hogy megakadályozza a dupla felszabadítást a log.deinit() során.



 Életciklus és erőforrás-kezelés

Az UndoLog egy explicit clear() és deinit() mintát használ a belső gyűjteményeinek élettartamának kezelésére.

 A clear() függvény
A clear() metódus minden új lépéskísérlet előtt meghívásra kerül a cikluson belül. Biztosítja, hogy a térképek (edge_weights, node_states) és az ArrayList (added_entanglements) kiürüljenek, és minden meglévő old_graph pillanatkép felszabaduljon.

 A deinit() függvény
A deinit() függvény a végső tisztítási lépés. Döntő fontosságú a szivárgások megelőzésében az optimize() ciklusban. Meghívja a clear() függvényt a pillanatkép felszabadítására, majd folytatja maguknak a hash map-eknek és array list-eknek a de-inicializálását.

 Interakció az optimize() ciklussal
Az optimize() függvény az errdefer és defer használatával biztosítja, hogy még ha egy allokáció meghiúsul is egy lépés alkalmazása során, a rendszer érvényes állapotban maradjon.

| Függvény | Felelősség |
| : | : |
| UndoLog.init | Lefoglalja a HashMap és ArrayList struktúrákat. |
| UndoLog.clear | Felszabadítja az old_graph pillanatképet és törli a térkép bejegyzéseket. |
| UndoLog.undoMove | Visszaállítja az állapotot az OptimizationState-be a move_type alapján. |
| UndoLog.deinit | A napló által birtokolt összes memória végső felszabadítása. |



 Szójegyzék

Ez a szójegyzék meghatározza az esso_optimizer.zig kódbázisban használt technikai terminológiát, tartomány-specifikus zsargont és Zig-specifikus idiómákat. Referenciaként szolgál a mérnökök számára az Entangled Stochastic Symmetry Optimizer (ESSO) alapjául szolgáló matematikai és számítási fogalmak megértéséhez.

 Alapvető tartományi kifejezések

 Összefonódás (Entanglement)
Az ESSO kontextusában az összefonódás egy kiszámított korrelációra utal két csomópont között egy SelfSimilarRelationalGraph-ban. A tiszta kvantum-összefonódással ellentétben ez egy szimulált metrika, amelyet az optimalizációs lépések torzítására használnak.
 Korrelációs erősség (Correlation Strength): Egy skaláris érték, amely a kapcsolat intenzitását képviseli.
 Fáziskülönbség (Phase Difference): A relatív különbség a kvantumfázisok között két összefonódott csomópont esetében.
 Bomlási tényező (Decay Factor): Egy felezési idő képlettel kiszámított érték az összefonódás erősségének időbeli csökkentésére, ha nem erősítik meg.

 Szimmetria transzformáció (Symmetry Transformation)
A gráf állapotára alkalmazott geometriai vagy algebrai művelet. Az optimalizáló ezeket használja nagy léptékű lépések végrehajtására, amelyek tiszteletben tartják a probléma alapul szolgáló topológiáját.
 Szimmetriacsoport (Symmetry Group): A támogatott transzformációk enumerációja: identity, reflection, rotation_90, rotation_180, rotation_270 és translation.
 Izometria (Isometry): Egy transzformáció, amely megőrzi a távolságokat, jellemzően a forgatási mátrix determinánsán keresztül ellenőrizve.

 Szimulált hűtés kifejezések (Simulated Annealing Terms)
 Hőmérséklet ($T$): Egy globális paraméter, amely szabályozza a rossz lépések (az energiát növelő lépések) elfogadásának valószínűségét. A magas $T$ lehetővé teszi a felfedezést; az alacsony $T$ lehetővé teszi a finomítást.
 Hűtési sebesség (Cooling Rate): Az a tényező, amellyel a hőmérsékletet minden iterációban megszorozzák, hogy a rendszert egy lokális minimumba fagyasszák.
 Újramelegítés (Reheat): Egy mechanizmus a lokális minimumokból való kilépésre a $T$ hirtelen növelésével, amikor a rendszer stagnál.

 Technikai definíciók táblázata

| Kifejezés | Implementációs részlet |
| : | : |
| NodePairKey | Egy struct, amely biztosítja a csomópont ID-k kanonikus rendezését (kisebb ID először), hogy a párokat irányítatlanként kezelje. |
| UndoLog | Egy memento struktúra, amely tárolja a módosított csomópontok/élek korábbi állapotát, hogy lehetővé tegye az elutasított lépések $O(1)$ vagy $O(N)$ visszaállítását. |
| Metropolis kritérium | A logika az acceptMove-ban, ahol $P(accept) = \exp(-\Delta E / T)$. |
| Stagnálás | Egy állapot, ahol a best_energy nem javult stagnation_limit iteráción keresztül. |
| Fraktáldimenzió | A gráf egy tulajdonsága, amelyet a fractalDimensionObjective-ben használnak a nem optimális strukturális komplexitás büntetésére. |

 Zig idiómák a kódbázisban

 errdefer
Kiterjedten használják a memóriabiztonság érdekében komplex allokációk során. Ha egy többlépéses inicializálás félúton meghiúsul, az errdefer biztosítja a korábban lefoglalt memória felszabadítását.
 Példa: A cloneGraph-ban a new_graph inicializálódik, és az errdefer new_graph.deinit() biztosítja a tisztítást, ha a csomópont/él klónozás meghiúsul.

 Payload minták
Az optimalizáló gyakran használja a while (iter.next()) |entry| vagy if (opt) |val| szerkezeteket az opcionális típusok és hash map bejegyzések biztonságos kicsomagolására.
 Példa: Iterálás a gráf csomópontjain szimmetria transzformációk alkalmazásához.

 Allokátor átadása
A Zig filozófiájával összhangban nincsenek rejtett allokációk. Minden memóriát igénylő struct (mint az OptimizationState vagy a SymmetryPattern) elfogad egy Allocator-t az init vagy clone metódusaiban.
