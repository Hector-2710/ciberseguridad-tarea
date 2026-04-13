# Análisis de Vulnerabilidades y Generación de SBOMs

Este proyecto automatiza la generación de *Software Bill of Materials* (SBOM) y el análisis de vulnerabilidades para repositorios de la organización `spring-projects` en GitHub. Utiliza herramientas estándar de la industria y proporciona un análisis cuantitativo detallado mediante Jupyter Notebooks.

## 🎯 Objetivo del Proyecto

1. **Seleccionar una organización real:** Se eligió `spring-projects` y se filtraron 33 repositorios con actividad reciente (último mes).
2. **Generar SBOMs:** Automatizar la creación de inventarios de dependencias usando `syft`.
3. **Análisis de Vulnerabilidades:** Escanear los SBOMs generados en busca de vulnerabilidades conocidas (CVEs) utilizando `grype`.
4. **Análisis Cuantitativo:** Consolidar los resultados en un Jupyter Notebook reproducible para identificar tendencias, niveles de severidad y las dependencias más afectadas.

## 📂 Estructura del Repositorio

```text
.
├── data/
│   ├── repos/                 # Código fuente de los repositorios clonados
│   ├── repos.json             # Archivo de configuración con los repositorios a analizar
│   ├── results/               # Archivos SBOM generados en formato JSON
│   └── vulnerabilities/       # Reportes de vulnerabilidades generados por Grype
├── nbs/
│   └── sbom/
│       ├── analisis_vulnerabilidades.ipynb  # Notebook principal con el análisis cuantitativo
│       ├── explicacion_script_sbom.ipynb    # Notebook explicando el proceso de Syft
│       └── generacion_sbom.ipynb            # Notebook interactivo de generación
├── scripts/
│   ├── add_submodules.py          # Script para clonar/actualizar repositorios localmente
│   ├── analyze_vulnerabilities.py # Script para escanear SBOMs con Grype
│   └── generate_sboms.py          # Script para orquestar la generación de SBOMs con Syft
└── README.md                      # Este archivo
```

## 🛠️ Requisitos Previos

Para ejecutar este proyecto desde cero, necesitas tener instalados los siguientes componentes:

1. **Python 3.8+**
2. **Dependencias de Python:**
   ```bash
   pip install pandas matplotlib seaborn jupyter
   ```
3. **Syft** (Generador de SBOM):
   ```bash
   curl -sSfL https://raw.githubusercontent.com/anchore/syft/main/install.sh | sh -s -- -b /usr/local/bin
   ```
4. **Grype** (Escáner de vulnerabilidades):
   ```bash
   curl -sSfL https://raw.githubusercontent.com/anchore/grype/main/install.sh | sh -s -- -b /usr/local/bin
   ```

## 🚀 Instrucciones de Uso

El repositorio ya contiene los datos pre-generados (`data/results` y `data/vulnerabilities`), por lo que puedes ir directamente al paso 4 si solo deseas ver el análisis. Si deseas reproducir el proceso completo:

### 1. Obtener los repositorios
Descarga el código fuente de los repositorios listados en `data/repos.json`:
```bash
python scripts/add_submodules.py
```

### 2. Generar SBOMs
Crea los archivos SBOM en formato JSON para cada repositorio utilizando Syft:
```bash
python scripts/generate_sboms.py
```
*Los resultados se guardarán en `data/results/`.*

### 3. Analizar Vulnerabilidades
Escanea los SBOMs generados para encontrar vulnerabilidades usando Grype:
```bash
python scripts/analyze_vulnerabilities.py
```
*Los reportes JSON se guardarán en `data/vulnerabilities/`.*

### 4. Visualizar el Análisis Cuantitativo
Abre el notebook interactivo para explorar los gráficos y resultados:
```bash
jupyter notebook nbs/sbom/analisis_vulnerabilidades.ipynb
```
En el notebook encontrarás gráficos sobre la distribución de severidad (Crítica, Alta, Media, Baja), los repositorios más afectados y el "Top 10" de paquetes o dependencias que introducen más riesgos.
