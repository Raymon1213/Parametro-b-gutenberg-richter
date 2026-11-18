# Estimación del parámetro *b* de Gutenberg–Richter en Chile
### Máxima Verosimilitud (MLE) vs Bayes Empírico (EB)

**Grupo 7:** Benjamín Iribarren · Ramón Espinoza · Gaspar Albornoz
Estadística Avanzada (ICD3152) · Pontificia Universidad Católica de Valparaíso

🔗 **Panel interactivo:** https://Raymon1213.github.io/Parametro-b-gutenberg-richter/

## Objetivo
Estimar el parámetro *b* de la ley de Gutenberg–Richter, `log10 N(M) = a − b·M`, en cinco macrozonas de Chile y evaluar si compartir información entre zonas (Bayes Empírico, James–Stein) mejora la estimación respecto de la máxima verosimilitud.

## Datos
Catálogo del Centro Sismológico Nacional (Universidad de Chile): 44.675 sismos, 1520–2025, 11 variables (`eventos_filtrados_mapa.csv`). Se usan `mag`, `lat`, `anio` (y `mag_type`, `MWconv` para sensibilidad). La profundidad se imputa con MICE pero no entra a la estimación de *b*.

## Método
1. Filtro por magnitud de completitud **Mc = 4,2** (máxima curvatura + 0,2): 30.924 eventos.
2. **MLE con corrección de Utsu** (Δm = 0,1): `b = log10(e) / (M̄ − (Mc − Δm/2))`, `SE ≈ b/√n`.
3. **Bayes Empírico** normal–normal (DerSimonian–Laird) y James–Stein.
4. **Validación:** bootstrap iid, jackknife por año, bootstrap por bloques de años, KS, submuestreo (R = 500).
5. **Robustez:** Mc ∈ {4,0; 4,2; 4,5}, escala MWconv, exclusión de Perú/Bolivia, b por período.

Configuración reproducible: Δm = 0,1 · bootstrap = 1000 · R = 500 · semilla = 42.

## Resultados principales
- *b* va de ≈0,50 (Zona Austral) a ≈0,94 (Zona Central); heterogeneidad muy significativa (Cochran Q = 339,7).
- MLE ≈ Bayes Empírico con macrozonas grandes (contracción 0,4–5,4 %).
- Con pocas observaciones por zona (n = 20) EB reduce el ECM cerca de la mitad; con n = 350 la mejora es pequeña.
- El error estándar por bloques de años es 1,5–5,7 veces el analítico (dependencia temporal).
- Zona Austral: ajuste exponencial débil (KS D ≈ 0,10).

## Archivos
| Archivo | Descripción |
|---|---|
| `index.html` | Panel interactivo (página única, sin servidor) |
| `panel_template.html` | Plantilla editable del panel (contiene el marcador `__DATA__`) |
| `datos_panel.json` | Datos calculados que alimentan el panel |
| `Grupo7_ProyectoFinal_corregido.ipynb` | Análisis completo y reproducible |
| `eventos_filtrados_mapa.csv` | Catálogo sísmico |

## Cómo reproducir
1. Abrir el notebook en Google Colab y subir `eventos_filtrados_mapa.csv` y `panel_template.html`.
2. *Entorno de ejecución → Ejecutar todo*.
3. La última celda genera `datos_panel.json` y `panel.html`; renombrar `panel.html` a `index.html` para publicarlo.

Regenerar el panel sin el notebook:
```python
open('index.html','w',encoding='utf-8').write(
    open('panel_template.html',encoding='utf-8').read().replace(
        '__DATA__', open('datos_panel.json').read()))
```

## Limitaciones
Escalas de magnitud mixtas · un solo Mc · dependencia temporal · ≈11 % de eventos fuera de Chile (Perú/Bolivia) dentro de Norte Grande · solo cinco zonas para estimar la variabilidad entre zonas.
