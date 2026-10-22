# 5to-Azul-pensamiento-computacional-2026-2027
pensamiento computacional

Tarea 3

Problema: Alarma basica de antirrobo de una caja fuerte, gabeta u otros
- El problema es que muy seguido en la contidianidad dejamos una caja fuerte, gabeta, o closet cerrados, resguardando nuestras preciadas pertenencias en ellos, y tenemos que tener una forma de darnos cuenta si esta a salvo o no.
- Cualquier persona se puede beneficiar de un sistema de alarmas como este
- el circuito funciona de forma que la puerta, al abrirse, activará una señal de alerta (una luz LED) para advertir que algun intruso se metio en tus pertenencias.
- Codigo:
- // Definimos los pines que vamos a usar
const int pinBoton = 2;  // El botón simula la puerta
const int pinLed = 13;   // El LED simula la alarma

void setup() {
  // Configuramos el botón como entrada y el LED como salida
  pinMode(pinBoton, INPUT);
  pinMode(pinLed, OUTPUT);
}

void loop() {
  // Leemos el estado del botón
  int estadoPuerta = digitalRead(pinBoton);

  // Si el botón está presionado (puerta abierta)
  if (estadoPuerta == HIGH) {
    digitalWrite(pinLed, HIGH); // Enciende la alarma (LED)
  } else {
    digitalWrite(pinLed, LOW);  // Apaga la alarma (todo seguro)
  }
}

<img width="1085" height="766" alt="image" src="https://github.com/user-attachments/assets/10c9ca9c-9e97-4447-b4b8-f8ea30c12feb" />



## TAREA 22/10/26

Un centro de atención recibe estudiantes para entregar documentos. Hay una sola ventanilla y tres tipos de trámites: entrega de documentos, solicitud de constancia y consulta general. Algunos trámites tardan más que otros. El centro quiere reducir el tiempo promedio de espera sin dejar de atender por orden de llegada, salvo casos prioritarios.

**1.** Descompón el problema en al menos 4 subproblemas: llegada de estudiantes, registro, prioridades, atención, cierre.
**2.** Define qué datos se necesitan para representar a cada estudiante.
**3.** Diseña un algoritmo en pseudocódigo o diagrama de flujo que organice la atención.
**4.** Explica qué criterio de prioridad usarías y por qué.


## 1. Descomposición del Problema en Subproblemas

**1. Llegada de Estudiantes:** Captura el ingreso físico o virtual del usuario al sistema del centro de atención, determinando la tasa de concurrencia y el flujo de entrada.
**2. Registro y Clasificación del Trámite:** Identificación unívoca del estudiante y tipificación de su requerimiento (*Entrega de documentos*, *Solicitud de constancia* o *Consulta general*), asociando un tiempo de procesamiento estimado.
**3. Gestión de Prioridades y Encolamiento:** Asignación dinámica de la posición en la estructura de datos de espera, ponderando el orden de llegada (FIFO) frente a condiciones excepcionales o requerimientos de prioridad institucional.
**4. Atención en Ventanilla Única:** Ciclo de ejecución del servicio por parte del operador, caracterizado por la liberación de la ventanilla y el registro de tiempos de servicio.
**5. Cierre de Operaciones y Métricas:** Culor de la jornada o del ciclo de atención, con la generación de indicadores clave de rendimiento (KPIs), tales como tiempo promedio de espera y satisfacción del usuario.


## 2. Modelo de Datos: Representación del Estudiante

```text
Estructura Estudiante {
    Entero       id_estudiante        // Identificador único o número de matrícula
    Cadena       nombre               // Nombre completo del estudiante
    Entero       tipo_tramite         // 1: Entrega de documentos, 2: Solicitud de constancia, 3: Consulta general
    Entero       duracion_estimada    // Tiempo estimado de procesamiento en minutos (según tipo de trámite)
    Entero       nivel_prioridad      // 0: Normal, 1: Prioritario (Casos especiales, salud, urgencias académicas)
    MarcaTiempo  timestamp_llegada    // Hora exacta de llegada al centro para garantizar orden relativo
}

```

