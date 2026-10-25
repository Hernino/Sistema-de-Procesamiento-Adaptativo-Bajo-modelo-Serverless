# Sistema de Procesamiento Adaptativo Bajo modelo Serverless

Este repositorio contiene el código fuente, configuraciones, scripts y registros .log del proyecto de la Práctica I (Facultad de Ingeniería y Ciencias, Universidad Diego Portales).

El proyecto implementa una arquitectura híbrida sobre **Apache Flink** y **AWS Lambda (emulado en LocalStack)** capaz de mitigar el *backpressure* en tiempo real ante ráfagas de tráfico mediante **bifurcación elástica (*Micro-Batching*)**, escalado dinámico de contenedores y fusión de estados parciales (*State Merge*).

---

## Arquitectura del Sistema

El entorno distribuido se divide en dos nodos conectados en red local (LAN):
1. **Nodo Principal PC (Clúster Base VM):** Ejecuta el clúster de Apache Flink (`JobManager` + `TaskManagers` en Docker) y el **Orquestador**, el cual monitorea el caudal por operador vía REST API y escala horizontalmente los contenedores.
2. **Nodo Secundario Notebook (Entorno Serverless):** Ejecuta **LocalStack** en Docker utilizando la función `BurstHandler` (AWS Lambda), encargada de absorber y pre-procesar los lotes excedentes durante el *cold-start* de escalado del clúster local (~25 s).

---

## Estructura del Repositorio

```text
├── README.md
├── Códigos PC Local Fase 1.rar     # Nodo Principal (PC) - Contiene todos los códigos y scripts utilizados para todas las pruebas realizadas en la Fase 1.
├── Códigos PC Local Fase 2.rar     # Nodo Principal (PC) - Contiene todos los códigos y scripts utilizados para las dos pruebas realizadas en la Fase 2.
├── Códigos Notebook (Serverless).rar   # Nodo Secundario (Notebook) - Contiene todos los códigos y scripts utilizados para el procesamiento en Serverless para ambas Fases del proyecto.
├── Imagenes PC Fase 1 (Local).rar  # Contiene imágenes como evidencias del correcto funcionamiento de las pruebas realizadas en la Fase 1 (muestra las distintas terminales y el Dashboar de Flink).
├── Imágenes PC Fase 2 (Local).rar  # Contiene imágenes como evidencias del correcto funcionamiento de las pruebas realizadas en la Fase 2 (muestra las distintas terminales y el Dashboar de Flink mostrando la correcta separación de operadores por slot).
├── Logs PC Fase 1 (Local).rar      # Contiene todos los resultados en crudo en archivos .log totalmente detallados obtenidos en el PC Local durante la Fase 1 (muestra el comportamiento completo del sistema durante las pruebas realizadas).
├── Logs PC Fase 2 (Local).rar      # Contiene todos los resultados en crudo en archivos .log totalmente detallados obtenidos en el PC Local durante la Fase 2 (muestra el comportamiento completo del sistema durante las pruebas realizadas).
├── Logs Notebook Fase 1 (Serverless).rar   # Contiene todos los resultados en crudo en archivos .log totalmente detallados obtenidos en el PC Local durante la Fase 1 (estos logs fueron adjuntados solo para demostrar el correcto y real funcionamiento de LocalStack, no aportan datos importantes para análisis).
└── Diagramas                       # Contiene imágenes con los diagramas de trabajo de cada Fase.

