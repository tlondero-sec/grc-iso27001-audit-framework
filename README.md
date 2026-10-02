# LegalTech: Matriz de Riesgos Normativos & Framework de Contratos Inmutables

## 📋 Descripción del Proyecto

Este repositorio presenta un enfoque integral de **Gobierno, Riesgo y Cumplimiento (GRC)** aplicado a transacciones comerciales e inmobiliarias. El proyecto nace con el objetivo de cruzar los requerimientos legales y normativos de la República Argentina con soluciones técnicas criptográficas, garantizando que los procesos documentales cumplan con los estándares de **confidencialidad, integridad y disponibilidad (Tríada CIA)**.

El proyecto se divide en dos componentes principales:

1. **La Matriz de Aplicabilidad (En desarrollo):** Un mapeo normativo inspirado en frameworks de cumplimiento.
2. **El Framework de Contratos "Hasheables" (MVP Funcional):** Un motor en Python que genera, cifra y sella criptográficamente documentos legales.

---

## 🏗️ Componente 1: Framework de Contratos Inmutables (Python + Typst)

Para mitigar los riesgos de manipulación documental y repudiación de firmas, se desarrolló un pipeline automatizado (`generador_masivo.py`) que procesa datos estructurados y emite contratos con controles de seguridad embebidos.

### Características de Seguridad Implementadas:

* **Prevención de Inyecciones (Sanitización):** Implementación de la función `sanitizar_input()` que purga caracteres de control (`#`, `[`, `]`, `{`, `}`, `\`) antes de inyectar los datos en las plantillas, previniendo la ejecución de código arbitrario en el compilador.


* **Compilación de Alta Velocidad:** Uso de **Typst** en lugar de LaTeX o librerías PDF estándar, permitiendo renderizar documentos limpios pasándole los datos sanitizados por línea de comandos de forma aislada.


* **No Repudio e Integridad (Hashing):** Tras la compilación, la función `generar_hash_documento()` procesa el PDF en bloques de memoria (4096 bytes) y genera un **Hash SHA-256** único para cada contrato. Esto proporciona una trazabilidad probatoria sólida para auditorías.


* **Confidencialidad (Cifrado Simétrico On-the-Fly):** La función `cifrar_pdf_al_vuelo()` utiliza `pypdf` para aplicar cifrado al documento final, estableciendo los últimos 4 dígitos del DNI del cliente como contraseña de apertura y eliminando automáticamente el archivo en texto plano temporal (`pdf_path.unlink()`) para no dejar remanentes en disco.



---

## 📊 Componente 2: Matriz de Riesgos y Cumplimiento (WIP)

*Nota: Esta sección del proyecto se encuentra en etapa de diseño conceptual (Iniciada hace 1 mes).*

La matriz busca estructurar las obligaciones de ciberseguridad corporativa basándose en excelentes referencias del sector, tales como:

* La **Matriz de referencia normativa BCRA** para servicios financieros digitales, desarrollada por Bernardita Götte.


* El **Mapa de aplicabilidad normativa de seguridad de la información — Argentina**, también elaborado por Bernardita Götte.



**Objetivo del mapeo:**
Establecer un cruce entre los requisitos de la Ley 25.326 (Protección de Datos Personales), ISO 27001, y las disposiciones de la AAIP para asegurar que el almacenamiento y procesamiento de los contratos generados por el script cumplan legalmente con la retención, borrado seguro y transferencia de datos.

---

## 🚀 Uso del Generador (Prueba de Concepto)

El pipeline actual opera de forma 100% offline y *air-gapped* mediante un archivo `inquilinos_mock.csv` ubicado en el directorio `/data`.

```bash
# 1. Clonar el repositorio
git clone https://github.com/tlondero-sec/legaltech-inmutable-contracts.git
cd legaltech-inmutable-contracts

# 2. Instalar dependencias
pip install pypdf
# Nota: Se requiere tener Typst instalado en la variable de entorno PATH.

# 3. Ejecutar el pipeline
python generador_masivo.py

```

**Salida esperada:**
El sistema procesará cada fila, emitirá en consola el Hash SHA-256 de integridad y guardará los PDFs cifrados en la carpeta `/output_secure_pdfs/`.

---

## 🗺️ Roadmap de Desarrollo

* [x] Crear el pipeline ETL básico para generación de PDFs.
* [x] Implementar sanitización de inputs y mitigación de inyecciones.
* [x] Integrar motor de Hashing SHA-256 y Cifrado Simétrico.
* [ ] Finalizar el diseño de la Matriz de Riesgos GRC en formato tabla (Markdown/Excel).
* [ ] Integrar generación de un archivo de log/auditoría inmutable (`audit_trail.log`) que guarde todos los Hashes emitidos.
* [ ] Desarrollar una interfaz gráfica o CLI avanzada para la carga individual de contratos.
