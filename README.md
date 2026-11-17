# Bitcoin y criptomonedas — métricas de riesgo para LATAM

> Módulo de Python y notebook para medir volatilidad, drawdown, Sharpe/Sortino y correlación de BTC, ETH, BNB, ILF y GLD, desde la perspectiva de un inversor latinoamericano. Para analistas y estudiantes de finanzas.

**Estado (2026-11-17):** PARCIAL — `cripto_metrics.py` es un módulo y CLI funcional en su código; el notebook no tiene outputs guardados; no hay datos de BOB ni de inflación boliviana en el repo, así que la comparación BTC frente a inflación no se puede reproducir; el bloque de salida de ejemplo del README anterior se retira; falta `requirements.txt` y licencia.
**Repo:** kapitalya/bitcoin-latam-analisis · **Rama por defecto:** main · **Alias/nombres anteriores:** ninguno

> Aviso: análisis educativo, no es consejo financiero. Bitcoin es un activo muy volátil; los drawdowns históricos superiores al 70% no garantizan nada hacia adelante.

## Qué hace

- `cripto_metrics.py` (raíz): descarga precios con `yfinance` (`descargar_precios`) y calcula retornos logarítmicos (`retornos_log`), drawdown (`calcular_drawdown`), Sharpe y Sortino con tasa libre de riesgo configurable (`metricas_resumen`), correlación rolling (`correlacion_rolling`) y ciclos bull/bear sobre el precio de BTC (`ciclos_mercado`, umbrales por defecto de 20%). Como script, imprime el resumen y guarda `datos/metricas_cripto.csv`.
- `bitcoin_latam.ipynb`: exploración y visualizaciones con `TICKERS` = BTC-USD, ETH-USD, BNB-USD, ILF y GLD. Sin outputs guardados.

## Stack

Python. Librerías importadas: `numpy`, `pandas`, `scipy` (`stats`), `yfinance` (script y notebook), `matplotlib` y `seaborn` (notebook). Jupyter Notebook.

## Estructura

```
bitcoin-latam-analisis/
├── bitcoin_latam.ipynb          # Notebook de análisis (sin outputs guardados)
├── cripto_metrics.py            # Módulo y CLI de métricas (en la raíz, no en src/)
├── .github/workflows/blank.yml  # Plantilla de GitHub (ver Pruebas y CI)
└── README.md
```

`datos/` e `img/` no existen en el repo: el script crea `datos/` al guardar su CSV (sin verificar: que el script cree el directorio si no existe).

## Instalación y uso

```bash
git clone https://github.com/kapitalya/bitcoin-latam-analisis
cd bitcoin-latam-analisis

python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate
pip install numpy pandas scipy yfinance matplotlib seaborn jupyter   # sin requirements.txt: versiones sin fijar
mkdir -p datos
```

Análisis por línea de comandos (desde la raíz del repo):

```bash
# Tickers por defecto (BTC-USD, ETH-USD, GLD, ILF) desde 2020-01-01
python cripto_metrics.py

# Tickers y fecha de inicio personalizados
python cripto_metrics.py --tickers BTC-USD ETH-USD SOL-USD --inicio 2022-01-01

# Otra tasa libre de riesgo (anual, decimal)
python cripto_metrics.py --rfr 0.06
```

Uso como módulo (desde la raíz del repo):

```python
from cripto_metrics import descargar_precios, metricas_resumen, calcular_drawdown

precios = descargar_precios(['BTC-USD', 'GLD'], inicio='2023-01-01')
resumen = metricas_resumen(precios)
print(resumen)
```

El README anterior usaba `from src.cripto_metrics import ...`; esa ruta no existe en el repo.

## Variables de entorno (solo nombres)

Ninguna. No hay `.env.example`. `yfinance` no requiere clave.

## Pruebas y CI

- No hay pruebas automatizadas.
- `.github/workflows/blank.yml` es la plantilla por defecto de GitHub: se ejecuta en push y PR a `main`, pero solo hace `echo Hello, world!`. No valida el código.

### Resultados

- El bloque "Output del Script" del README anterior (por ejemplo, BTC +156.3% anual, Sharpe 2.10, drawdown máximo -77.2%) estaba escrito a mano, sin fecha de corte ni datos en el repo. Se retira. Resultados no verificables en el repo; para obtener cifras, ejecutar el CLI y citar la fecha de corte.
- La sección del notebook "Comparación BTC vs inflación boliviana como cobertura" es solo un encabezado: no hay código en el repo que cargue BOB ni datos de inflación boliviana. No reproducible; se retira de este README hasta que exista ese análisis.

## Despliegue

No aplica: el repo es de análisis local.

## Estado y relaciones

- Repositorio independiente. El README anterior enlazaba repos de una cuenta personal fuera de la organización (`Serius69/*`); no auditados en este corte.

## Precios y planes

No aplica (repositorio de análisis, sin venta). Precios y planes: fuente única `Vault/EMPRESA/portafolio/PRECIOS-APROBADOS-20261108.md`.

## Seguridad, privacidad y derechos

- Sin secretos en el repo.
- Fuente de precios: Yahoo Finance (vía `yfinance`), sujeta a sus términos de uso; no verificados en este corte.
- Licencia: el repo no tiene archivo LICENSE; no hay derechos de reutilización declarados.

## Convenciones de trabajo

Cambios por rama `codex/*` y PR contra `main` (no hay rama `develop`). CI: el workflow de plantilla no valida nada; no se considera CI verde a efectos de revisión.
