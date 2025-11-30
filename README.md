# Sistemas Intensivos en Datos - Taller 2: Enriquecimiento de datos con fuentes abiertas
# Integrantes:

- 2222222222 Lina Ojeada
- 202512491 Ivan Suarez
- 202525657 Diego Baron

# Instalaciones
- Arquitectura medallón: A continuación se detalla el paso a paso de ejecucion del taller:

| Etapa | Nombre del notebook            |
|-------|--------------------------------|
| Bronze | job-bronze-embarcaciones |
| Silver | job-silver-embarcaciones |
| Gold   | job-gold-embarcaciones   |

- Orquestación: Los flujos anteriores están orquestados, y la ETL puede consultarse en el siguiente enlace: [ETL](https://dbc-2d8353f4-c1b7.cloud.databricks.com/jobs/1115246332897841/tasks?o=2778813972398238). Asimismo, la orquestación está definida según los siguientes pasos en el flujo YAML:


resources:
    

    jobs:
    ETL_embarcaciones:
      name: ETL-embarcaciones
      trigger:
        pause_status: PAUSED
        file_arrival:
          url: /Volumes/proyecto/default/raw_data/marine-cadastre/
      tasks:
        - task_key: embarcaciones-bronze
          notebook_task:
            notebook_path: /Workspace/Users/odl_user_1905298@databrickslabs.com/job-bronze-embarcaciones
            source: WORKSPACE
          existing_cluster_id: 1006-120337-9shwlkkm
        - task_key: silver-embarcaciones
          depends_on:
            - task_key: embarcaciones-bronze
          notebook_task:
            notebook_path: /Workspace/Users/odl_user_1905298@databrickslabs.com/job-silver-embarcaciones
            source: WORKSPACE
          existing_cluster_id: 1006-120337-9shwlkkm

        - task_key: gold-embarcaciones
          depends_on:
            - task_key: silver-embarcaciones
          notebook_task:
            notebook_path: /Workspace/Users/odl_user_1905298@databrickslabs.com/job-gold-embarcaciones
            source: WORKSPACE
          existing_cluster_id: 1006-120337-9shwlkkm

        - task_key: Embarcaciones
          depends_on:
            - task_key: gold-embarcaciones
          dashboard_task:
            subscription: {}
            dashboard_id: 01f0cd737b7f1b45b75fc380c5bf51c1

      queue:
        enabled: true

- Dashboard: ##############################
  

- Finalmente, se adjunta presentacion junto con los correspondientes notebooks de la arquitectura medallón.
  
# Objetivo del taller:

- Resolver un requerimiento de negocio basado en dos datasets relacionados disponibles de forma abierta
- Utilizar la plataforma de Databricks para procesar los datos y presentar los resultados.
- Crear un proceso escalable que permita utilizar la infraestructura de manera eficiente

# Instrucciones Ejecución:

- El pipeline orquestado se ejecuta automáticamente cada vez que se detecta un nuevo archivo en la ruta de staging dentro de Databricks. Una vez se activa la ejecución, el proceso ingesta, transforma y enriquece las fuentes de datos NOAA y AIS, con el fin de generar una tabla transformada que sirve como insumo para un dashboard donde se visualizan el grid, las métricas de profundidad y las características de las embarcaciones que realizaron los registros.