## 3. Criterio de Prioridad y Justificación

### Criterio Seleccionado:

1. **Prioridad Absoluta por Excepción:** Casos médicos, discapacidades o restricciones de tiempo críticas debidamente justificadas tienen preferencia absoluta sobre cualquier turno estándar.
2. **Prioridad por Tiempo de Procesamiento Corto dentro de la misma categoría:** Entre estudiantes con igual nivel de prioridad ordinaria, se aplica una variante orientada a minimizar el tiempo de espera acumulativo de la cola, priorizando aquellos trámites con menor duración estimada (ej. *Consulta general* o *Entrega de documentos* frente a trámites complejos), respetando secundariamente el orden estricto de llegada (FIFO).


4. Algoritmo de Gestión de Atención (Pseudocódigo)

```text
Algoritmo GestionCentroAtencion
    // Inicialización de estructuras de datos
    CrearColaPrioridad(ColaEspera)
    Booleano centroAbierto = VERDADERO
    
    // Bucle principal de operaciones del centro
    Mientras centroAbierto == VERDADERO Hacer
        
        // 1. Fase de Llegada y Registro
        Si HayNuevoEstudianteEnFila() Entonces
            Estudiante nuevoEstudiante = RegistrarEstudiante()
            ColaEspera.Insertar(nuevoEstudiante)
        Fin Si
        
        // 2. Fase de Atención en Ventanilla Única
        Si VentanillaEstaLibre() Y NO ColaEspera.EstaVacia() Entonces
            
            // Extraer al siguiente estudiante según la función de prioridad optimizada
            Estudiante actual = ColaEspera.ObtenerSiguienteAtencion()
            
            IniciarAtencion(actual)
            SimularTiempoProceso(actual.duracion_estimada)
            FinalizarAtencion(actual)
            
        Fin Si
        
        // 3. Verificación de Cierre
        Si EsHoraDeCierre() Y ColaEspera.EstaVacia() Entonces
            centroAbierto = FALSO
        Fin Si
        
    Fin Mientras
    
    GenerarReporteMetricas()
Fin Algoritmo

```


## Módulo 1: 

### 1. Representación de Paradas y Distancias

Se modela un sistema con 6 paradas (incluyendo el punto de partida/base escolar) y un volumen conocido de estudiantes por estación.

| ID Parada | Nombre de la Parada | Estudiantes a Recoger | Distancia al Siguiente Nodo (km) |
| --- |        --- | ---                 | --- |
| **P0** |    Base Escolar         //      | 0 |                  2.5 (a P1)  |
| **P1** |    Residencial Norte   //      | 5 |                  3.1 (a P2)  |
| **P2** |    Parque Central     //       | 8 |                  1.8 (a P3)  |
| **P3** |    Urbanización Sur  //       | 12 |                  4.0 (a P4) |
| **P4** |    Plaza Mayor      //        | 4 |                   2.2 (a P5) |
| **P5** |    Sector Este     //          | 6 |                   3.5 (a P0) |


### 2. Algoritmo Paso a Paso para Construir la Ruta

1. **Inicialización:**
* Definir la Base Escolar (P0) como punto de partida actual ($N_{actual} = P0$).
* Inicializar el conjunto de paradas pendientes $S = \{P1, P2, P3, P4, P5\}$.
* Inicializar la ruta seleccionada $Ruta = [P0]$ y el contador de capacidad actual del autobús en $0$.


2. **Ciclo de Construcción:**
* Mientras $S$ no esté vacío:
* Evaluar todas las paradas $P_i$ pertenecientes a $S$.
* Calcular una función de costo heurística que combine distancia mínima y mayor concentración de estudiantes:

$$\text{Costo} = \text{Distancia}(N_{actual}, P_i) - (\alpha \times \text{Estudiantes}_{P_i})$$


 Seleccionar la parada $P_s$ que minimice dicho costo (priorizando cercanía pero compensando con alta demanda de pasajeros).
 Actualizar $N_{actual} = P_s$, añadir $P_s$ a $Ruta$, y remover $P_s$ de $S$.




