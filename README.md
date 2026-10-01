# 🧬 BioSemantic Monitor

**WebSite:** [biosemantic-monitor.vercel.app](https://biosemantic-monitor.vercel.app/)

El **BioSemantic Monitor v5.0** es un panel de control interactivo que se ejecuta directamente en el navegador web a partir de un único archivo, diseñado para explorar datos de ocurrencia de biodiversidad sobre un mapa interactivo y enriquecerlos con fuentes semánticas (taxonomía, interacciones de especies y literatura taxonómica). Combina datos en vivo de ocurrencias de **GBIF** con fuentes de enriquecimiento como **Catalogue of Life (COL)**, **GloBI** y **Plazi**, presentando los resultados mediante mapas, un grafo de conocimiento dirigido por fuerzas, gráficos y métricas.

Todo funciona del lado del cliente en un solo archivo HTML, sin requerir procesos de compilación, backends ni claves de API.

---

## Características principales

### 🗺️ Vista de mapa

* **Ocurrencias de GBIF en vivo:** Se dibujan como marcadores (hasta 500 visibles simultáneamente en el mapa), codificados por color según el reino. Las especies de la lista integrada de la UICN se resaltan en color rojo.


* **Capa de densidad de GBIF:** Una capa de teselas de densidad que se adapta dinámicamente a los filtros activos de país y taxón.


* **Mapas base:** Oscuro, satelital, claro (Esri) y OpenStreetMap.


* **Filtros avanzados:** Rango de años, país (18 países preestablecidos) y grupo taxonómico (animales, plantas, hongos, aves).


* **Carga masiva (Bulk Load):** Permite paginar a través de la API de búsqueda de GBIF en lotes de 300 registros (con 5 solicitudes concurrentes). Se pueden seleccionar límites de 3,000, 10,000, 50,000, 100,000 o todos los disponibles, con barra de progreso y botón de parada (los desplazamientos de GBIF están limitados a 200,000 registros por carga).


* **Consulta por zona al hacer clic:** Permite hacer clic en cualquier parte del mapa para obtener hasta 100 ocurrencias en un radio aproximado de 0.05° alrededor del punto. El botón *Restaurar* permite volver al conjunto de datos completo.


* **Explorador geográfico:** Navegación jerárquica de Mundo → Continente → País; al seleccionar un país, el mapa se acerca automáticamente y aplica el filtro.


* **Navegador temporal de conjuntos de datos:** Un control deslizante de 1900 a 2025 que lista los registros cargados en un rango de $\pm 5$ años, agrupados por institución, con enlaces a los datasets originales en GBIF.


* **Panel de registros:** Una lista explorable (primeros 100 registros) con una vista detallada por registro que incluye taxonomía, país, año, institución, coordenadas y enlace al dataset.



### 🧠 Vista de análisis

* **Grafo de conocimiento (Diseño de fuerzas D3):** Un grafo centrado en países que conecta naciones, especies y entidades relacionadas. Es posible alternar entre la vista de *Todos los países* y un subgrafo específico de un país seleccionado o aleatorio.


* **Enriquecimiento semántico** para la especie seleccionada:
* **🔗 GloBI:** Interacciones entre especies (depredación, parasitismo, polinización, etc.) añadidas como nodos y aristas.


* **📜 Plazi:** Tratamientos taxonómicos provenientes de TreatmentBank.


* **📚 COL:** Datos taxonómicos obtenidos del Catalogue of Life mediante la API de ChecklistBank.




* **Gráficos (Chart.js):** Registros clasificados por especie, país, año (últimos 30 años) e institución.


* **Métricas:** Totales de registros, especies, países, instituciones y especies amenazadas, acompañados de tarjetas de clústeres según el tipo de nodo.



### 📡 Registro de fuentes de datos

La barra lateral organiza ocho categorías de fuentes (Taxonomía, Interacciones, Rasgos, Literatura, Genómica, Conservación, Repositorios, Ontologías) equipadas con interruptores de activación.

| Estado | Fuentes |
| --- | --- |
| **Disponible** | GBIF, Catalogue of Life, GloBI, Plazi TreatmentBank, IUCN (búsqueda local)

 |
| **Demo** (marcador de posición, sin API conectada por ahora) | TDWG, NCBI Taxonomy, Web of Life, Interaction Web DB, TRY, Amniote DB, EOL, BHL, Europe PMC, BOLD, GenBank, UniProt, CITES, DataONE, PANGAEA, OBO Foundry, BioPortal

 |

---

## Primeros pasos

1. Guarda el archivo como `index.html`.


2. Ábrelo en un navegador web moderno (Chrome, Firefox, Edge, Safari). Se requiere conexión a internet.



Ciertos navegadores limitan las solicitudes de origen cruzado en páginas abiertas con el protocolo `file://`. Si las APIs no cargan correctamente, se recomienda iniciar un servidor local simple:

```bash
python3 -m http.server 8000
# luego abre http://localhost:8000 en el navegador

```

---

## Uso

1. **Selecciona filtros** en la barra lateral izquierda (país, grupo taxonómico, rango de años) y haz clic en **Apply Filters**.


2. **Carga más datos** usando **Bulk Load → Start** (puedes especificar un límite previo si no deseas importar todo el conjunto).


3. **Explora el mapa:** haz clic en los marcadores para ver ventanas emergentes, selecciona cualquier zona vacía del mapa para una consulta puntual, utiliza el Explorador Geográfico para cambiar de región y abre **📅 Time** para acceder al navegador temporal.


4. Cambia a la pestaña **🧠 Analysis** para visualizar el grafo de conocimiento, gráficos y métricas.


5. Dentro del grafo de conocimiento, selecciona un nodo de especie y utiliza las opciones **GloBI / Plazi / COL** para enriquecerlo.



---

## Dependencias (cargadas vía CDN)

| Biblioteca | Versión | Propósito |
| --- | --- | --- |
| [Leaflet](https://leafletjs.com/) | 1.9.4 | Mapas

 |
| [Chart.js](https://www.chartjs.org/) | 4.4.0 | Gráficos

 |
| [D3](https://d3js.org/) | v7 | Grafo de conocimiento

 |

---

## Servicios externos

| Servicio | Utilizado para |
| --- | --- |
| [GBIF API](https://api.gbif.org) | Búsqueda de ocurrencias y teselas de densidad

 |
| [ChecklistBank / COL](https://api.checklistbank.org) | Enriquecimiento taxonómico

 |
| [GloBI](https://www.google.com/search?q=https://api.globalbioticinteractions.org) | Interacciones entre especies

 |
| [Plazi TreatmentBank](https://tb.plazi.org) | Tratamientos taxonómicos

 |
| [Esri ArcGIS](https://www.google.com/search?q=https://server.arcgisonline.com) / [OpenStreetMap](https://www.google.com/search?q=https://tile.openstreetmap.org) | Teselas de mapas base

 |

---

## Limitaciones conocidas

* **Respaldo de demostración (Demo):** Si GloBI o Plazi no se encuentran disponibles (o no devuelven resultados), la aplicación muestra datos etiquetados claramente como *demo*. Las interacciones demo de GloBI solo existen para un conjunto reducido de géneros (por ejemplo, *Panthera*, *Pan*, *Loxodonta*, *Gorilla*, *Ursus*, *Harpia*, *Tremarctos*).


* **Estado de la UICN:** Proviene de una pequeña lista estática integrada de 14 especies en el código fuente, no de la API oficial en vivo de la Lista Roja de la UICN.


* **Alternancia de fuentes:** Los selectores de estado en las fuentes reflejan visualmente cuáles están activas (y actualizan contadores), pero todavía no modifican de manera dinámica qué APIs se consultan en las peticiones.


* **Insignias de estado:** Aquellas etiquetas marcadas como `demo` funcionan como marcadores de posición para futuras integraciones.


* **Rendimiento:** Al cargar conjuntos de datos muy extensos, todos los registros se retienen en la memoria del navegador, renderizando únicamente los primeros 500 marcadores y 100 elementos de lista. Se aconseja utilizar filtros o límites reducidos en ordenadores con recursos limitados.


* **Límites de tasa y CORS:** Las APIs públicas pueden limitar solicitudes intensivas o bloquear peticiones de origen cruzado según el estado de la red.



---

## Personalización

Toda la configuración principal se encuentra en la parte superior del bloque `<script>`:

* `DATA_SOURCES`: El registro de fuentes (permite añadir nuevas fuentes o modificar sus estados y URLs de API).


* `IUCN`: Diccionario de consulta de especies amenazadas (asocia nombre científico con código de estado y nombre común).


* `CBOUNDS` y las opciones del elemento `<select id="sel-country">`: Cajas delimitadoras de países y listado de naciones.


* `GEO_HIERARCHY`: El árbol de continentes y países utilizado en el Explorador Geográfico.


* `BASEMAPS`: Proveedores de teselas de mapas.



---

## Atribución de datos

Los datos de ocurrencia pertenecen a © GBIF y sus proveedores de datos. La taxonomía pertenece a © Catalogue of Life. Las interacciones provienen de Global Biotic Interactions (GloBI) y los tratamientos literarios de Plazi. Por favor, asegúrese de citar los conjuntos de datos originales (enlazados desde cada registro) al reutilizar los resultados.