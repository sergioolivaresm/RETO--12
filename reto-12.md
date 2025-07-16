# RETO 11
## Sergio Olivares Martin
### Punto 1
Desarrollar un algoritmo que imprima de manera ascendente los valores (todos del mismo tipo) de un diccionario.

```python
#Comenzamos creando mi diccionario, en este caso usamos mi edad, y la fecha del día de hoy.
diccionario = {
    'edad': 18,
    'mes_actual': 7,
    'dia_actual': 15,
    'año_actual': 2025
}

valores = diccionario.values()#Luego obtenemos los valores del diccionario

lista_valores = list(valores)#Ponemos estos valores en una lista

lista_valores.sort()#Ordenamos esta lista en orden ascendente

# Finalmente imprimimos los resultados
print("Valores ordenados ascendentemente:")
for valor in lista_valores:
    print(valor)

```
### Punto 2
Desarrollar una función que reciba dos diccionarios como parámetros y los mezcle, es decir, que se construya un nuevo diccionario con las llaves de los dos diccionarios; si hay una clave repetida en ambos diccionarios, se debe asignar el valor que tenga la clave en el primer diccionario.

```python
#Suponiendo que se reciben dos diccionarios:
def mezclar_diccionarios(dic1, dic2): 

    resultado = dict(dic2)
    resultado.update(dic1)
    return resultado#Devuelve un solo diccionario que combina sus llaves y valores, si hay claves repetidas utiliza el valor del primero diccionario
    
mezclado = mezclar_diccionarios(d1, d2)
print(mezclado) #Imprimimos el resultado
```
```python
#Suponiendo que el usuario debe ingresar los dos diccionarios:
def pedir_diccionario(nombre):

    d = {}
    print(f"Ingresar datos para el {nombre}:")
    n = int(input("Cantidad de elementos: "))
    for i in range(n):
        clave = input(f"Ingrese la clave {i+1}: ")
        valor = input(f"Ingrese el valor para la clave '{clave}': ")
        d[clave] = valor
    return d #Le pedimos al usuario las claves y los valores necesario para crear los dos diccionarios


#Utilizamos el mismo codigo que en el primero para mezclar los dos diccionarios ya creados y en caso de haber claves repetidas usar el valor del primer diccionario
dic1 = pedir_diccionario("primer diccionario")
dic2 = pedir_diccionario("segundo diccionario")

mezclado = mezclar_diccionarios(dic1, dic2)

print("Diccionario combinado:")
print(mezclado)#Imprimimos el resultadp
```
### Punto 3

Dado el JSON:
```json
{
  "jadiazcoronado":{
    "nombres": "Juan Antonio",
    "apellidos": "Diaz Coronado",
    "edad":19,
    "colombiano":true,
    "deportes":["Futbol","Ajedrez","Gimnasia"]
  },
  "dmlunasol":{
    "nombres": "Dorotea Maritza",
    "apellidos": "Luna Sol",
    "edad":25,
    "colombiano":false,
    "deportes":["Baloncesto","Ajedrez","Gimnasia"]
  }
}
```

Cree un programa que lea de un archivo con dicho JSON y:

Imprima los nombres completos (nombre y apellidos) de las personas que practican el deporte ingresado por el usuario.

Imprima los nombres completos (nombre y apellidos) de las personas que estén en un rango de edades dado por el usuario.

```python
#Suponiendo que guardamos el json como personas.json en mi computadora.
import json

def cargar_datos(nombre_archivo):

    with open(nombre_archivo, 'r', encoding='utf-8') as archivo:
        datos = json.load(archivo)
    return datos#Lee el archivo json y nos devuelve los datos de dicho archivo como diccionario

archivo_json = 'personas.json' #Leemos el archivo que está en mi computadora
datos = cargar_datos(archivo_json)

#Le solicitamos el deporte al usuario
deporte_usuario = input("Ingrese el deporte a buscar: ")
imprimir_por_deporte(datos, deporte_usuario)

#Le solicitamos el rango de edades al usuario
edad_min = int(input("\nIngrese edad mínima: "))
edad_max = int(input("Ingrese edad máxima: "))
imprimir_por_rango_edad(datos, edad_min, edad_max)

def imprimir_por_deporte(datos, deporte_buscado):
#Imprimimos los nombres completos de las personas que practican el deporte que el usuario asignó
    print(f"\nPersonas que practican '{deporte_buscado}':")
    encontrado = False
    for persona in datos.values():
        if deporte_buscado in persona['deportes']:
            print(f"- {persona['nombres']} {persona['apellidos']}")
            encontrado = True
    if not encontrado:
        print("Ninguna persona encontrada.") #En caso de que el usuario coloque un deporte que no está en el diccionario damos como mensaje "Ninguna persona encontrada"


def imprimir_por_rango_edad(datos, edad_min, edad_max):
#Hacemos básicamente lo mismo que en el anterior pero esta vez se imprimen los nombres en base a el rango de edad que nos de el usuario
    print(f"\nPersonas entre {edad_min} y {edad_max} años:")
    encontrado = False
    for persona in datos.values():
        if edad_min <= persona['edad'] <= edad_max:
            print(f"- {persona['nombres']} {persona['apellidos']}")
            encontrado = True
    if not encontrado:
        print("Ninguna persona encontrada en ese rango de edad.")#En caso de que el usuario coloque un rango de edad que no está en el diccionario damos como mensaje "Ninguna persona encontrada"
```
### Punto 4
A través de un programa conectese a al menos 3 API's , obtenga el JSON, imprimalo y extraiga los pares de llave : valor.

Primero necesitaremos tener instalada la librería "requests"

```python
import requests #Luego de instalar la libreria debemos importarla a nuestro código.

def get_json_and_extract(url): #Definimos una función que recibe una URL, obtiene el JSON y extrae las claves con sus valores.
    response = requests.get(url) #Utilizamos el get para hacer una solicitud HTTP a la url que ponemos acontinuanción
    if response.status_code == 200:#Comprobamos que la respuesta recibida sea exitosa
        data = response.json() #Convertimos esa misma respuesta en un diccionario o en una lista
        print(f"\nJSON recibido desde {url}:")
        print(data)
        print("\nPares clave:valor:")
        if isinstance(data, list):  # Si el JSON es una lista, toma el primer objeta para procesarlo
            data = data[0]
        for key, value in data.items():
            print(f"{key}: {value}")# Si es un diccionario lo recorre y muestra cada clave con su respectivo valo
    else:
        print(f"Error al conectar con {url}. Código de estado: {response.status_code}")#Si no puede concetarse con el url mostramos error.

# Llamamos y nos conectamos a tres APIs diferentes, luego extraemos sus datos
get_json_and_extract("https://api.coindesk.com/v1/bpi/currentprice.json")#Api que nos da el precio actual de bitcoin
get_json_and_extract("https://restcountries.com/v3.1/name/peru")#Api datos relevantes de países
get_json_and_extract("https://pokeapi.co/api/v2/pokemon/1")#Api de información sobre pokemon.
```

## Fin, Gracias.

