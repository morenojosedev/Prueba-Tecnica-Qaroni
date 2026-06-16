---

##  Requisitos

- Cuenta de Google para ejecutar Colab
- Power BI Desktop para abrir el dashboard
- No se requiere instalación local de PySpark

---

##  ETL — Instrucciones de ejecución

### Google Colab
1. Abrir el enlace: https://colab.research.google.com/github/morenojosedev/Prueba-Tecnica-Qaroni/blob/main/etl/PruebaTecnica.ipynb
2. Ir a **Entorno de ejecución** → **Ejecutar todo**
3. Las tablas de salida se generan automáticamente en `data/output/`

---

##  Nota importante sobre los datos

Los archivos CSV de entrada contienen caracteres especiales
(tildes, ñ) que pueden aparecer corruptos si se abren
directamente. El notebook los corrige automáticamente
usando `ftfy` antes de procesar los datos.

---

##  Dependencias

```bash
pip install ftfy pyspark pandas
```

Todas las dependencias se instalan automáticamente
al ejecutar el notebook en Google Colab.

---

##  Actualizar el Dashboard con nuevos datos

Después de ejecutar el ETL en Google Colab:

1. Descargar las tablas de salida generadas:
   - `tabla_salida_1.csv`
   - `tabla_salida_2.csv`
2. Reemplazar los archivos en la carpeta local donde
   está conectado el archivo `dashboard.pbix`
3. Abrir `dashboard.pbix` en Power BI Desktop
4. Ir a **Inicio** → **Actualizar**
5. El dashboard se actualiza automáticamente con
   los nuevos datos generados por el ETL

###  Si los archivos están en una carpeta diferente

**Opción 1 — Cambiar ruta por archivo:**
1. Power BI Desktop → pestaña **Inicio** → **Transformar datos**
2. En Power Query → **Configuración del origen de datos**
3. Selecciona el archivo → clic en **Cambiar origen**
4. Busca la nueva carpeta y selecciona el archivo
5. Repite para cada CSV
6. Clic en **Cerrar y aplicar**

**Opción 2 — Cambiar ruta global (más rápido):**
1. Power Query → **Configuración del origen de datos**
2. Selecciona todos los archivos con **Ctrl + clic**
3. Clic en **Cambiar origen**
4. Selecciona la nueva carpeta
5. **Aceptar** → **Cerrar y aplicar**

---

##  Tablas de Salida
### Tabla 1 — Impacto Ambiental Empresas
| Campo | Descripción |
|---|---|
| empresa_id | Identificador único de la empresa |
| nombre | Nombre oficial de la empresa |
| pais | País de la empresa |
| emisiones_netas | Emisiones después de aplicar reducciones |
| cumple_normativa | 1 = Cumple, 0 = No cumple |
| regulacion_id | Regulación evaluada |
| costo_impuesto | Multa estimada si no cumple esa regulación |

> ⚠️ Archivo comprimido: `tabla_salida_1.rar` (1.52MB / 135MB descomprimida)
> Contiene **2.462.700 filas** — cruce NxMxO: 
> 24.627 empresas × 10 regulaciones × 10 países.
> Descomprimir antes de usar en Power BI.

### Tabla 2 — Beneficios Proyectos Energéticos
| Campo | Descripción |
|---|---|
| empresa_id | Identificador de la empresa |
| tipo_energia | Tecnología energética del proyecto |
| total_inversion | Inversión total por tipo de energía |
| energia_generada_total | Capacidad total de generación |
| emisiones_evitadas_total | Total de emisiones evitadas |
| retorno_estimado | Inversión / emisiones evitadas |

---

##  Modelado de Datos Power BI

Modelo en estrella con:
- **2 tablas FACT:** FACT_Riesgo_Regulatorio, FACT_Oportunidades_Inversion
- **3 dimensiones:** Dim_Empresas, Dim_Regulaciones, Dim_Tecnologias
- **1 tabla de medidas:** TablaMedidas
- **1 tabla RLS:** MapeoUsuarios

---

## 🔐 Seguridad — Row Level Security (RLS)

| Rol | Acceso |
|---|---|
| Sustainability Officer Global | Acceso total al dashboard |
| AudRegStatic1 | Solo ve empresas de REG000000 |
| AudRegStatic2 | Solo ve empresas de REG000001 |
| AudRegStatic3 | Solo ve empresas de REG000002 |
| AudRegStatic4 | Solo ve empresas de REG000003 |
| AudRegStatic5 | Solo ve empresas de REG000005 |
| AudRegStatic6 | Solo ve empresas de REG000008 |
| AudRegStatic7 | Solo ve empresas de REG000010 |
| AudRegStatic8 | Solo ve empresas de REG000011 |
| AudRegStatic9 | Solo ve empresas de REG000016 |
| AudRegStatic10 | Solo ve empresas de REG000019 |

Los roles de auditor están implementados con filtros DAX estáticos
sobre `FACT_Riesgo_Regulatorio[regulacion_id]`. Cada auditor
solo puede ver las empresas asociadas a su regulación asignada.

En producción con Power BI Service, los filtros estáticos se
reemplazarían por `USERPRINCIPALNAME()` cruzado con la tabla
`MapeoUsuarios`, asignando usuarios reales a cada rol.

**Para probar los roles en Power BI Desktop:**
1. Pestaña **Modelado** → **Ver como**
2. Selecciona el rol a probar
3. El dashboard muestra solo los datos permitidos para ese rol
4. Clic en **Detener la visualización** para volver a la vista completa

---

##  Dashboard Power BI

### Página 1 — Resumen Ejecutivo
Responde: ¿Cuál es el estado global de riesgo y oportunidad?
- 5 KPIs principales
- Top 5 empresas con mayor exposición fiscal
- Impacto ambiental por tipo de energía
- Conclusión ejecutiva dinámica
- Paleta accesible para daltonismo (azul-amarillo)

### Página 2 — Detalle Riesgo Regulatorio
Responde: ¿Qué empresas incumplen y cuánto deben pagar?
- Top 10 empresas con mayor multa potencial
- % de cumplimiento por regulación
- Listado completo de empresas incumplidoras
- Conclusión operativa dinámica

---

##  Tecnologías utilizadas

| Tecnología | Uso |
|---|---|
| PySpark | ETL y transformación de datos |
| Google Colab | Entorno de ejecución del ETL |
| Power BI Desktop | Desarrollo del dashboard |
| DAX | Medidas y columnas calculadas |
| ftfy | Corrección de encoding en archivos CSV |

---

##  Autor
**Nombre:** José Moreno
**Email:** morenojose.dev@gmail.com
