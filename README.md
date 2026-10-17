# Cuando el camino más corto no es el mejor

Proyecto de Fundamentos de Inteligencia Artificial. Simula una zona inundada donde un agente tiene que llegar de un punto inicial a un refugio. El terreno tiene obstáculos, agua profunda que no se puede cruzar y zonas con distinto nivel de inundación. Comparamos tres algoritmos: BFS, búsqueda de costo uniforme (UCS) y A*.

La idea es ver si la ruta más corta es también la más conveniente.

## Equipo

- Castillo Gamboa Emir Ariel
- Dominguez Caamal Andres Said
- Kumul Ek Angela Rubi
- Pirin Solis David Antonio

Materia: Fundamentos de Inteligencia Artificial
Grupo: LITIID71
Profesor: Carlos André Castro Rodriguez 
Fecha de entrega: 17/oct/2026

## Cómo ejecutarlo

En Google Colab:

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/AngelaRKE/ElCaminoMasCorto/blob/main/notebooks/camino_mas_corto.ipynb)

Abrir el notebook y elegir Entorno de ejecución > Ejecutar todo. No hay que instalar nada.

En tu computadora (Python 3.10 o superior):

```bash
git clone https://github.com/AngelaRKE/ElCaminoMasCorto.git
cd ElCaminoMasCorto
pip install numpy pandas matplotlib jupyter
jupyter notebook notebooks/camino_mas_corto.ipynb
```

El notebook usa semillas fijas, así que siempre da los mismos resultados.

## Archivos

```text
ElCaminoMasCorto/
├── README.md
├── notebooks/
│   └── camino_mas_corto.ipynb
└── docs/
    └── Justificacion_ElCaminoMasCorto_LITIID71.pdf
```

El notebook tiene el código, las pruebas, las gráficas y las decisiones de diseño. El PDF de la carpeta `docs` resume las justificaciones del proyecto: por qué elegimos los costos y los algoritmos, los errores que detectamos, los resultados y las limitaciones.

## Cómo modelamos el terreno

| Código | Significado | Costo de entrar |
| --- | --- | --- |
| -1 | Obstáculo | no se puede pasar |
| 0 | Seco | 1 |
| 1 | Inundación leve | 2 |
| 2 | Inundación moderada | 4 |
| 3 | Inundación alta | 8 |
| 4 | Agua profunda | no se puede pasar |

El costo de cada paso es el de la celda a la que se entra, y el inicio no cuesta. Los movimientos son en 4 direcciones. BFS ignora los costos y busca el menor número de pasos. UCS y A* buscan el menor costo total, así que el riesgo sí afecta su decisión. El agua profunda es un bloqueo, no un costo alto.

## Resultados

Escenario base (cuadrícula de 10x12):

| Algoritmo | Costo | Movimientos | Nodos expandidos |
| --- | --- | --- | --- |
| BFS | 25 | 11 | 82 |
| UCS | 19 | 15 | 91 |
| A* | 19 | 15 | 56 |

- BFS encuentra la ruta más corta, pero cuesta 31.6 % más que la de UCS y A*.
- UCS y A* llegan al mismo costo, pero A* expande menos nodos.
- Cuando el río sube a nivel 3, UCS deja de cruzarlo y rodea por el puente seco. BFS no cambia de ruta.
- Si el refugio queda aislado, los tres algoritmos informan que no hay ruta.
- Las 46 comprobaciones automáticas del notebook pasan.

## Pruebas

1. Hay ruta hasta el refugio.
2. Se bloquea una zona de la ruta y la búsqueda se repite.
3. El refugio queda inaccesible.
4. Se cambia el riesgo del río y se vuelve a buscar.
5. La ruta más corta deja de ser la más conveniente.
6. Prueba adicional: metimos 6 errores a propósito en los algoritmos y el validador detectó 5. El sexto no cambia el resultado en este modelo de costos y está explicado en el notebook.

## Limitaciones

El terreno no cambia con el tiempo, el riesgo se cobra por celda sin acumularse y los costos los elegimos nosotros, no vienen de datos reales.

## Uso de IA

Usamos Claude para estructurar el notebook y escribir la primera versión del código y la documentación. Para verificarlo ejecutamos el notebook completo, comparamos los costos contra un verificador independiente y corrimos pruebas aleatorias y de mutación. Además, el equipo leyó todo el material generado, lo revisó, corrigió y le añadió contenido propio.