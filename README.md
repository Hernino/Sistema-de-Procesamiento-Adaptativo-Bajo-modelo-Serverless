# Sistema de Procesamiento Adaptativo Bajo modelo Serverless

Este repositorio contiene el código fuente, configuraciones, scripts y registros .log del proyecto de la Práctica I (Facultad de Ingeniería y Ciencias, Universidad Diego Portales).

El proyecto implementa una arquitectura híbrida sobre **Apache Flink** y **AWS Lambda (emulado en LocalStack)** capaz de mitigar el *backpressure* en tiempo real ante ráfagas de tráfico mediante **bifurcación elástica (*Micro-Batching*)**, escalado dinámico de contenedores y fusión de estados parciales (*State Merge*).

---

## Arquitectura del Sistema

El entorno distribuido se divide en dos nodos conectados en red local (LAN):
1. **Nodo Principal (Clúster Base VM):** Ejecuta el clúster de Apache Flink (`JobManager` + `TaskManagers` dinámicos en Docker) y el **Orquestador Reactivo**, el cual monitorea el caudal por operador vía REST API y escala horizontalmente los contenedores.
2. **Nodo Secundario (Entorno Serverless):** Ejecuta **LocalStack** en Docker exponiendo la función `BurstHandler` (AWS Lambda), encargada de absorber y pre-procesar los lotes excedentes durante el *cold-start* de escalado del clúster local (~25 s).

---

## Estructura del Repositorio

> *(Completa o ajusta los nombres de tus archivos en la lista inferior según cómo los tengas nombrados en tus carpetas)*

```text
├── README.md
├── cluster-flink/                  # Nodo Principal (PC) - Clúster Base y Orquestador
│   ├── docker-compose.yml          # Definición de JobManager y TaskManagers (Flink 1.18)
│   ├── Dockerfile                  # Imagen personalizada con PyFlink y dependencias
│   ├── orquestador.py              # [ESPACIO: Nombre de tu script del Orquestador Reactivo]
│   └── jobs/
│       ├── fase1_poc/              # Scripts de la Prueba de Concepto (Fase 1)
│       │   ├── [ESPACIO: ej. job_baseline.py]
│       │   └── [ESPACIO: ej. job_serverless_hibrido.py]
│       └── fase2_nexmark/          # Consultas del Benchmark Estándar NexMark (Fase 2)
│           ├── [ESPACIO: ej. nexmark_q1_stateless.py]
│           └── [ESPACIO: ej. nexmark_q4_stateful.py]
│
├── serverless-node/                # Nodo Secundario (Notebook) - AWS Lambda en LocalStack
│   ├── docker-compose.yml          # Configuración del contenedor LocalStack y Docker Socket
│   ├── lambda_function.py          # Lógica del BurstHandler (Stateless / Stateful Micro-batch)
│   └── deploy.sh                   # Script de empaquetado (.zip) y registro automático en Lambda
│
├── logs-experimentales/            # Registros crudos (.log) de las pruebas de 4 minutos
│   ├── fase1_poc/
│   │   ├── Resultado Baseline (1).log
│   │   ├── Resultado Baseline Carga Extrema.log
│   │   ├── Resultado Basaline Intermitente.log
│   │   ├── Resultado Serverless Carga Extrema.log
│   │   ├── Resultado Serverless Intermitente.log
│   │   ├── Resultado Serverless Tolerancia a Fallos.log
│   │   └── Resultado Test Serverless (1).log
│   └── fase2_nexmark/
│       ├── test_nexmark_q1_stateless.log
│       ├── test_nexmark_q1_stateless_bifurcado.log
│       ├── Resultado_NexMark_Q1_Bifurcado_Final.log
│       └── Resultado_NexMark_Q4_Stateful_Final.log
│
└── analisis-y-graficos/            # Scripts de evaluación estadística y figuras del reporte
    ├── [ESPACIO: ej. generar_graficos.py]
    ├── grafico_fase1_comparativa_log.png
    ├── grafico_percentiles_comparativa.png
    └── grafico_fase2_nexmark_q1_q4.png
