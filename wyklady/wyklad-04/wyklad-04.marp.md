---
marp: false
theme: default
paginate: true
header: 'Analiza Dużych Zbiorów Danych | W04: Rozproszone przetwarzanie wsadowe: Apache Spark'
footer: 'Uniwersytet Warmińsko-Mazurski w Olsztynie'
style: |
  section {
    font-family: 'Segoe UI', -apple-system, BlinkMacSystemFont, Roboto, sans-serif;
    font-size: 24px;
    padding: 38px 48px;
    color: #2d3748;
  }
  h1 {
    color: #1a365d;
    font-size: 38px;
  }
  h2 {
    color: #2b6cb0;
    border-bottom: 2px solid #e2e8f0;
    padding-bottom: 6px;
    font-size: 30px;
    margin-top: 0;
  }
  h3 {
    color: #4a5568;
    font-size: 24px;
    margin-bottom: 8px;
  }
  footer {
    font-size: 13px;
    color: #a0aec0;
  }
  header {
    font-size: 13px;
    color: #a0aec0;
  }
  .accent {
    color: #c53030;
    font-weight: bold;
  }
  .blue {
    color: #2b6cb0;
    font-weight: bold;
  }
  .box-info {
    background: #ebf8ff;
    border-left: 6px solid #3182ce;
    padding: 10px 14px;
    border-radius: 4px;
    margin-top: 14px;
    font-size: 21px;
  }
  .box-warning {
    background: #fffaf0;
    border-left: 6px solid #dd6b20;
    padding: 10px 14px;
    border-radius: 4px;
    margin-top: 14px;
    font-size: 21px;
  }
  .box-dark {
    background: #2d3748;
    color: #edf2f7;
    padding: 12px 16px;
    border-radius: 6px;
    margin-top: 14px;
    font-size: 20px;
    line-height: 1.4;
  }
  pre {
    background: #1a202c;
    color: #edf2f7;
    font-size: 18px;
    border-radius: 6px;
    padding: 10px;
  }
  table {
    font-size: 19px;
    border-collapse: collapse;
    width: 100%;
  }
  th {
    background: #edf2f7;
    color: #2d3748;
    padding: 7px;
  }
  td {
    padding: 7px;
    border-bottom: 1px solid #e2e8f0;
  }
  .source {
    font-size: 13px;
    color: #718096;
    margin-top: 12px;
    border-top: 1px dashed #cbd5e0;
    padding-top: 4px;
    font-style: italic;
  }

  /* --- WIZUALNE KOMPONENTY ARCHITEKTONICZNE --- */
  .flow-row {
    display: flex;
    align-items: stretch;
    justify-content: space-between;
    gap: 12px;
    margin: 16px 0;
  }
  .flow-arrow-h {
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
    color: #4a5568;
    font-weight: bold;
    padding: 0 4px;
    user-select: none;
  }
  .flow-arrow-v {
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 20px;
    color: #4a5568;
    font-weight: bold;
    margin: 3px 0;
    user-select: none;
  }
  .card-node {
    background: #ffffff;
    border: 2px solid #e2e8f0;
    border-radius: 8px;
    padding: 11px 13px;
    flex: 1;
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.07);
    font-size: 17px;
    line-height: 1.35;
  }
  .card-node h4 {
    margin: 0 0 4px 0;
    font-size: 18px;
    color: #2b6cb0;
  }
  .card-node .sub {
    font-size: 13px;
    color: #718096;
    margin-bottom: 6px;
    display: block;
    font-weight: 600;
  }
  .card-node ul {
    margin: 4px 0 0 16px;
    padding: 0;
    font-size: 15px;
    color: #4a5568;
  }
  .card-node li {
    margin-bottom: 2px;
  }
  
  .card-blue { border-top: 5px solid #3182ce; background: #ebf8ff; }
  .card-green { border-top: 5px solid #38a169; background: #f0fff4; }
  .card-purple { border-top: 5px solid #805ad5; background: #faf5ff; }
  .card-orange { border-top: 5px solid #dd6b20; background: #fffaf0; }
  .card-red { border-top: 5px solid #e53e3e; background: #fff5f5; }
  .card-bronze { border-top: 6px solid #b7791f; background: #fffaf0; }
  .card-silver { border-top: 6px solid #718096; background: #f7fafc; }
  .card-gold { border-top: 6px solid #d69e2e; background: #fffff0; }

  .stack-tier {
    background: #ffffff;
    border: 1px solid #cbd5e0;
    border-left: 6px solid #3182ce;
    border-radius: 6px;
    padding: 8px 14px;
    margin-bottom: 8px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 17px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.04);
  }
  .tier-title {
    font-weight: bold;
    color: #1a365d;
  }
  .tier-desc {
    color: #4a5568;
    font-size: 15px;
  }

  .grid-2 {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 14px;
    margin: 14px 0;
  }
  .grid-3 {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 12px;
    margin: 14px 0;
  }
  .grid-4 {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    gap: 10px;
    margin: 14px 0;
  }
  .badge {
    display: inline-block;
    padding: 2px 7px;
    border-radius: 4px;
    font-size: 11px;
    font-weight: bold;
    text-transform: uppercase;
  }
  .badge-blue { background: #bee3f8; color: #2b6cb0; }
  .badge-green { background: #c6f6d5; color: #22543d; }
  .badge-purple { background: #e9d8fd; color: #553c9a; }
  .badge-red { background: #fed7d7; color: #9b2c2c; }
  .badge-yellow { background: #fefcbf; color: #744210; }
---

<!-- _class: lead -->
<!-- _paginate: false -->
<!-- _header: '' -->

# Analiza Dużych Zbiorów Danych
## Wykład 4: Rozproszone przetwarzanie wsadowe: Apache Spark

**dr inż. Krzysztof Ropiak**
Uniwersytet Warmińsko-Mazurski w Olsztynie
*Wydział Matematyki i Informatyki*

<!--
NOTATKI DLA WYKŁADOWCY:
- Czas łączny: 90 minut w formule ciągłej.
- Kontekst kursu: Na W03 poznaliśmy fizykę składowania danych (Parquet, Arrow, Delta Lake, Iceberg). Dziś przechodzimy do warstwy 3 (COMPUTE & TRANSFORMATION) – czyli silnika obliczeniowego, który potrafi przetworzyć terabajty tych plików równolegle na dziesiątkach serwerów.
- Cel wykładu: Zrozumieć architekturę Apache Spark (od RDD i grafu DAG do Catalysta, Tungstena i AQE) oraz nauczyć studentów diagnozowania i naprawiania trzech najgroźniejszych patologii produkcyjnych: Shuffle, Out Of Memory (OOM) oraz Data Skew.
-->

---

## Plan wykładu

* **1. Od MapReduce do In-Memory Computing i Architektura Apache Spark**
  Dlaczego dyskowy MapReduce przegrał z czasem • Rewolucja RDD i odporność na awarie przez *Lineage Graph* • Architektura fizyczna klastra (Driver, Executors, Cluster Manager) • Hierarchia wykonania (Job ➔ Stage ➔ Task) • Ewolucja API • Lazy Evaluation i transformacje *Narrow* vs *Wide*
* **2. Realia produkcyjne: Diagnoza padającego joba (Shuffle, OOM, Data Skew)**
  Studium przypadku "TelecomPol" (syndrom 99% postępu) • Anatomia operacji *Shuffle* • Model *Unified Memory Manager* • Dwa oblicza błędu `OutOfMemoryError` • Skośność danych (*Data Skew*) • Pułapka PySpark UDF vs Apache Arrow • Diagnostyka w **Spark UI**
* **3. Nowoczesne rozwiązanie: Silnik optymalizacyjny pod maską**
  Kompilator zapytań **Catalyst Optimizer** • Silnik wykonawczy **Project Tungsten** (*Off-Heap Memory* i *Whole-Stage CodeGen*) • Rewolucja **Adaptive Query Execution (AQE)** • Techniki *Key Salting*, `repartition` vs `coalesce` oraz `cache()`
* **4. Synteza inżynierska, natywne silniki C++/Rust i dyskusja**
  Zmierzch czystego JVM (Photon, Velox, Comet) • Złota macierz strategii złączeń (Joins) • Dwa produkcyjne scenariusze decyzyjne • Podsumowanie

<!--
NOTATKI DLA WYKŁADOWCY:
Zapowiedzieć studentom, że dzisiejszy wykład to "anatomia silnika odrzutowego". Większość programistów pisze w Sparku na ślepo, dopóki zadanie nie wybuchnie w nocy na produkcji. Po tym wykładzie studenci będą rozumieć, co dzieje się z każdym bajtem w pamięci i sieci.
-->

---

## 01. Dlaczego klasyczny MapReduce przegrał z czasem?

W modelu **Hadoop MapReduce** (Wykład 1) każdy etap obliczeń był zmuszony do pełnej materializacji wyników pośrednich na dyskach systemu HDFS z 3-krotną replikacją:

```
Klasyczny Hadoop MapReduce (np. 3-etapowe zapytanie lub algorytm iteracyjny ML / PageRank):
[HDFS Read] ➔ (Map 1 ➔ Reduce 1) ➔ [Zapis HDFS 3x] ➔ [Odczyt HDFS] ➔ (Map 2 ➔ Reduce 2) ➔ [Zapis HDFS 3x]
                                         ▲                 ▲
                       90% czasu zadania to czekanie na dyskowe I/O, sieć i serializację!

Apache Spark (In-Memory DAG Execution):
[Storage Read] ➔ (Etap 1 w RAM) ──────► (Etap 2 w RAM) ──────► (Etap 3 w RAM) ➔ [Wynik końcowy]
                                   ▲                       ▲
                    Potokowe przetwarzanie w pamięci operacyjnej (10x – 100x szybciej)
```

<div class="grid-2">
  <div class="card-node card-red">
    <h4>Wąskie gardło MapReduce</h4>
    <ul>
      <li>Brak możliwości utrzymania danych pomiędzy krokami w pamięci RAM.</li>
      <li>Sztywny dwuetapowy szablon (każdy problem trzeba wyrazić jako jedną parę <code>Map</code> + <code>Reduce</code>).</li>
      <li>Koszmarna wydajność przy uczeniu maszynowym (np. K-Means, regresja logistyczna) i zapytaniach interaktywnych.</li>
    </ul>
  </div>
  <div class="card-node card-green">
    <h4>Przełom In-Memory (AMPLab, 2009–2012)</h4>
    <ul>
      <li>Keny pamięci RAM spadły na tyle, by klastry mogły dysponować terabajtami pamięci operacyjnej.</li>
      <li>Zamiast pojedynczego kroku Map+Reduce, budujemy pełny acykliczny graf skierowany (<b>DAG</b>).</li>
      <li>Dysk i sieć są dotykane tylko wtedy, gdy jest to absolutnie konieczne.</li>
    </ul>
  </div>
</div>

<div class="source">Źródło: M. Zaharia et al., "Spark: Cluster Computing with Working Sets", USENIX HotCloud 2010.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Przypomnieć eksperyment Mateia Zaharii z UC Berkeley: regresja logistyczna na 100 GB danych w Hadoop MapReduce trwała ~80 sekund na jedną iterację, bo za każdym razem czytała te same dane z dysku. W Sparku pierwsza iteracja ładuje dane do RAM, a kolejne trwają poniżej 1 sekundy!
-->

---

## 02. Rewolucja RDD: Odporność na awarie bez replikacji w RAM

Jak zapewnić odporność na awarię węzła w pamięci RAM bez kosztownego kopiowania każdego bajtu na 3 serwery (jak w systemach DSM – *Distributed Shared Memory*)?

<div class="grid-2">
  <div class="card-node card-blue">
    <h4>RDD (Resilient Distributed Dataset)</h4>
    <span class="sub">Abstrakcja podstawowa Apache Spark (NSDI 2012)</span>
    <ul>
      <li><b>Resilient (Odporny):</b> Jeśli serwer zgaśnie i straci zawartość RAM, Spark automatycznie odtworzy brakujący fragment.</li>
      <li><b>Distributed (Rozproszony):</b> Dane są fizycznie podzielone na <b>partycje</b> rozsiane po wielu maszynach klastra.</li>
      <li><b>Immutable (Niemutowalny):</b> Raz utworzonego zbioru RDD nie można zmienić w miejscu – można go jedynie przekształcić w nowy RDD.</li>
    </ul>
  </div>

  <div class="card-node card-purple">
    <h4>Sekret: Graf Rodowodu (Lineage Graph)</h4>
    <span class="sub">Przeliczanie zamiast replikacji!</span>
    <ul>
      <li>Ponieważ zbiory są <b>niemutowalne</b>, a operacje deterministyczne, Spark nie musi replikować danych pośrednich w RAM.</li>
      <li>Spark pamięta "przepis kucharski" (graf transformacji), jak powstała każda partycja: <code>RDD_C = RDD_B.filter(...) = RDD_A.map(...)</code>.</li>
      <li>Gdy węzeł nr 4 ulegnie awarii, Spark odtwarza <b>tylko utraconą partycję nr 4</b>, aplikując przepis od najbliższego punktu źródłowego!</li>
    </ul>
  </div>
</div>

<div class="box-info">
<b>Geniusz inżynierski (Best Paper Award NSDI 2012):</b> Zastąpienie fizycznej replikacji pamięci RAM logicznym grafem rodowodu (<i>Lineage</i>) pozwoliło zaoszczędzić <b>3-krotność pamięci operacyjnej</b> i wyeliminowało ruch sieciowy podczas normalnej pracy bez awarii.
</div>

<div class="source">Źródło: M. Zaharia, M. Chowdhury, T. Das et al., "Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing", USENIX NSDI 2012.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Warto zapytać studentów: "Dlaczego niemutowalność (immutability) jest tak ważna w systemach rozproszonych?". Gdyby ktoś mógł modyfikować wiersze w miejscu z wielu wątków, odtworzenie stanu po awarii na podstawie historii komend byłoby niemożliwe z powodu wyścigów (race conditions).
-->

---

## 03. Fizyczna architektura klastra Apache Spark

Każda aplikacja Spark działa w architekturze **Master–Worker**:

```
┌────────────────────────────┐         ┌──────────────────────────────────────┐
│    DRIVER PROGRAM (JVM)    │         │           EXECUTOR NODE 1            │
│  ┌──────────────────────┐  │         │  ┌──────────┐  ┌──────────────────┐  │
│  │     SparkSession     │  │         │  │ Cache /  │  │ Slot 1: [Task A] │  │
│  │ (DAG Scheduler, AQE, │  │◄───────►│  │  Memory  │  │ Slot 2: [Task B] │  │
│  │  Catalyst Optimizer) │  │         │  └──────────┘  └──────────────────┘  │
│  └──────────────────────┘  │         └──────────────────────────────────────┘
└─────────────┬──────────────┘                            ▲
              │ Prośba o kontenery                        │ Przydział zasobów
              ▼                                           ▼
┌────────────────────────────┐         ┌──────────────────────────────────────┐
│      CLUSTER MANAGER       │         │           EXECUTOR NODE 2            │
│ (Kubernetes / YARN / Cloud)│────────►│  ┌──────────┐  ┌──────────────────┐  │
└────────────────────────────┘         │  │ Cache /  │  │ Slot 3: [Task C] │  │
                                       │  │  Memory  │  │ Slot 4: [Task D] │  │
                                       │  └──────────┘  └──────────────────┘  │
                                       └──────────────────────────────────────┘
```
---
## 03-1. Fizyczna architektura klastra Apache Spark

* **Driver Program:** Pojedynczy proces zarządzający cyklem życia aplikacji. Tłumaczy kod użytkownika (Python/SQL/Scala) na plan fizyczny, dzieli pracę na drobne zadania (*Tasks*) i rozsyła je do wykonania.
* **Cluster Manager:** Zarządca infrastruktury (Kubernetes, Hadoop YARN, Databricks Cloud Manager) – wie tylko, ile jest wolnych procesorów i RAM-u w serwerowni, nie rozumie logiki zapytania SQL.
* **Executors:** Procesy robocze JVM uruchamiane na węzłach klastra. Każdy posiada przydzieloną pamięć RAM oraz pewną liczbę rdzeni procesora (**Cores / Slots**) do równoległego wykonywania zadań.

<div class="source">Źródło: Apache Spark Official Documentation, "Cluster Mode Overview", 2024.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Podkreślić, że Driver to pojedynczy punkt awarii (Single Point of Failure) dla danego zadania! Jeśli Driver zginie, całe zadanie pada. Jeśli zginie Executor, Driver prosi Cluster Managera o nowego Executora i powtarza na nim tylko te Taski, które upadły.
-->

---

## 04. Hierarchia egzekucji kodu: Od Aplikacji do Pojedynczego Tasku

W jaki sposób Twoje zapytanie SQL lub skrypt PySpark zamienia się w fizyczne wątki na procesorach w klastrze?

<div class="flow-row">
  <div class="card-node card-blue">
    <h4>1. Application</h4>
    <span class="sub">Cały program / sesja</span>
    <ul>
      <li>Twój skrypt lub notatnik przypisany do jednej instancji <code>SparkSession</code>.</li>
      <li>Może zawierać dziesiątki zapytań i operacji.</li>
    </ul>
  </div>
  <div class="flow-arrow-h">➔</div>
  <div class="card-node card-purple">
    <h4>2. Job (Zadanie)</h4>
    <span class="sub">1 Action = 1 Job</span>
    <ul>
      <li>Wyzwalany przez każdą <b>Akcję</b> (np. <code>count()</code>, <code>save()</code>, <code>show()</code>).</li>
      <li>Posiada własny acykliczny graf wykonania (DAG).</li>
    </ul>
  </div>
  <div class="flow-arrow-h">➔</div>
  <div class="card-node card-orange">
    <h4>3. Stage (Etap)</h4>
    <span class="sub">Granica wymiany sieciowej</span>
    <ul>
      <li>Zestaw operacji, które można wykonać lokalnie na węźle bez udziału sieci.</li>
      <li>Każdy <b>Shuffle</b> tnie Job na osobne Stage'e!</li>
    </ul>
  </div>
  <div class="flow-arrow-h">➔</div>
  <div class="card-node card-green">
    <h4>4. Task (Jednostka)</h4>
    <span class="sub">1 Partycja = 1 Task</span>
    <ul>
      <li>Najmniejsza jednostka pracy wysyłana do Executora.</li>
      <li>Wykonywana przez <b>1 wątek CPU (1 Slot)</b> na dokładnie <b>1 partycji danych</b>.</li>
    </ul>
  </div>
</div>

<div class="box-dark">
<b>Złote równanie równoległości w Apache Spark:</b><br>
<code>1 Partycja Danych = 1 Task = 1 Wątek Procesora (Core/Slot)</code>.<br>
Jeśli masz klaster ze 100 rdzeniami CPU, ale Twoje dane są zapisane w 1 niepodzielnym pliku (1 partycja), to <b>99 rdzeni będzie bezczynnie stało</b>, a cały plik będzie mielony przez 1 rdzeń!
</div>

<div class="source">Źródło: J. S. Damji, B. Wenig, T. Das, D. Lee, "Learning Spark, 2nd Edition", O'Reilly 2020 (Rozdział 2).</div>

<!--
NOTATKI DLA WYKŁADOWCY:
To równanie to absolutny fundament. Wielu juniorów myśli: "mam wolny skrypt, więc dokupię w chmurze większy klaster ze 128 rdzeniami". Jeśli ich DataFrame ma 2 partycje, to 126 rdzeni będzie marnować pieniądze firmy, nie robiąc absolutnie nic.
-->

---

## 05. Ewolucja API w Sparku: Dlaczego deklaratywność wygrała?

Ewolucja Sparka od niskopoziomowych funkcji lambda ku wysokopoziomowym abstrakcjom relacyjnym:

| Cechy / Kryterium | RDD API (Spark 1.0, 2014) | DataFrame API / Spark SQL (Spark 1.3 / 2.0+) | Dataset API (Spark 1.6 / 2.0+, tylko Scala/Java) |
|---|---|---|---|
| **Poziom abstrakcji** | Niskopoziomowy, imperatywny (*"JAK to policzyć"*) | Wysokopoziomowy, deklaratywny (*"CO chcę otrzymać"*) | Pośredni (łączy zalety RDD i DataFrame) |
| **Znajomość schematu przez silnik** | **Brak (Czarna skrzynka)** – obiekty Javy/Pythona są nieprzezroczyste | **Pełna znajomość kolumn i typów** (jak w tabeli relacyjnej) | Pełna znajomość schematu dzięki *Encoders* |
| **Optymalizator zapytań (Catalyst)** | ❌ Brak (wykonuje kod dokładnie tak, jak napisał programista) | ✅ **Tak** (automatycznie przepisuje i optymalizuje plan) | ✅ **Tak** (częściowo ograniczony przy funkcjach lambda) |
| **Reprezentacja w pamięci** | Obiekty sterty JVM (duży narzut pamięciowy i GC) | **Binarny format Tungsten (`UnsafeRow`)** poza stertą | Konwersja pomiędzy `UnsafeRow` a obiektami JVM |
| **Wydajność między językami** | Scala szybsza niż Python (narzut serializacji w Pythonie) | **Identyczna wydajność w Pythonie, SQL, Scali i R!** | Niedostępne w Pythonie ani R |

<div class="box-info">
<b>Wniosek praktyczny:</b> W nowoczesnym Sparku (3.x / 4.x) używamy niemal wyłącznie <b>DataFrame API oraz Spark SQL</b>. Pisząc w PySparku za pomocą DataFrame API, Twój kod nie wykonuje się w powolnym interpreterze Pythona – Python służy jedynie do zbudowania planu zapytania, który wykonuje natywny silnik JVM/C++!
</div>

<div class="source">Źródło: M. Armbrust, R. S. Xin, M. Zaharia et al., "Spark SQL: Relational Data Processing in Spark", ACM SIGMOD 2015.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Wyjaśnić różnicę między kodem imperatywnym a deklaratywnym. W RDD programista mówi: "weź wiersz, wyciągnij pole 3, przemnóż, potem połącz z inną parą". Jeśli napiszesz najpierw JOIN, a potem FILTER w RDD, Spark wykona gigantyczny JOIN przed filtrowaniem. W DataFrame API mówisz "chcę złączenie i filtr", a Catalyst sam przesunie filtr PRZED złączenie!
-->

---

## 06. Leniwa ewaluacja (Lazy Evaluation): Transformacje vs Akcje

Operacje w Apache Spark dzielą się rygorystycznie na dwie kategorie:

<div class="grid-2">
  <div class="card-node card-blue">
    <h4>Transformacje (Transformations)</h4>
    <span class="sub">Operacje LENIWE (Lazy) – zwracają nowy DataFrame</span>
    <ul>
      <li>Przykłady: <code>select()</code>, <code>filter()</code>, <code>where()</code>, <code>groupBy()</code>, <code>join()</code>, <code>orderBy()</code>, <code>withColumn()</code>.</li>
      <li>Wywołanie transformacji <b>NIE przetwarza żadnych danych</b> i trwa ułamek milisekundy!</li>
      <li>Dopisuje jedynie kolejny krok do logicznego planu wykonania (grafu DAG).</li>
    </ul>
  </div>

  <div class="card-node card-orange">
    <h4>Akcje (Actions)</h4>
    <span class="sub">Operacje ZACHŁANNE (Eager) – wymuszają wynik</span>
    <ul>
      <li>Przykłady: <code>count()</code>, <code>show()</code>, <code>collect()</code>, <code>take(n)</code>, <code>write.parquet()</code>, <code>write.saveAsTable()</code>.</li>
      <li>Wywołanie akcji mówi silnikowi: <i>"Potrzebuję teraz fizycznego wyniku na ekranie lub na dysku!"</i>.</li>
      <li>Uruchamia kompilator Catalyst, optymalizuje cały łańcuch i wysyła zadania na klaster.</li>
    </ul>
  </div>
</div>

```python
# Krok 1–3: Transformacje (czas wykonania: 0.002 sekundy – klaster nic jeszcze nie czyta!)
df_logs   = spark.read.parquet("s3://shop/logs_10TB/")
df_errors = df_logs.filter(df_logs.status == 500)
df_final  = df_errors.select("user_id", "timestamp")

# Krok 4: Akcja (Dopiero TERAZ rusza maszyna! Spark widzi cały plan i czyta z S3 tylko 2 kolumny i bloki ze statusem 500)
df_final.write.format("delta").save("s3://shop/errors_report/")
```

<div class="source">Źródło: Apache Spark Programming Guide, "RDD & DataFrame Operations: Transformations and Actions", 2024.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Częsty błąd początkujących w notatnikach Jupyter/Databricks: po każdej linijce transformacji dopisują `.show()` albo `print(df.count())`, żeby "sprawdzić czy działa". W ten sposób zmuszają klaster do wielokrotnego czytania i przeliczania terabajtów danych od samego początku!
-->

---

## 07. Narrow vs Wide Transformations: Gdzie rodzi się koszt sieciowy?

To najważniejszy podział architektoniczny w całym silniku Spark. Decyduje o tym, czy zadanie wykona się w sekundy, czy zawiesi sieć w serwerowni:

```
NARROW TRANSFORMATION (Wąska: filter, select, map, union)        WIDE TRANSFORMATION (Szeroka: groupBy, join, orderBy, distinct)
Partycja wejściowa ma dokładnie 1 partycję wyjściową!            Dane z wszystkich partycji muszą zostać wymieszane przez sieć!

  [Węzeł 1: Part A] ──────────► [Węzeł 1: Part A']                 [Węzeł 1: Part A] ───┬──╲ ╱──┬───► [Węzeł 1: Part X]
  [Węzeł 2: Part B] ──────────► [Węzeł 2: Part B']                 [Węzeł 2: Part B] ───┼───╳───┼───► [Węzeł 2: Part Y]
  [Węzeł 3: Part C] ──────────► [Węzeł 3: Part C']                 [Węzeł 3: Part C] ───┴──╱ ╲──┴───► [Węzeł 3: Part Z]
          ▲                                                                                      ▲
  Brak ruchu sieciowego! Praca w 100% w RAM (1 Stage).                  Bariera SHUFFLE! Cięcie na nowy Stage, dysk + sieć TCP/IP.
```

<div class="grid-2">
  <div class="card-node card-green">
    <h4>Transformacje Wąskie (Narrow)</h4>
    <ul>
      <li>Każdy wiersz można przetworzyć niezależnie, bez wiedzy o wierszach leżących na innych serwerach.</li>
      <li>Spark łączy wiele wąskich operacji w jeden potok (<i>Pipelining</i>) wykonywany w pojedynczym przebiegu w pamięci RAM.</li>
    </ul>
  </div>
  <div class="card-node card-red">
    <h4>Transformacje Szerokie (Wide)</h4>
    <ul>
      <li>Wymagają zgrupowania rekordów o tym samym kluczu z całego klastra na jednym węźle (np. <code>GROUP BY user_id</code>).</li>
      <li>Wymuszają kosztowną operację <b>Shuffle</b> (zapis na dysk + wysyłka przez sieć) i tworzą granicę nowego etapu (<b>Stage Boundary</b>).</li>
    </ul>
  </div>
</div>

<div class="source">Źródło: M. Zaharia et al., "Resilient Distributed Datasets", USENIX NSDI 2012.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Wytłumaczyć na żywym przykładzie: jeśli na sali jest 100 studentów w 5 rzędach i proszę każdego: "pomnóż swój wiek przez 2" – każdy robi to w głowie bez wstawania z ławki (Narrow Transformation). Jeśli jednak powiem: "ustawcie się w grupy według miesiąca urodzenia i policzcie średni wzrost" – wszyscy muszą wstać, zacząć krzyczeć i przemieszczać się po sali (Wide Transformation / Shuffle!).
-->

---

## 08. Studium przypadku: Nocny potok "TelecomPol" staje na 99%

<div class="box-warning">
<b>Scenariusz produkcyjny (Awaria o 03:30 w nocy):</b><br>
Operator telekomunikacyjny "TelecomPol" przetwarza co noc <b>2 miliardy rekordów bilingowych CDR</b> (<i>Call Detail Records</i> – ok. 800 GB danych Parquet). Zadanie wykonuje złączenie z tabelą klientów i agregację czasu połączeń po numerze telefonu (<code>GROUP BY msisdn</code>).
</div>

* **Objawy w panelu monitoringu (Spark UI):**
  * Etap (Stage) składa się z **200 zadań (Tasks)**.
  * **199 z 200 zadań** wykonuje się błyskawicznie w ciągu **90 sekund**.
  * Pasek postępu zatrzymuje się na magicznym **99.5% (199/200 Tasks Succeeded)**.
  * Ostatni, 200. task mieli dane samotnie przez **3,5 godziny**, wentylatory jednego serwera pracują na 100%, aż w końcu całe zadanie kończy się błędem:
    `org.apache.spark.SparkException: Job aborted due to stage failure: java.lang.OutOfMemoryError: Java heap space`

<div class="box-info">
<b>Pytanie diagnostyczne:</b> Dlaczego 199 rdzeni skończyło pracę w półtorej minuty, a jeden rdzeń pracował ponad 3 godziny i zabił cały klaster? Aby odpowiedzieć, musimy rozebrać na części pierwsze trzy demony Sparka: <b>Shuffle</b>, <b>Pamięć Executora</b> oraz <b>Data Skew</b>.
</div>

<!--
NOTATKI DLA WYKŁADOWCY:
To jest punkt zwrotny wykładu (ok. 30-35 minuty). Każdy inżynier Big Data na świecie widział pasek postępu w Sparku zatrzymany na "199/200 tasks". To klasyczny objaw patologii rozproszonego przetwarzania.
-->

---

## 09. Anatomia operacji Shuffle: Najdroższy proces w klastrze

Co naprawdę dzieje się pod maską, gdy piszesz niewinnie wyglądające `df.groupBy("klucz").sum("kwota")` lub `df1.join(df2, "id")`?

```
Faza 1: MAP / SHUFFLE WRITE (Na węźle wysyłającym)            Faza 2: REDUCE / SHUFFLE READ (Na węźle odbierającym)
┌────────────────────────────────────────────────┐            ┌────────────────────────────────────────────────┐
│ 1. Obliczenie funkcji skrótu: hash(klucz) % N  │            │ 5. Pobranie swoich bloków przez sieć (Netty)   │
│ 2. Buforowanie w pamięci Execution Memory      │  Transfer  │ 6. Scalenie (Merge Sort) strumieni z N węzłów  │
│ 3. Gdy brakuje RAM: zrzut na dysk (Disk Spill) │ ─────────► │ 7. Gdy brakuje RAM: ponowny Disk Spill!        │
│ 4. Zapis pliku .data oraz indeksu .index       │   TCP/IP   │ 8. Wykonanie właściwej agregacji / złączenia   │
└────────────────────────────────────────────────┘            └────────────────────────────────────────────────┘
```

* **Dlaczego Shuffle jest tak kosztowny? Kumuluje 4 najcięższe operacje w informatyce:**
  1. **CPU:** Serializacja obiektów do strumienia bajtów, liczenie funkcji skrótu (*MurmurHash3*), sortowanie i kompresja.
  2. **Disk I/O:** Spark **zawsze** zapisuje pliki Shuffle na lokalny dysk węzła (*Shuffle Write*), aby w razie awarii odbiorcy nie przeliczać poprzedniego etapu od nowa!
  3. **Network I/O:** Wysłanie gigabajtów/terabajtów danych pomiędzy wszystkimi węzłami klastra (*All-to-All communication*).
  4. **Memory Pressure:** Konieczność utrzymania tablic mieszających lub buforów sortujących w pamięci RAM odbiorcy.

<div class="source">Źródło: H. Karau, R. Warren, "High Performance Spark", O'Reilly 2017 (Rozdział 4: Joins and Shuffles).</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Zaskoczyć studentów faktem: nawet w silniku "In-Memory" takim jak Spark, operacja Shuffle ZAWSZE zapisuje dane na fizyczny dysk lokalny węzła roboczego (w katalogu `spark.local.dir`). Dlatego w chmurze maszyny pod Sparka muszą mieć szybkie lokalne dyski NVMe SSD!
-->

---

## 10. Zarządzanie pamięcią wewnątrz Executora (Unified Memory Manager)

Pamięć RAM przydzielona pojedynczemu Executorowi (np. `spark.executor.memory = 10 GB`) nie jest jednym wielkim workiem. Od wersji Spark 1.6 zarządza nią **Unified Memory Manager**:

```
Całkowita pamięć JVM Executora (np. 10 GB):
┌───────────────┬────────────────────────────┬────────────────────────────────────────────────────────────────┐
│   Reserved    │        User Memory         │                  SPARK MEMORY (~60% całości)                   │
│    Memory     │        (~40% reszty)       │  ┌───────────────────────────┐  ┌───────────────────────────┐  │
│   (300 MB)    │                            │  │      Storage Memory       │  │     Execution Memory      │  │
│               │ Własne struktury danych,   │  │     (Domyślnie 50%)       │◄─┤     (Domyślnie 50%)       │  │
│ System Sparka │ obiekty UDF, metadane RDD  │  │ Cache / Persist, Broadcast│─►│ Shuffle, Join, Sort, Agg  │  │
└───────────────┴────────────────────────────┴──┴───────────────────────────┴──┴───────────────────────────┴──┘
                                                                             ▲
                                                Elastyczna granica (Dynamic Occupancy Mechanism)!
```

* **Elastyczna granica pomiędzy *Storage* a *Execution*:**
  * Jeśli nie używasz `df.cache()`, **Execution Memory** może zająć 100% obszaru *Spark Memory* na potrzeby wielkich złączeń i sortowania.
  * Jeśli używasz `cache()`, ale nagle rusza ciężki `JOIN`, *Execution Memory* **wyrzuci (evict)** zbuforowane bloki *Storage* na dysk, by ratować wykonanie zapytania!
  * **Kiedy dochodzi do Disk Spill?** Gdy sam obszar *Execution Memory* wypełni się po brzegi – Spark zrzuca nadmiar tabel sortowania na dysk SSD, co chroni przed OOM, ale **spowalnia zadanie 10-krotnie**.

<div class="source">Źródło: Apache Spark Memory Management Specification (SPARK-10000: Unified Memory Management in Spark 1.6+).</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Wyjaśnić studentom, że przed Sparkiem 1.6 granica między Storage a Execution była sztywnym murem (Static Memory Manager). Prowadziło to do absurdu: zapytanie wybuchało błędem OOM przy złączeniu, podczas gdy obok leżało 30 GB zupełnie pustej pamięci zarezerwowanej na Cache!
-->

---

## 11. Dwa oblicza katastrofy `OutOfMemoryError` (OOM)

Kiedy widzisz w logach błąd `OutOfMemoryError`, pierwsze pytanie inżyniera brzmi: **Kto wybuchł – Driver czy Executor?** Przyczyny i lekarstwa są zupełnie inne!

<div class="grid-2">
  <div class="card-node card-red">
    <h4>1. Driver OOM (Wybucha mózg klastra)</h4>
    <span class="sub">Przepełnienie pamięci na węźle zarządzającym</span>
    <ul>
      <li><b>Grzech główny #1:</b> Wywołanie <code>df.collect()</code> lub <code>df.toPandas()</code> na dużym zbiorze – próba ściągnięcia 500 GB danych z klastra do pamięci jednego procesu Drivera!</li>
      <li><b>Grzech główny #2:</b> Zbyt duży <code>BroadcastJoin</code> – Driver najpierw sam pobiera całą rozgłaszaną tabelę do swojej pamięci, aby zbudować z niej Hash Mapę.</li>
      <li><b>Lekarstwo:</b> Zakaz używania <code>collect()</code> na danych produkcyjnych (używaj <code>take(20)</code> lub zapisu do S3), zmniejszenie progu <code>autoBroadcastJoinThreshold</code>.</li>
    </ul>
  </div>

  <div class="card-node card-orange">
    <h4>2. Executor OOM (Wybucha węzeł roboczy)</h4>
    <span class="sub">Przepełnienie pamięci podczas przetwarzania partycji</span>
    <ul>
      <li><b>Przyczyna #1 (Data Skew):</b> Jedna gigantyczna partycja (np. 40 GB dla klucza <code>NULL</code>) trafia do jednego Tasku i nie mieści się w <i>Execution Memory</i>.</li>
      <li><b>Przyczyna #2 (Zbyt mało partycji):</b> Czytanie lub tasowanie 1 TB danych podzielonego tylko na 10 partycji (100 GB na jeden wątek!).</li>
      <li><b>Przyczyna #3 (Za dużo rdzeni na Executor):</b> 16 wątków w jednym Executorze walczy o wspólną pulę RAM w tym samym ułamku sekundy.</li>
    </ul>
  </div>
</div>

<div class="box-warning">
<b>Pułapka kontenerowa (Kubernetes / YARN):</b> Czasami nie zobaczysz błędu Java OOM, lecz kod wyjścia <code>Exit Code 137 (OOMKilled)</code>. Oznacza to, że proces Pythona lub pamięć <i>Off-Heap</i> przekroczyła limit kontenera Linuxa (<code>spark.executor.memoryOverhead</code>) i jądro systemu bezlitośnie zabiło proces sygnałem <code>SIGKILL</code>!
</div>

<div class="source">Źródło: Databricks Documentation, "Debugging OutOfMemoryError Exceptions in Apache Spark", 2023.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Kod 137 (128 + 9, gdzie 9 to SIGKILL) to klasyczne pytanie rekrutacyjne na stanowisko Senior Data Engineer. Jeśli w PySparku proces Pythona rozrośnie się w RAM, maszyna JVM tego nie widzi, ale widzi to Linux OOM Killer i zabija cały kontener z kodem 137.
-->

---

## 12. Zjawisko skośności danych (Data Skew) i problem maruderów

Wracamy do awarii w "TelecomPol" (200 tasków). To podręcznikowy przykład **Data Skew**:

```
Rozkład danych po operacji hash(msisdn) % 200:
Task 001 (Executor 1): ██ 150 MB                      ──► Czas: 45 s  (OK)
Task 002 (Executor 2): ██ 145 MB                      ──► Czas: 42 s  (OK)
...
Task 199 (Executor 8): ██ 155 MB                      ──► Czas: 47 s  (OK)
Task 200 (Executor 3): ██████████████████████████████ ──► 120 GB! (Disk Spill + GC Pause + OOM!)
                       ▲
       Wszystkie połączenia nieznane / techniczne mają wartość msisdn = "UNKNOWN" lub NULL!
```

* **Dlaczego funkcja skrótu zawiodła?**
  * Funkcja `hash(klucz)` jest deterministyczna: ten sam klucz --> **zawsze ten sam numer partycji**!
  * W realnym świecie dane nigdy nie mają rozkładu jednostajnego – rządzi nimi **rozkład potęgowy (Prawo Zipfa / Zasada Pareto)**:
    * E-commerce: 1 konto hurtownika lub bota generuje 20% wszystkich zamówień.
    * Telekomunikacja / IoT: uszkodzone czujniki lub połączenia nieautoryzowane wysyłają milion rekordów z kluczem `NULL` lub `-1`.
  * W efekcie **czas wykonania całego etapu (Stage) jest równy czasowi wykonania najwolniejszego zadania (*Straggler Task*)**.

<div class="source">Źródło: Y. Kwon et al., "SkewTune: Mitigating Skew in MapReduce Applications", ACM SIGMOD 2012.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
W "TelecomPol" okazało się, że nieodebrane połączenia z sieci zagranicznych miały pusty numer kierunkowy `NULL`. 300 milionów rekordów `NULL` otrzymało dokładnie ten sam `hash(null)` i powędrowało przez sieć do jednego biednego rdzenia CPU na trzecim serwerze!
-->

---

## 13. Pułapka klasycznych funkcji UDF w PySparku vs Apache Arrow

Programiści Pythona uwielbiają pisać własne funkcje i rejestrować je jako `@udf`. W klasycznym PySparku to **wydajnościowe samobójstwo**:

```
Klasyczny Python UDF (Wiersz po wierszu – katastrofa wydajności):
[Executor JVM: UnsafeRow] ──(1. Deserializacja i wysyłka przez socket TCP/Pipe: Pickle)──► [Proces Python Worker]
[Executor JVM: UnsafeRow] ◄──(3. Odbiór i ponowna serializacja do formatu JVM: Pickle)──── [2. Pętla for w Pythonie]

Wektoryzowany Pandas UDF (Spark 2.3+ / 3.x z wykorzystaniem Apache Arrow):
[Executor JVM: Kolumny]   ════(Bezpośredni transfer bloków kolumnowych Arrow / Zero-Copy)═══► [Wektory C/NumPy/Pandas]
```

<div class="grid-2">
  <div class="card-node card-red">
    <h4>Dlaczego klasyczny Python UDF zabija klaster?</h4>
    <ul>
      <li><b>Czarna skrzynka dla Catalysta:</b> Optymalizator nie wie, co robi kod Pythona – wyłącza wypychanie predykatów i generowanie kodu.</li>
      <li><b>Koszt podwójnej serializacji:</b> Każdy pojedynczy wiersz musi opuścić maszynę JVM, zostać spakowany protokołem <code>Pickle</code>, przesłany lokalnym gniazdem do procesu Pythona i wrócić z powrotem!</li>
      <li>Spowolnienie rzędu <b>10x – 100x</b> względem wbudowanych funkcji Spark SQL.</li>
    </ul>
  </div>

  <div class="card-node card-green">
    <h4>Hierarchia dobrych praktyk w PySparku</h4>
    <ul>
      <li><b>Wybór nr 1 (Zawsze pierwszy!):</b> Wbudowane funkcje <code>pyspark.sql.functions</code> (np. <code>F.when</code>, <code>F.regexp_extract</code>) – działają w 100% wewnątrz JVM/C++.</li>
      <li><b>Wybór nr 2 (Gdy brakuje wbudowanej):</b> <b>Pandas UDF (Vectorized UDF)</b> – wykorzystuje poznany na Wykładzie 3 format <b>Apache Arrow</b> do przesyłania całych paczek kolumn (np. po 10 000 wierszy) i liczy je wektorowo w C.</li>
    </ul>
  </div>
</div>

<div class="source">Źródło: B. Cutler, L. Jin (IBM / Databricks), "Introducing Pandas UDF for PySpark Powered by Apache Arrow", 2017.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Nawiązać bezpośrednio do Wykładu 3 (Slajd 12 o Apache Arrow). Tutaj studenci widzą na własne oczy, jak teoria z poprzedniego tygodnia (format in-memory Arrow) ratuje produkcyjny kod w PySparku, przyspieszając funkcje użytkownika z 2 godzin do 3 minut!
-->

---

## 14. Rentgen inżyniera danych: Jak czytać Spark UI w 60 sekund?

Gdy zadanie działa wolno, profesjonalny inżynier nie zgaduje – otwiera zakładkę **Stages** w **Spark UI** i patrzy na tabelę *Summary Metrics for Completed Tasks*:

| Metryka w Spark UI | Co oznacza zdrowy klaster? | Alarm produkcyjny (Co oznacza patologia?) | Diagnoza i działanie naprawcze |
|---|---|---|---|
| **Duration (Min / Median / Max)** | Min: `18s` \| Median: `21s` \| Max: `25s` (wartości zbliżone) | Min: `4s` \| Median: `12s` \| **Max: `2.4 h`** | **Data Skew!** Jeden task dostał gigantyczną ilość danych. Włącz AQE Skew Join lub zastosuj *Salting*. |
| **Shuffle Spill (Memory / Disk)** | `0 B / 0 B` (wszystko zmieściło się w RAM) | Memory: `85 GB` \| **Disk: `18 GB`** | **Za mało Execution Memory na task!** Zwiększ liczbę partycji (`spark.sql.shuffle.partitions`) lub RAM. |
| **GC Time (Garbage Collection)** | `< 2–3%` całkowitego czasu trwania tasku | **> 15–20%** czasu trwania tasku (pasek świeci na czerwono) | Tworzysz miliony obiektów na stercie JVM (np. w RDD lub UDF). Przejdź na natywne DataFrame API (Tungsten). |
| **Scheduler Delay / Task Deserialization** | `< 50 ms` | Kilka sekund na każdy krótki task | **Problem małych plików** lub przeładowany Driver wysyłający tysiące mikro-tasków. Wykonaj `OPTIMIZE`. |

<div class="box-info">
<b>Wskazówka diagnostyczna:</b> Zawsze porównuj kolumnę <b>Median (50th percentile)</b> z kolumną <b>Max</b>. Jeśli mediana wynosi 10 sekund i 100 MB, a wartość maksymalna wynosi 45 minut i 30 GB – masz 100% dowód na skośność klucza (<i>Data Skew</i>).
</div>

<div class="source">Źródło: Apache Spark Web UI Instrumentation & Monitoring Guide, 2024.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Pokazać studentom logikę percentyli: średnia arytmetyczna kłamie! Jeśli 199 tasków trwa 10 sekund, a jeden trwa 2000 sekund, średnia wynosi ~20 sekund i wygląda niewinnie. Ale porównanie Mediany (10 s) z Max (2000 s) natychmiast demaskuje marudera (Straggler).
-->

---

## 15. Matematyka katastrofy złączeń: `SortMergeJoin` vs `BroadcastHashJoin`

Złączenie (`JOIN`) dwóch tabel to najczęstsza operacja w hurtowniach i Lakehouse'ach. Spark posiada dwa zupełnie różne sposoby jej wykonania:

```
1. SortMergeJoin (Domyślny dla dwóch dużych tabel):              2. BroadcastHashJoin (Gdy jedna tabela jest mała, np. < 10-100 MB):
Obie tabele są tasowane przez sieć (Shuffle) i sortowane!        Mała tabela wysyłana w całości do każdego węzła. ZERO SHUFFLE!

[Tabela Faktów 1 TB] ──(Shuffle 1 TB!)──► [Sort] ──┐             [Tabela Faktów 1 TB] ────────────────────────► [Hash Lookup w RAM]
                                                   ├─► WYNIK                                                           ▲
[Tabela Wymiaru 5MB] ──(Shuffle 5 MB)───► [Sort] ──┘             [Tabela Wymiaru 5MB] ──► [Driver] ──(Broadcast)───────┘
```

<div class="grid-2">
  <div class="card-node card-orange">
    <h4>SortMergeJoin (SMJ)</h4>
    <ul>
      <li>Wymaga przetasowania (<b>Shuffle</b>) oraz fizycznego <b>posortowania</b> obu łączonych zbiorów po kluczu złączenia.</li>
      <li>Niezwykle odporny i skalowalny (radzi sobie nawet gdy tabele mają po 100 TB, bo może zrzucać bloki na dysk).</li>
      <li><span class="accent">Bardzo kosztowny sieciowo i procesorowo</span>, jeśli jedna z tabel jest małym słownikiem!</li>
    </ul>
  </div>

  <div class="card-node card-green">
    <h4>BroadcastHashJoin (BHJ)</h4>
    <ul>
      <li>Driver pobiera małą tabelę wymiaru (np. słownik krajów lub sklepów) i rozsyła jej kopię do pamięci RAM każdego Executora.</li>
      <li>Wielka tabela faktów (1 TB) <b>nie rusza się z miejsca!</b> Złączenie staje się superszybką transformacją wąską (<i>Narrow</i>).</li>
      <li>Kontrolowane parametrem <code>spark.sql.autoBroadcastJoinThreshold</code> (domyślnie 10 MB, często podnoszone do 50–100 MB).</li>
    </ul>
  </div>
</div>

<div class="source">Źródło: M. Armbrust et al., "Spark SQL: Relational Data Processing in Spark", ACM SIGMOD 2015.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Podkreślić skalę zysku: jeśli łączysz tabelę sprzedaży (1 TB) ze słownikiem walut (2 MB), SortMergeJoin wyśle 1 TB danych przez karty sieciowe i posortuje miliardy wierszy. BroadcastHashJoin wyśle tylko 2 MB do węzłów i wykona złączenie w 3 sekundy bez dotykania sieci dla tabeli 1 TB!
-->

---

## 16. Mózg Sparka – Catalyst Optimizer: Cztery fazy kompilacji

Przechodzimy do Modułu 3: Jak Spark potrafi automatycznie zoptymalizować nasze zapytanie? Odpowiada za to regułowo-kosztowy kompilator **Catalyst Optimizer**:

<div class="flow-row">
  <div class="card-node card-blue">
    <h4>1. Analysis</h4>
    <span class="sub">Unresolved ➔ Resolved Plan</span>
    <ul>
      <li>Sprawdza w <b>Catalogu</b> (np. Delta/Iceberg/Unity), czy tabele i kolumny w ogóle istnieją.</li>
      <li>Rozwiązuje typy danych i nazwy atrybutów.</li>
    </ul>
  </div>
  <div class="flow-arrow-h">➔</div>
  <div class="card-node card-purple">
    <h4>2. Logical Optimization</h4>
    <span class="sub">Regułowa przebudowa drzewa</span>
    <ul>
      <li>Stosuje ponad 80 reguł algebraicznych (<i>Rule-Based Optimization</i>).</li>
      <li>Wypycha filtry w dół, usuwa zbędne kolumny, upraszcza stałe.</li>
    </ul>
  </div>
  <div class="flow-arrow-h">➔</div>
  <div class="card-node card-orange">
    <h4>3. Physical Planning</h4>
    <span class="sub">Co ➔ JAK to policzyć</span>
    <ul>
      <li>Generuje możliwe plany fizyczne.</li>
      <li>Model kosztowy (<b>CBO</b>) wybiera np. <code>BroadcastHashJoin</code> zamiast <code>SortMergeJoin</code>.</li>
    </ul>
  </div>
  <div class="flow-arrow-h">➔</div>
  <div class="card-node card-green">
    <h4>4. Code Generation</h4>
    <span class="sub">Kompilacja Janino (Tungsten)</span>
    <ul>
      <li>Tłumaczy wybrany plan fizyczny na czysty bajtkod maszyny JVM w locie.</li>
      <li>Gotowy kod trafia na Executory.</li>
    </ul>
  </div>
</div>

<div class="box-dark">
<b>Jak podejrzeć pracę Catalysta w kodzie?</b><br>
Wystarczy wywołać <code>df.explain(extended=True)</code> lub <code>df.explain("formatted")</code>! Spark wypisze na ekran wszystkie cztery drzewa transformacji: od tego, co napisałeś, aż po fizyczny plan, który zostanie uruchomiony na klastrze.
</div>

<div class="source">Źródło: M. Armbrust, R. S. Xin et al., "Spark SQL: Relational Data Processing in Spark", ACM SIGMOD 2015.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Zachęcić studentów do używania `df.explain("formatted")` podczas laboratoriów. Jeśli w planie fizycznym widzą `Exchange hashpartitioning`, oznacza to dokładnie operację Shuffle! Jeśli widzą `BroadcastExchange`, oznacza to BroadcastHashJoin.
-->

---

## 17. Optymalizacje logiczne Catalysta w praktyce

Zobaczmy, jak **Catalyst Optimizer** ratuje nieoptymalnie napisane zapytanie analityka, przebudowując drzewo algebraiczne przed dotknięciem dysku:

```
Zapytanie napisane przez użytkownika (Najpierw wielki JOIN, na końcu filtr i wybór 2 kolumn):
SELECT u.kraj, t.kwota FROM transakcje t JOIN uzytkownicy u ON t.user_id = u.id WHERE t.rok = 2025 AND u.aktywny = true

Plan Naiwny (Gdyby wykonać dosłownie – katastrofa!):          Plan Zoptymalizowany przez Catalyst (Wykonywany w rzeczywistości):
        [Projekcja: kraj, kwota]                                                [Projekcja: kraj, kwota]
                   ▲                                                                       ▲
     [Filtr: rok=2025 AND aktywny=true]                                           [JOIN po user_id = id]
                   ▲                                                                 ▲                ▲
   [JOIN całych tabel ze wszystkimi kolumnami!]                         [Projekcja: id, kwota]  [Projekcja: id, kraj]
         ▲                          ▲                                            ▲                ▲
[Skan: 50 kolumn, 10 lat]  [Skan: 30 kolumn, wszyscy]                   [Filtr: rok=2025]      [Filtr: aktywny=true]
                                                                                 ▲                ▲
                                                                        [Skan Parquet/Delta]   [Skan Parquet/Delta]
```

---
## 17-01. Optymalizacje logiczne Catalysta w praktyce


* **Trzy kluczowe reguły optymalizacji logicznej:**
  1. **Predicate Pushdown (Wypychanie predykatów):** Filtry `WHERE` są spychane na sam dół drzewa – aż do stopki pliku **Apache Parquet** (Wykład 3!), dzięki czemu niepasujące bloki nie są w ogóle czytane z S3.
  2. **Column Pruning (Przycinanie kolumn):** Catalyst wykrywa, że z 80 kolumn obu tabel potrzebne są tylko 4 (`user_id`, `id`, `kraj`, `kwota`, `rok`, `aktywny`) i wyrzuca resztę na etapie odczytu.
  3. **Constant Folding & Boolean Simplification:** Wyrażenia takie jak `WHERE kwota > 100 * 12 AND true` są wyliczane raz podczas kompilacji planu (`kwota > 1200`), a nie miliard razy dla każdego wiersza!

<div class="source">Źródło: Apache Spark Catalyst Optimizer Rules (`org.apache.spark.sql.catalyst.optimizer`).</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Pokazać studentom piękną synergię pomiędzy Wykładem 3 a Wykładem 4: Catalyst Optimizer w Sparku (W04) przygotowuje wypychane filtry i listę kolumn, a format pliku Apache Parquet / Delta Lake (W03) realizuje je fizycznie, omijając całe grupy wierszy i kolumn na S3!
-->

---

## 18. Silnik fizyczny – Project Tungsten: Ucieczka ze sterty JVM

W 2015 roku twórcy Sparka zauważyli, że przy szybkich dyskach SSD i sieciach 10/40 GbE wąskim gardłem przestało być I/O, a stał się **narzut samej maszyny wirtualnej Javy (JVM)**:

<div class="grid-2">
  <div class="card-node card-red">
    <h4>Koszmar obiektów na stercie JVM (Heap)</h4>
    <ul>
      <li>Zwykły napis 4-znakowy <code>"ABCD"</code> w UTF-8 to <b>4 bajty</b> danych.</li>
      <li>Jako obiekt <code>java.lang.String</code> na stercie JVM zajmuje aż <b>48 bajtów</b> (nagłówek obiektu 64-bit, wskaźniki, tablica znaków, wyrównanie pamięci)!</li>
      <li>Miliony małych obiektów powodują gigantyczne pauzy odśmiecacza pamięci (<b>JVM Garbage Collector Stop-the-World</b>).</li>
    </ul>
  </div>

  <div class="card-node card-green">
    <h4>Rozwiązanie Tungsten: Format <code>UnsafeRow</code></h4>
    <ul>
      <li><b>Zarządzanie pamięcią Off-Heap (C-style):</b> Spark sam alokuje ciągłe bloki pamięci binarnej za pomocą <code>sun.misc.Unsafe</code>, poza kontrolą Garbage Collectora.</li>
      <li><b>Zwarta struktura bitowa:</b> Wiersz <code>UnsafeRow</code> składa się z maski bitowej dla wartości <code>NULL</code>, bloków stałej szerokości (8 bajtów na pole) oraz sekcji zmiennej długości.</li>
      <li>Sortowanie i porównywanie wierszy odbywa się na surowych bajtach za pomocą wskaźników pamięciowych!</li>
    </ul>
  </div>
</div>

```
Układ binarny pojedynczego wiersza w Project Tungsten (UnsafeRow – zwarty ciąg bajtów bez obiektów Javy):
┌──────────────────────┬───────────────────────────┬───────────────────────────┬──────────────────────────────────┐
│ Null Bitset Bitmap   │ Pole 1: ID (8B int64)     │ Pole 2: Offset+Len (8B)   │ Sekcja Variable-Length: "Olsztyn"│
│ (1 bit na kolumnę)   │ Wartość: 42               │ Wskaźnik do napisu ──────►│ (Ciągłe bajty UTF-8 w pamięci)   │
└──────────────────────┴───────────────────────────┴───────────────────────────┴──────────────────────────────────┘
```

<div class="source">Źródło: R. Xin, J. Rosen (Databricks), "Project Tungsten: Bringing Apache Spark Closer to Bare Metal", 2015.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Zwrócić uwagę na ten kontrast: 4 bajty użytecznych danych kontra 48 bajtów w obiekcie Javy (12-krotny narzut!). Dzięki formatowi UnsafeRow w Project Tungsten Spark mieści w tej samej pamięci RAM 4-5 razy więcej danych i całkowicie unika zacinania się na Garbage Collectorze.
-->

---

## 19. Whole-Stage Code Generation: Kompilacja do jednej pętli CPU

Tradycyjne silniki bazodanowe wykonywały zapytania w tzw. modelu **Volcano Iterator Model** (każdy operator wywołuje wirtualną metodę `.next()` operatora poniżej dla każdego wiersza). **Tungsten Faza 2** (Spark 2.0) zastąpił to kompilacją w locie:

```
Tradycyjny Model Volcano (Miliony wywołań wirtualnych):        Whole-Stage Code Generation (Jedna płaska pętla w bajtkodzie):
  [Project.next()] ◄── [Filter.next()] ◄── [Scan.next()]         for (row in parquetBatch) {
          │                   │                  │                 if (row.rok == 2025) {        // Bez wywołań funkcji!
   Skoki po pamięci, chybienia Cache L1 i brak optymalizacji       sum += row.kwota * 1.23;      // Dane prosto z rejestrów CPU!
   przez kompilator JIT dla miliarda wierszy!                    }
                                                                 }
```
---
## 19-01. Whole-Stage Code Generation: Kompilacja do jednej pętli CPU

* **Dlaczego *Whole-Stage Code Generation* przyspieszyło Sparka nawet 10-krotnie?**
  * Zamiast przekazywać wiersze przez łańcuch obiektów (`Scan ➔ Filter ➔ Project ➔ Aggregate`), Spark używa mikro-kompilatora **Janino**, aby wygenerować **jedną szytą na miarę pętlę `for` w czystej Javie** dla całego etapu (Stage).
  * **Zero wywołań funkcji wirtualnych** (*Virtual Function Dispatch*).
  * Zmienne pośrednie trzymane są bezpośrednio w ultraszybkich **rejestrach procesora CPU**, zamiast być zapisywane i odczytywane z pamięci RAM.

<div class="box-info">
<b>Jak rozpoznać CodeGen w Spark UI?</b> W planie zapytania zobaczysz gwiazdkę przy nazwie operatora, np. <code>*(1) Filter</code> oraz <code>*(1) Project</code> – oznacza to, że zostały one sklejone w jedną skompilowaną funkcję natywną `WholeStageCodegen (1)`.
</div>

<div class="source">Źródło: T. Neumann, "Efficiently Compiling Efficient Query Plans for Modern Hardware", VLDB 2011.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Wspomnieć postać prof. Thomasa Neumanna (TU Monachium, twórca bazy HyPer i Umbra). Jego artykuł z VLDB 2011 wywrócił świat silników bazodanowych do góry nogami: pokazał, że kompilując zapytanie SQL w locie do kodu maszynowego/bajtkodu, silnik działa tak szybko, jakby doświadczony programista C napisał dedykowaną pętlę pod jedno konkretne zapytanie.
-->

---

## 20. Przełom w Spark 3.x: Adaptive Query Execution (AQE)

Jaka była największa wada **Catalyst Optimizera** w Sparku 2.x? Podejmował wszystkie decyzje **w ciemno przed startem zadania**, bazując często na nieaktualnych lub brakujących statystykach tabel!

```
Klasyczny Spark 2.x (Statyczny plan wykonania):
[Plan fizyczny ustalony przed startem] ────► [Stage 1] ────► [Stage 2] ────► [Stage 3] (Nawet jeśli po drodze filtr odrzucił 99.9% danych!)

Spark 3.x+ z Adaptive Query Execution (AQE):
[Plan początkowy] ──► [Stage 1] ──► (STOP! Odczyt prawdziwych rozmiarów plików Shuffle!) ──► [Re-optymalizacja planu dla Stage 2!]
```

* **Dlaczego granica *Shuffle* jest idealnym momentem na korektę planu?**
  * Przed uruchomieniem zapytania Spark nie wie, ile wierszy przeżyje skomplikowany filtr `WHERE` albo regex.
  * Jednak każdy etap kończący się operacją **Shuffle** zapisuje pliki pośrednie na dysk wraz z dokładnymi statystykami co do bajta!
  * **AQE** zagląda do tych statystyk w trakcie trwania zadania i **przebudowuje dalszą część drzewa wykonania w locie**.

<div class="source">Źródło: W. Fan, H. van Hövell, M. Xue (Databricks), "Adaptive Query Execution: Speeding Up Spark SQL at Runtime", 2020.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Użyć analogii nawigacji samochodowej: Spark 2.x był jak papierowa mapa drukowana przed wyjazdem z Olsztyna do Warszawy – jeśli pod Mławą zdarzył się wypadek, i tak wjeżdżałeś w korek. Spark 3.x z AQE to Google Maps z danymi na żywo: po przejechaniu pierwszego odcinka sprawdza rzeczywisty ruch na trasie i dynamicznie zmienia drogę!
-->

---

## 21. Trzy supermoce AQE w walce z patologiami produkcyjnymi

Od wersji Spark 3.2 mechanizm **AQE jest włączony domyślnie** (`spark.sql.adaptive.enabled = true`) i automatycznie rozwiązuje trzy klasyczne problemy inżynierskie:

<div class="grid-3">
  <div class="card-node card-blue">
    <h4>1. Scalanie małych partycji</h4>
    <span class="sub">Coalescing Post-Shuffle Partitions</span>
    <ul>
      <li>Domyślnie po Shuffle Spark tworzy <b>200 partycji</b> (<code>spark.sql.shuffle.partitions=200</code>).</li>
      <li>Jeśli po filtrze zostało tylko 20 MB danych, 200 partycji po 100 KB tworzy problem małych plików.</li>
      <li><b>AQE automatycznie skleja</b> małe sąsiednie partycje w większe bloki (np. po 64 MB)!</li>
    </ul>
  </div>

  <div class="card-node card-green">
    <h4>2. Dynamiczny Broadcast Join</h4>
    <span class="sub">Converting SortMergeJoin to BHJ</span>
    <ul>
      <li>Tabela na dysku ma 500 GB, więc Catalyst planuje ciężki <code>SortMergeJoin</code>.</li>
      <li>W trakcie Stage 1 warunek <code>WHERE</code> wycina 99.9% wierszy i z 500 GB zostaje <b>tylko 8 MB</b>!</li>
      <li><b>AQE w locie anuluje Shuffle</b> drugiej tabeli i zamienia złączenie na błyskawiczny <code>BroadcastHashJoin</code>!</li>
    </ul>
  </div>

  <div class="card-node card-purple">
    <h4>3. Automatyczny Skew Join</h4>
    <span class="sub">Dynamically Optimizing Skew Joins</span>
    <ul>
      <li>AQE bada rozkład rozmiarów partycji po zapisie Shuffle.</li>
      <li>Gdy wykryje partycję znacznie większą od mediany (domyślnie > 5x mediana i > 256 MB), oznacza ją jako <b>skośną (Skewed)</b>.</li>
      <li><b>AQE automatycznie tnie gigantyczną partycję</b> na mniejsze kawałki i duplikuje odpowiadający klucz z drugiej tabeli!</li>
    </ul>
  </div>
</div>

<div class="box-info">
<b>Wracając do awarii w "TelecomPol":</b> Włączenie <code>spark.sql.adaptive.skewJoin.enabled = true</code> sprawia, że przy złączeniu gigantyczna partycja nr 200 zostaje automatycznie pocięta na kilkadziesiąt mniejszych zadań i rozdzielona na wszystkie wolne rdzenie klastra!
</div>

<div class="source">Źródło: Apache Spark 3.x SQL Performance Tuning Guide – "Adaptive Query Execution", 2024.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Zaznaczyć ważny detal inżynierski: AQE Skew Join automatycznie naprawia skośność podczas ZŁĄCZEŃ (JOIN), bo może zduplikować pasujący rekord z drugiej tabeli. Co jednak, gdy skośność występuje w czystym `GROUP BY` lub gdy AQE nie daje rady z ekstremalnym skosem? Tego dotyczy następny slajd!
-->

---

## 22. Ręczne zwalczanie ekstremalnego Data Skew: Solenie kluczy i izolacja NULL

Kiedy automatyczny AQE nie wystarcza (np. przy ciężkich agregacjach `GROUP BY` z funkcją `COUNT(DISTINCT)` lub oknach analitycznych `WINDOW`), inżynier stosuje dwie techniki:

<div class="grid-2">
  <div class="card-node card-orange">
    <h4>Technika 1: Odfiltrowanie i izolacja kluczy <code>NULL</code></h4>
    <span class="sub">Gdy gorącym kluczem jest brak danych</span>
    <ul>
      <li>W 80% przypadków produkcyjnych przyczyną skosu przy złączeniu jest klucz <code>NULL</code>, <code>""</code> lub <code>-1</code>.</li>
      <li>W SQL warunek <code>NULL = NULL</code> jest zawsze fałszywy – rekordy z <code>NULL</code> nigdy się nie złączą, a mimo to Spark przesyła je przez sieć do jednego workera!</li>
      <li><b>Rozwiązanie:</b> Podziel DataFrame na dwie części: <code>df_valid = df.filter("id IS NOT NULL")</code> (idzie do JOIN) oraz <code>df_nulls</code> (doklejany na końcu tanim <code>unionByName</code> bez Shuffle!).</li>
    </ul>
  </div>

  <div class="card-node card-blue">
    <h4>Technika 2: Solenie kluczy (Key Salting)</h4>
    <span class="sub">Sztuczne rozproszenie gorącego klucza na N wiader</span>
    <ul>
      <li>Doklejamy do klucza losową liczbę z przedziału <code>0..N-1</code> (np. zamiast <code>"PL"</code> tworzymy 20 kluczy: <code>"PL_0"</code>, <code>"PL_1"</code>, ..., <code>"PL_19"</code>).</li>
      <li> przy <b>Agregacji (GROUP BY):</b> Wykonujemy 2-etapową agregację – najpierw częściową po kluczu "posolonym" (20 rdzeni liczy równolegle), a potem końcową po kluczu oryginalnym.</li>
      <li>Przy <b>Złączeniu (JOIN):</b> Dużą tabelę solimy losową liczbą <code>0..19</code>, a w drugiej tabeli powielamy (<code>explode</code>) gorący klucz 20 razy.</li>
    </ul>
  </div>
</div>

```
Przykład dwuetapowej agregacji z soleniem klucza (Eliminacja wąskiego gardła dla gorącego klucza "HOT_KEY"):
[100 mln wierszy "HOT_KEY"] ──(Dodaj losowy salt 0..9)──► 10 Tasków liczy SUM po "HOT_KEY_0".."HOT_KEY_9" ──► 1 Task sumuje 10 wyników!
```

<div class="source">Źródło: H. Karau, R. Warren, "High Performance Spark", O'Reilly 2017; J. S. Damji et al., "Learning Spark", O'Reilly 2020.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Pokazać studentom elegancję izolacji NULL-i: jedna linijka `.filter(col("user_id").isNotNull())` przed złączeniem potrafi skrócić czas trwania zadania z 4 godzin do 3 minut i nie wymaga żadnego solenia kluczy!
-->

---

## 23. Kontrola partycji i pamięci podręcznej: `repartition` vs `coalesce` i `cache`

Świadome zarządzanie liczbą partycji oraz cyklem życia pamięci podręcznej odróżnia architekta od początkującego programisty:

| Operacja / Metoda | Co robi pod maską? | Czy wywołuje Shuffle? | Kiedy stosować w praktyce? |
|---|---|---|---|
| **`df.repartition(N)`** lub **`repartition(N, "kol")`** | Dzieli lub scala dane w dokładnie `N` równych partycji za pomocą `RoundRobin` lub `HashPartitioning`. | ✅ **TAK (Pełny Shuffle przez sieć!)** | Gdy musisz **zwiększyć** równoległość, wyrównać skośne partycje lub przygotować układ plików pod częsty filtr/złączenie po kolumnie `"kol"`. |
| **`df.coalesce(N)`** | Łączy istniejące partycje lokalnie na tych samych węzłach bez ruszania danych przez sieć. | ❌ **NIE (Narrow Transformation!)** | Wyłącznie gdy chcesz **zmniejszyć** liczbę partycji (np. po złożonym `filter()`, tuż przed zapisem do Parquet/Delta, aby uniknąć małych plików). |

---
## 23-01. Kontrola partycji i pamięci podręcznej: `repartition` vs `coalesce` i `cache`

<div class="box-warning">
<b>Pułapka nadużywania <code>df.cache()</code> / <code>df.persist()</code>:</b><br>
1. <b>Zasada jednokrotnego użycia:</b> Jeśli DataFrame jest używany w kodzie tylko raz (jeden liniowy potok), wywołanie <code>cache()</code> <b>spowolni zadanie</b> i zmarnuje RAM!<br>
2. <b>Kiedy używać Cache?</b> Tylko wtedy, gdy z tego samego kosztownie wyliczonego DataFrame'u odgałęziasz <b>wiele różnych akcji</b> (np. zapis do 3 tabel lub pętla trenowania modelu ML). Po zakończeniu pracy zawsze zwolnij pamięć metodą <code>df.unpersist()</code>!
</div>

<div class="source">Źródło: Apache Spark SQL Tuning Guide & RDD Persistence Levels (`StorageLevel.MEMORY_AND_DISK`), 2024.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Ostrzec przed ukrytym niebezpieczeństwem `coalesce(1)`! Wielu programistów pisze na końcu skryptu `df.coalesce(1).write.parquet(...)`, bo "chcą mieć jeden ładny plik". Ponieważ `coalesce` jest transformacją wąską, zmusza to CAŁY wcześniejszy etap (Stage) do wykonania się na JEDNYM wątku procesora! Jeśli trzeba zmniejszyć liczbę plików bez zabicia równoległości wcześniejszego etapu, używamy AQE lub `repartition`.
-->

---

## 24. Zmierzch ery czystego JVM: Natywne silniki wektoryzowane (C++ / Rust)

Przez dekadę kod wykonywał się na maszynie wirtualnej Javy (JVM). Jednak w latach 2021–2025 nastąpiła kolejna rewolucja sprzętowa: **zastąpienie warstwy wykonawczej JVM natywnymi silnikami wektoryzowanymi pisanymi w C++ oraz Rust**:

```
Tradycyjny Apache Spark (JVM + Tungsten):
[Kod PySpark / SQL] ──► [Catalyst Optimizer] ──► [Plan Fizyczny Spark] ──► [Wykonywanie w JVM (Whole-Stage CodeGen)]

Nowoczesny Stos Hybrydowy (Spark API + Natywny Silnik Wektoryzowany C++/Rust):
[Kod PySpark / SQL] ──► [Catalyst Optimizer] ──► [Plan Fizyczny] ──► [Silnik C++/Rust: Photon / Velox / DataFusion Comet]
                                                                                     ▲
                                                           100% wektoryzacja SIMD (AVX-512), układ kolumnowy Arrow, zero GC!
```

<div class="grid-3">
  <div class="card-node card-blue">
    <h4>Databricks Photon (C++)</h4>
    <span class="sub">Nagroda SIGMOD 2022</span>
    <ul>
      <li>Przepisany od zera w C++ wektoryzowany silnik wykonawczy dla platformy Databricks.</li>
      <li>W 100% kompatybilny z API Sparka – nie wymaga zmiany ani jednej linijki kodu!</li>
      <li>Przyspiesza zapytania SQL i zapisy Delta Lake średnio <b>3x – 8x</b>.</li>
    </ul>
  </div>

  <div class="card-node card-purple">
    <h4>Meta Velox & Gluten (C++)</h4>
    <span class="sub">Open Source (Meta / Intel)</span>
    <ul>
      <li>Otwartoźródłowa biblioteka wektoryzowanych operatorów C++ stworzona przez inżynierów Meta (VLDB 2022).</li>
      <li>Projekt <b>Apache Gluten</b> pozwala podpiąć silnik Velox bezpośrednio pod klaster Apache Spark Open Source.</li>
    </ul>
  </div>

  <div class="card-node card-green">
    <h4>Apache DataFusion Comet (Rust)</h4>
    <span class="sub">Akcelerator ASF w języku Rust</span>
    <ul>
      <li>Oficjalny subprojekt Apache DataFusion oparty natywnie na formacie <b>Apache Arrow</b>.</li>
      <li>Przejmuje wykonanie planu fizycznego Sparka w bezpiecznym pamięciowo kodzie Rust z instrukcjami SIMD.</li>
    </ul>
  </div>
</div>

<div class="source">Źródło: A. Behm et al., "Photon: A Fast Query Engine for Lakehouse Systems", ACM SIGMOD 2022; P. Pedreira et al., "Velox: Meta's Unified Execution Engine", VLDB 2022.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Pokazać studentom kierunek, w jakim zmierza cała branża: API Sparka (DataFrame / Spark SQL) wygrało wojnę o standard pisania kodu i nigdzie nie znika. Ale pod maską maszyna JVM jest wymieniana na natywne silniki C++ i Rust operujące na wektorach Arrow i instrukcjach SIMD procesora (Wykład 3!).
-->

---

## 25. Złota macierz strategii złączeń (Joins) w Apache Spark

Strategie fizyczne, jakich używa Spark do łączenia zbiorów danych, wraz z ich kosztami i ograniczeniami:

| Strategia fizyczna w Planie | Wymagany Shuffle? | Wymagane Sortowanie? | Obsługiwane warunki złączenia | Kiedy Spark jej używa i jakie niesie ryzyko? |
|---|---|---|---|---|
| **BroadcastHashJoin (BHJ)** | ❌ **Brak** (Tylko rozgłoszenie małej tabeli) | ❌ Brak | Równościowe (`=`) | **Najszybsza strategia!** Gdy jedna tabela jest mniejsza niż próg (np. `< 10–100 MB`). **Ryzyko:** Driver/Executor OOM przy zbyt dużej tabeli. |
| **ShuffleHashJoin (SHJ)** | ✅ **Tak** (Obie tabele po kluczu) | ❌ Brak | Równościowe (`=`) | Gdy jedna tabela jest wyraźnie mniejsza od drugiej, ale za duża na Broadcast. Buduje tablicę Hash per partycja bez kosztu sortowania. |
| **SortMergeJoin (SMJ)** | ✅ **Tak** (Obie tabele po kluczu) | ✅ **Tak** (Obie tabele) | Równościowe (`=`) | **Domyślny koń roboczy dla dużych tabel (TB/PB).** Bardzo stabilny pamięciowo (może robić *Spill* na dysk), ale wolniejszy przez sortowanie. |

---

## 25-01. Złota macierz strategii złączeń (Joins) w Apache Spark

(ciąg dalszy)

| Strategia fizyczna w Planie | Wymagany Shuffle? | Wymagane Sortowanie? | Obsługiwane warunki złączenia | Kiedy Spark jej używa i jakie niesie ryzyko? |
|---|---|---|---|---|
| **BroadcastNestedLoopJoin / CartesianProduct** | ❌ / ✅ | ❌ Brak | **Nierównościowe** (`<`, `>`, `BETWEEN`, brak `ON`) | **EKSTREMALNIE NIEBEZPIECZNE!** Złożoność $O(N \times M)$. Złączenie 1 mln wierszy z 1 mln wierszy daje **1 bilion porównań** i zawiesza klaster! |

<div class="box-info">
<b>Wskazówka inżynierska (Join Hints):</b> Jeśli wiesz na 100%, że tabela słownikowa po odfiltrowaniu jest mała, możesz wymusić strategię wskazówką: <code>df_duzy.join(F.broadcast(df_maly), "id")</code> lub w SQL: <code>SELECT /*+ BROADCAST(m) */ * FROM duzy d JOIN maly m ON ...</code>
</div>

<div class="source">Źródło: Apache Spark SQL Performance Tuning – "Join Strategy Hints for SQL Queries", 2024.</div>

<!--
NOTATKI DLA WYKŁADOWCY:
Uczulić studentów na `BroadcastNestedLoopJoin`. Pojawia się on zawsze wtedy, gdy analityk zapomni warunku `ON` albo napisze złączenie przedziałowe typu `ON a.timestamp BETWEEN b.start AND b.end` bez dodatkowego klucza równościowego. W dużych zbiorach danych oznacza to natychmiastową śmierć klastra.
-->

---

## 26. Studia decyzyjne dla inżynierów

<div class="box-info">
<b>Scenariusz A: Złączenie z tabelą klientów trwa 5 godzin i kończy się błędem OOM</b><br>
Twój zespół łączy tabelę kliknięć ze strony WWW (5 miliardów wierszy, 2 TB) z tabelą zarejestrowanych profili użytkowników (50 milionów wierszy, 15 GB) po kolumnie <code>user_id</code>. W Spark UI widzisz strategię <code>SortMergeJoin</code>, w której 199 tasków kończy się w 3 minuty, a 1 task ma rozmiar 450 GB danych Shuffle i wybucha błędem <code>OutOfMemoryError</code>. Analityk biznesowy mówi: <i>"Przecież 75% ludzi na stronie przegląda sklep bez logowania jako goście!"</i>.<br>
👉 <b>Pytanie inżynierskie:</b> Co znajduje się w kolumnie <code>user_id</code> dla gości? Dlaczego nawet automatyczny AQE Skew Join miał tu ciężkie życie i jak naprawisz ten potok w 3 linijkach kodu, skracając czas wykonania z 5 godzin do 4 minut?
</div>


<div class="">
Odpowiedź A: Goście niezalogowani mają `user_id = NULL` (lub `-1` / `"GUEST"`). Ponieważ stanowią 75% ruchu (1,5 TB!), wszystkie te wiersze mają identyczny hash i lądują w jednym tasku. Co więcej, gość niezalogowany NIGDY nie ma profilu w tabeli zarejestrowanych klientów! Zatem przesyłanie 1,5 TB przez sieć do JOIN-a jest całkowicie bezcelowe. Rozwiązanie: odfiltrować gości PRZED złączeniem (`df_logged = df.filter("user_id IS NOT NULL")`), wykonać błyskawiczny JOIN tylko na 25% zalogowanych, a gości dokleić na końcu przez `unionByName`!
</div>

---
## 27. Studia decyzyjne dla inżynierów

<div class="box-info">
<b>Scenariusz B: Kosztowny potok ML Feature Engineering w PySparku</b><br>
Data Scientist napisał skrypt w PySparku, który przygotowuje cechy dla modelu kredytowego na 500 GB danych. Skrypt zawiera 12 wywołań klasycznych funkcji <code>@udf</code> w Pythonie (formatujących daty i wyliczających logarytmy), po każdym kroku wywołuje <code>df.cache()</code> oraz <code>print(df.count())</code> do logów, a na samym końcu zapisuje wynik przez <code>df.coalesce(1).write.parquet()</code>. Zadanie mieli na 64-rdzeniowym klastrze przez 6 godzin.<br>
👉 <b>Pytanie inżynierskie:</b> Wymień wszystkie cztery kardynalne błędy architektoniczne w tym skrypcie i zaproponuj plan refaktoryzacji.
</div>

<div class="">
Odpowiedź B:
1. Zastąpić 12 klasycznych `@udf` wbudowanymi funkcjami `pyspark.sql.functions` (np. `F.log`, `F.date_format`) lub wektoryzowanymi Pandas UDF (Arrow).
2. Usunąć wszystkie pośrednie `print(df.count())`, które zmuszały klaster do 12-krotnego uruchamiania osobnych Jobów.
3. Usunąć zbędne wywołania `df.cache()` w liniowym potoku, które zapychały pamięć i wywoływały Disk Spill.
4. Usunąć zabójcze `coalesce(1)`, które dławiło cały ostatni etap do jednego rdzenia CPU!
</div>

---

## 28. Złote zasady inżyniera Apache Spark

1. **Nigdy nie walcz z optymalizatorem – pisz deklaratywnie:**
   * Używaj DataFrame API, Spark SQL i wbudowanych funkcji `pyspark.sql.functions`. Unikaj niskopoziomowych RDD oraz klasycznych funkcji UDF w Pythonie jak ognia.
2. **Traktuj operację Shuffle jak podatek od luksusu:**
   * Każdy `groupBy`, `join`, `distinct` i `repartition` to kosztowny zapis dyskowy i transfer sieciowy. Filtruj wiersze (`filter`) i odrzucaj zbędne kolumny (`select`) tak wcześnie, jak to możliwe, a małe tabele zawsze rozgłaszaj przez **BroadcastJoin**.
3. **Chroń Drivera przed przepełnieniem pamięci:**
   * Nigdy nie wywołuj `df.collect()` ani `df.toPandas()` na niezagregowanych tabelach produkcyjnych. Driver służy do dyrygowania klastrem, a nie do magazynowania terabajtów danych.
4. **Projektuj potoki z myślą o skośności danych (Data Skew):**
   * Zawsze zakładaj, że w danych produkcyjnych pojawią się klucze `NULL` oraz gorące klucze (*Hot Keys*). Korzystaj z **Adaptive Query Execution (AQE)**, izoluj wartości puste przed złączeniami i diagnozuj rozkład percentyli w **Spark UI**.

<!--
NOTATKI DLA WYKŁADOWCY:
Te 4 punkty to esencja praktycznej inżynierii obliczeń rozproszonych.
-->

---

## Podsumowanie Wykładu 4

* **Architektura i In-Memory DAG:** Apache Spark zastąpił dyskowy model MapReduce acyklicznym grafem wykonania (**DAG**) w pamięci RAM, zapewniając odporność na awarie poprzez logiczny graf rodowodu (**Lineage Graph**), a nie kosztowną replikację pamięci.
* **Model wykonania:** Praca w klastrze dzieli się według hierarchii **Application ➔ Job (Action) ➔ Stage (Shuffle Boundary) ➔ Task (1 Partycja = 1 Wątek CPU)**, przy czym transformacje wąskie (*Narrow*) wykonują się lokalnie w RAM, a szerokie (*Wide*) wymuszają wymianę sieciową **Shuffle**.
* **Patologie produkcyjne:** Trzy główne przyczyny awarii i spowolnień to narzut **Shuffle** (*Disk Spill*), błędy przepełnienia pamięci **OOM** (na poziomie Drivera lub Executora) oraz skośność danych (**Data Skew** powodująca powstawanie maruderów – *Stragglers*).
* **Silnik pod maską:** Wydajność Sparka napędzają trzy filary: kompilator **Catalyst Optimizer** (optymalizacje logiczne i kosztowe), **Project Tungsten** (pamięć *Off-Heap* `UnsafeRow` i *Whole-Stage CodeGen*) oraz **Adaptive Query Execution (AQE)** dynamicznie korygujący plan w trakcie działania.
* **Przyszłość:** Warstwa wykonawcza ewoluuje od czystego JVM ku natywnym, wektoryzowanym silnikom w C++ i Rust (**Databricks Photon**, **Velox**, **DataFusion Comet**).

---

<!-- _class: lead -->
<!-- _header: '' -->

# Dziękuję za uwagę!

### Następny wykład (W05):
**Chmurowe platformy danych i ekosystem Databricks**
*Fenomen Lakehouse w chmurze • Databricks vs Snowflake • Zarządzanie i bezpieczeństwo: Unity Catalog • Deklaratywne potoki Delta Live Tables (DLT) • Silnik Photon, MLflow i FinOps*

<br>

*Specyfikacje, materiały źródłowe i diagramy dostępne w repozytorium kursu.*
