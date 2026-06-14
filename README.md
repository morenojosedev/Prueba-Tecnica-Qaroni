# Prueba-Tecnica

## ⚙️ ETL — Instrucciones de ejecución

###  Google Colab
1. Abrir el enlace: https://colab.research.google.com/github/morenojosedev/Prueba-Tecnica-Qaroni/blob/main/PruebaTecnica.ipynb
2. Ir a **Runtime** → **Run all**
3. Las tablas de salida se generan automáticamente en `/data/`

##  Tablas de Salida

### Tabla 1 — Impacto Ambiental Empresas
| Campo | Descripción |
|---|---|
| empresa_id | Identificador único de la empresa |
| nombre | Nombre oficial de la empresa |
| emisiones_netas | Emisiones después de aplicar reducciones |
| cumple_normativa | 1 = Cumple, 0 = No cumple |
| regulacion_id | Normativa aplicable a la empresa |
| costo_impuesto | Multa estimada por exceso de emisiones |

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

## Seguridad — Row Level Security (RLS)

| Rol | Acceso |
|---|---|
| Sustainability Officer Global | Acceso total al dashboard |
| Auditor Regulacion | Solo ve empresas de su regulación asignada |

En producción con Power BI Service, el acceso se gestiona 
mediante `USERPRINCIPALNAME()` cruzado con la tabla `MapeoUsuarios`.

---

##  Dashboard Power BI

### Página 1 — Resumen Ejecutivo
Responde: ¿Cuál es el estado global de riesgo y oportunidad?
- 5 KPIs principales
- Top 5 empresas con mayor exposición fiscal
- Impacto ambiental por tipo de energía
- Conclusión ejecutiva dinámica

### Página 2 — Detalle Riesgo Regulatorio
Responde: ¿Qué empresas incumplen y cuánto deben pagar?
- Top 10 empresas con mayor multa potencial
- % de cumplimiento por regulación
- Listado completo de empresas incumplidoras

---

##  Tecnologías utilizadas

| Tecnología | Uso |
|---|---|
| PySpark | ETL y transformación de datos |
| Google Colab | Entorno de ejecución del ETL |
| Power BI Desktop | Desarrollo del dashboard |
| DAX | Medidas y columnas calculadas |
| Power BI Service | Publicación y RLS en producción |

---

## 👤 Autor
Nombre: José Moreno
Email: morenojose.dev@gmail.com
