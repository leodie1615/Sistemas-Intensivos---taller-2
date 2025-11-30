# Sistemas-Intensivos---taller-2
# Integrantes:

- 2222222222 Lina Ojeada
- 202512491 Ivan Suarez
- 202525657 Diego Baron

# Instalaciones
Acontinuación se detalla el paso a paso de ejecucion del taller:

Etapa Nombre del notebook
Bronze job-bronze-embarcaciones
Silver job-silver-embarcaciones
Gold job-gold-embarcaciones

Los anteriores flujos estan orquestados bajo el siguiente flujo YAML:

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
  
# Objetivo del taller:

• Resolver un requerimiento de negocio basado en dos datasets relacionados disponibles de forma abierta
• Utilizar la plataforma de Databricks para procesar los datos y presentar los resultados.
• Crear un proceso escalable que permita utilizar la infraestructura de manera eficiente

# Instrucciones Ejecución:

- El Pipeline orquestado, se ejecuta cada vez que existe un nuevo archivo en una ruta de staging dentro Databricks. Una vez se desencadena la ejecución, ingesta, transforma y enriquece las fuentes de datos NOAA y AIS con el fin de crear una tabla transformada que pueda ser soporte para un Dashboard.

# Conclusiones:

- Las recomendaciones finales y los insights se encuentran en el notebook taller2_final.
  Nota: El informe ejecutivo se encuentra en el repositorio bajo el nombre: Resumen Ejecutivo Taller 2.pdf

