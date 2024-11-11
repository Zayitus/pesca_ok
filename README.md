

# **Predicción de Desembarques Pesqueros en Tierra del Fuego**

<a target="_blank" href="https://cookiecutter-data-science.drivendata.org/">
    <img src="https://img.shields.io/badge/CCDS-Project%20template-328F97?logo=cookiecutter" />
</a>

## **Descripción del Proyecto**

La provincia de **Tierra del Fuego**, ubicada en el extremo austral de Argentina, cuenta con una vasta riqueza en recursos pesqueros. Este proyecto tiene como objetivo predecir los **desembarques pesqueros** utilizando técnicas avanzadas de **Aprendizaje Automático**. Al construir un modelo predictivo, buscamos optimizar la gestión de recursos y mejorar la planificación en la industria pesquera de la región.

### **Motivación del Proyecto**
- **Optimización** de la gestión de recursos pesqueros.
- **Identificación** de patrones estacionales y tendencias en los desembarques.
- **Mejora** en la toma de decisiones para políticas pesqueras sostenibles.

## **Estructura del Proyecto**
Entrar a https://github.com/Zayitus/pesca_ok/tree/master/desembarques_pesca_tdf para acceder al proyecto
```
├── LICENSE            <- Licencia de uso del proyecto (open-source)
├── Makefile           <- Comandos de conveniencia como `make data` o `make train`
├── README.md          <- Información general y guía del proyecto
├── data
│   ├── external       <- Datos de fuentes externas
│   ├── interim        <- Datos intermedios que han sido transformados
│   ├── processed      <- Datos finales listos para modelado
│   └── raw            <- Datos originales y sin procesar
│
├── docs               <- Documentación del proyecto (usando mkdocs)
│
├── models             <- Modelos entrenados y serializados
│
├── notebooks          <- Notebooks Jupyter para análisis y exploración
│                         (E.g., `1.0-initial-data-exploration.ipynb`)
│
├── pyproject.toml     <- Archivo de configuración del proyecto y herramientas
│
├── references         <- Diccionarios de datos y material explicativo
│
├── reports            <- Informes generados (HTML, PDF, LaTeX, etc.)
│   └── figures        <- Gráficos y figuras generados para los informes
│
├── requirements.txt   <- Dependencias del proyecto (pip freeze)
│
├── setup.cfg          <- Configuración de herramientas como flake8
│
└── prediccion_desembarques
    ├── __init__.py             <- Inicializa el módulo de Python
    ├── config.py               <- Variables de configuración del proyecto
    ├── dataset.py              <- Scripts para cargar y procesar datos
    ├── features.py             <- Código para generar características para el modelado
    ├── modeling                
    │   ├── __init__.py 
    │   ├── predict.py          <- Inferencia y predicciones con modelos entrenados
    │   └── train.py            <- Entrenamiento de modelos
    └── plots.py                <- Generación de gráficos y visualizaciones
```

## **Requisitos**

Para ejecutar este proyecto, asegúrate de tener instaladas las siguientes dependencias:

```bash
pip install -r requirements.txt
```

### **Dependencias Principales**
- **Python** (>= 3.8)
- **Pandas** (para manipulación de datos)
- **NumPy** (para operaciones numéricas)
- **Matplotlib y Seaborn** (para visualizaciones)
- **XGBoost** (para modelado predictivo)
- **Scikit-learn** (para entrenamiento y evaluación de modelos)

## **Uso del Proyecto**

### **1. Clonar el repositorio**

```bash
git clone https://github.com/usuario/prediccion_desembarques.git
cd prediccion_desembarques
```

### **2. Preparar el entorno**

Asegúrate de que tienes un entorno virtual configurado:

