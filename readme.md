# Práctica de ETL con pandas

## ETL y Validación de Datos

1. Importa la biblioteca pandas con el siguiente comando:
```python
import pandas as pd
```
2. Carga el dataset 'netflix_titles.csv' utilizando pandas:
```python
df = pd.read_csv('netflix_titles.csv')
```
3. Visualiza la información general del dataset utilizando el método `info()`:
```python
df.info()
```
4. Valida los datos nulos y analiza la viabilidad de eliminar los registros nulos. Puedes usar el método `isnull().sum()` para contar los valores nulos en cada columna:
```python
df.isnull().sum()
```

## Creación de DataFrames y Normalización de Datos

1. Crea dos DataFrames, uno para películas y otro para series. Puedes usar el método `loc[]` para filtrar los datos:
```python
df_peliculas = df.loc[df['type'] == 'Movie']
df_series = df.loc[df['type'] == 'TV Show']
```
2. Normaliza la fecha. Asegúrate de convertir la columna 'date_added' a formato de fecha:
```python
df['date_added'] = pd.to_datetime(df['date_added'])
```
3. Normaliza la columna 'duration'. Para las películas, extrae el valor numérico en minutos. Para las series, cambia el nombre de la columna a 'temporadas' y exprésala en valores enteros:
```python
df_peliculas['duration'] = df_peliculas['duration'].str.replace(' min','').astype(int)
df_series = df_series.rename(columns={'duration': 'temporadas'})
df_series['temporadas'] = df_series['temporadas'].str.replace(' Seasons','').str.replace(' Season','').astype(int)
```

## Función de Búsqueda de Filmografía

1. Crea una función que busque en los registros de 'cast' y genere la filmografía dado el nombre del actor. Puedes usar el método `str.contains()` para buscar el nombre del actor en la columna 'cast':
```python
def filmografia(actor):
    return df[df['cast'].str.contains(actor, na=False)]
```
Para usar la función, simplemente pasa el nombre del actor como argumento:
```python
filmografia('Tom Hanks')
```