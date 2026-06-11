# Log de Argentina

<www.lda.com>

El comportamiento de **Log de Argentina (LDA.com)** respecto al registro de frecuencias y bandas, así como la compatibilidad con el estándar ADIF, tiene razones tanto de diseño de la plataforma como técnicas.

Aquí te detallo los motivos de esto y cómo manejar tus archivos ADIF:

---

## ¿Por qué LDA prioriza la Banda sobre la Frecuencia exacta?

La razón principal por la que LDA (y muchas plataformas orientadas a diplomas y estadísticas nacionales) se centra de forma mandatoria en la **banda** y no en la frecuencia exacta se debe a la **simplificación del sistema de validación y la gestión de bases de datos**.

* **Validación de Diplomas:** El sistema de Log de Argentina está fuertemente automatizado para procesar y otorgar diplomas (como el *Diploma Argentina*, *Permanente*, etc.) y constancias de QSLs. Para la inmensa mayoría de las bases de estas activaciones, el requisito es confirmar el contacto en una **banda específica** (ej. 40m, 10m) y en un **modo determinado** (ej. FT8, SSB, CW). La frecuencia exacta (por ejemplo, `28.074 MHz`) es un dato secundario para el algoritmo de validación del diploma.
* **Normalización de Datos:** En el desarrollo de software para radioaficionados, obligar a que coincida la frecuencia exacta entre dos estaciones para validar un QSO automático es problemático. Si tu transceptor estaba calibrado en `7.074.10` y el de tu corresponsal en `7.074.00`, una validación por frecuencia estricta podría fallar. Al reducir el campo a la **banda**, se eliminan estas discrepancias de hardware o de "vicio" de sintonía.

Sin embargo, internamente el estándar de la plataforma suele procesar los datos completos, aunque en las grillas visuales o reportes públicos se simplifique mostrando únicamente la banda para mantener la interfaz limpia y enfocada en los objetivos del radioaficionado (el "casillero" de la banda completado).

---

## ¿Es posible enviarle archivos ADIF con la frecuencia exacta?

**Sí, es perfectamente posible y, de hecho, es lo ideal.** El formato **ADIF (Amateur Data Interchange Format)** está estandarizado globalmente. Un archivo ADIF bien generado incluye etiquetas específicas tanto para la frecuencia como para la banda.

Las etiquetas estándar que maneja el formato son:

* `<BAND:3>40M` (Define la banda)
* `<FREQ:7>7.07400` (Define la frecuencia exacta en Megahertz)

### ¿Cómo lo procesa LDA.com?

Cuando subes tu archivo ADIF a LDA:

1. **El parser (lector) de LDA lee todo el bloque del QSO.** Si el archivo contiene el campo `<FREQ>`, el sistema lo interpreta, calcula automáticamente a qué banda pertenece (mediante software) y llena el campo correspondiente en su base de datos.
2. Si tu software de libro de guardia (como *WSJT-X*, *JTDX*, *Log4OM*, o *N1MM*) genera el ADIF con la frecuencia exacta, **no necesitas borrarla ni modificar el archivo**. LDA lo aceptará sin problemas porque ignora los subcampos que no requiere para su validación de diplomas o simplemente realiza la conversión interna `Frecuencia ➔ Banda`.

### Recomendación práctica si tienes problemas de importación:

Si en algún momento LDA te rechaza un ADIF que solo contiene frecuencias y no bandas, es debido a que el script de importación de la web exige la etiqueta `<BAND>` de forma mandatoria para ahorrar recursos de cómputo en el servidor.

Si te topas con ese problema, puedes usar herramientas de gestión de logs como **LM Logger** (de DL2ALF) o **Adif Master** para abrir tu archivo, generar la columna de bandas automáticamente a partir de las frecuencias en un solo clic, y exportar una versión optimizada para LDA.