```bash
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### **3. Ejecución de los Notebooks**

Abre el notebook principal para el análisis y la predicción:

```bash
jupyter notebook notebooks/1.0-initial-data-exploration.ipynb
```

### **4. Entrenamiento del Modelo**

Para entrenar el modelo desde cero:

```bash
python prediccion_desembarques/modeling/train.py
```

### **5. Generar Predicciones**

Para generar predicciones utilizando el modelo entrenado:

```bash
python prediccion_desembarques/modeling/predict.py
```

## **Datos**

Los datos utilizados en este proyecto provienen del [Instituto Provincial de Análisis e Investigación, Estadística y Censos (IPIEC)](https://ipiec.tierradelfuego.gob.ar/) de Tierra del Fuego. Los datos cubren un periodo desde **1990 hasta 2024**, con información mensual sobre los desembarques pesqueros en los puertos de **Ushuaia y Almanza**.

## **Resultados del Modelo**

### Modelo Original (XGBoost)
- **R² Score**: 0.80
- **Root Mean Squared Error (RMSE)**: 1228.92
- **Validación Cruzada (R² promedio)**: 0.75

### Modelo Mejorado (XGBoost Optimizado)
- **R² Score**: 0.94
- **Root Mean Squared Error (RMSE)**: 685.86
- **Mean Absolute Error (MAE)**: 573.69
- **Mejora porcentual:**
  - Reducción del RMSE: 48.6%
  - Mejora del R²: 23.7%

### Características del Modelo Mejorado
1. **Características Temporales**
   - Transformaciones cíclicas (mes_sin, mes_cos)
   - Variables de rezago (lag_1 a lag_12)
   - Diferencias temporales (diff_1, diff_12)

2. **Estadísticas Móviles**
   - Media móvil (moving_avg_3)
   - Desviación estándar móvil (moving_std_3)
   - Máximos y mínimos móviles

3. **Hiperparámetros Optimizados**
   ```python
   {
       'n_estimators': 500,
       'learning_rate': 0.05,
       'max_depth': 5,
       'min_child_weight': 7,
       'subsample': 0.6,
       'colsample_bytree': 0.7,
       'gamma': 0.1
   }
   ```

4. **Características más Importantes**
   - diff_1 (20.63%): Cambios mensuales
   - moving_min_3 (13.58%): Mínimos recientes
   - moving_avg_3 (11.41%): Tendencia reciente
   - mes_12 (10.67%): Efecto diciembre
   - diff_12 (10%): Cambios anuales

## **Gráficos Importantes**

### **Análisis de Residuos**
Los residuos muestran que el modelo tiene un desempeño adecuado, aunque con algunos picos que indican errores en periodos específicos.

### **Distribución de los Errores**
La distribución de los errores es aproximadamente simétrica, indicando un buen ajuste sin sesgos significativos.

### **Evolución del Error Absoluto**
El error absoluto varía a lo largo del tiempo, sugiriendo la necesidad de variables adicionales para mejorar la precisión en ciertos meses.

## **Contribuciones**

Si deseas contribuir al proyecto:

1. Haz un fork del repositorio.
2. Crea una rama (`git checkout -b feature/nueva-funcionalidad`).
3. Realiza tus cambios y haz commit (`git commit -am 'Añadir nueva funcionalidad'`).
4. Sube tus cambios (`git push origin feature/nueva-funcionalidad`).
5. Crea un Pull Request.

## **Licencia**

Este proyecto está licenciado bajo la licencia MIT. Consulta el archivo [LICENSE](LICENSE) para obtener más detalles.

## **Contacto**

Si tienes preguntas o sugerencias, no dudes en contactarme:

- **Correo**: schvartz.g@gmail.cm
- **LinkedIn**: www.linkedin.com/in/gaston-schvartz
- # **Predicción de Desembarques Pesqueros en Tierra del Fuego**

![Banner](ruta/a/tu/imagen/banner.jpg)

<a target="_blank" href="https://cookiecutter-data-science.drivendata.org/">
    <img src="https://img.shields.io/badge/CCDS-Project%20template-328F97?logo=cookiecutter" />
</a>

## **Descripción del Proyecto**

La provincia de **Tierra del Fuego**, ubicada en el extremo austral de Argentina, cuenta con una vasta riqueza en recursos pesqueros. Este proyecto tiene como objetivo predecir los **desembarques pesqueros** utilizando técnicas avanzadas de **Aprendizaje Automático**. Al construir un modelo predictivo, buscamos optimizar la gestión de recursos y mejorar la planificación en la industria pesquera de la región.

[... contenido anterior ...]



[... resto del contenido ...]

Para agregar la imagen como banner, deberías:
1. Guardar la imagen en la carpeta del proyecto (por ejemplo en `reports/figures/banner.jpg`)
2. Referirla en el README usando la ruta relativa correcta
3. Ajustar el tamaño si es necesario usando HTML:
```html
<p align="center">
  <img src="reports/figures/banner.jpg" alt="Banner" width="100%"/>
</p>