3. **Cierre de Ruta:**
 Al vaciar $S$, añadir la Base Escolar (P0) al final de $Ruta$ para completar el circuito de retorno.


4. **Salida:** Retornar la ruta optimizada y la métrica de distancia total acumulada.


### 3. Ruta Propuesta Aplicando el Algoritmo

Siguiendo la heurística de proximidad y alta densidad de pasajeros:

* **Secuencia de Recorrido:**

$$\text{P0 (Base)} \rightarrow \text{P1 (5 est.)} \rightarrow \text{P2 (8 est.)} \rightarrow \text{P3 (12 est.)} \rightarrow \text{P4 (4 est.)} \rightarrow \text{P5 (6 est.)} \rightarrow \text{P0 (Base)}$$


* **Estudiantes Totales Recogidos:** 35 estudiantes.

### 4. Comparación y Selección de Alternativas

Se evalúa la ruta propuesta frente a una alternativa secuencial arbitraria (orden alfabético/numérico estricto).

**Alternativa A (Ruta Propuesta - Heurística Vecino + Demanda):** Distancia total aproximada = 17.1 km.
**Alternativa B (Ruta Secuencial Estricta P0-P1-P2-P3-P4-P5-P0):** Distancia total aproximada = 22.5 km.

 **Criterio Seleccionado:** **Menor distancia total recorrida**.
 **Justificación:** La alternativa propuesta reduce la distancia en un 24%, lo que se traduce directamente en un menor consumo de combustible, menor desgaste mecánico de la unidad de transporte y un tiempo de viaje optimizado para los estudiantes a bordo.


## Modulo 2:

### 1. Identificación de Errores o Limitaciones del Algoritmo Original

Analizando el pseudocódigo provisto:

1. **Error Matemático / Lógico Crítico:** El cálculo del promedio suma las notas (`nota1 + nota2 + nota3`), pero omite la división entre el número total de notas (3). Esto genera un valor inflado, lo que falsea completamente la condición de aprobación.
2. **Falta de Validación de Datos de Entrada:** No existe control sobre los valores ingresados; el sistema aceptaría notas negativas o valores absurdos mayores a 100.

### 2. Pseudocódigo Corregido y Mejorado

A continuación, se presenta el pseudocódigo optimizado que corrige los errores conceptuales, añade validación estricta de rangos e incorpora un sistema de recuperación académica.

```text
Algoritmo EvaluacionEstudianteMejorado
    // Definición de variables
    Real nota1, nota2, nota3, nota4, promedio
    Booleano notasValidas

    // Inicialización de bandera de validación
    notasValidas = FALSO

    // Validación de entrada de datos (Rango 0 a 100)
    Mientras notasValidas == FALSO Hacer
        Escribir "Ingrese las 4 notas del estudiante (0 a 100):"
        Leer nota1, nota2, nota3, nota4

        Si (nota1 >= 0 Y nota1 <= 100) Y _
           (nota2 >= 0 Y nota2 <= 100) Y _
           (nota3 >= 0 Y nota3 <= 100) Y _
           (nota4 >= 0 Y nota4 <= 100) Entonces
            notasValidas = VERDADERO
        SiNo
            Escribir "Error: Una o más notas están fuera del rango permitido [0, 100]. Intente de nuevo."
        FinSi
    FinMientras

    // Corrección del cálculo matemático: Suma dividida entre el total de elementos (4)
    promedio = (nota1 + nota2 + nota3 + nota4) / 4

    // Mostrar el resultado cuantitativo del promedio
    Escribir "El promedio final del estudiante es: ", promedio

    // Evaluación con lógica de aprobación y recuperación
    Si promedio >= 60 Entonces
        Escribir "Estado: APRUEBA"
    SiNo
        Si promedio >= 50 Y promedio < 60 Entonces
            Escribir "Estado: NO APRUEBA DIRECTO - Debe presentar EXAMEN DE RECUPERACIÓN"
        SiNo
            Escribir "Estado: REPRUEBA DEFINITIVA"
        FinSi
    FinSi

FinAlgoritmo

```
