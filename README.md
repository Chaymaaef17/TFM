# Sistema de navegación autónoma semántica para robot móvil en entornos interiores dinámicos

Trabajo Fin de Máster · Máster en Robótica y Automatización · Universidad Europea de Madrid, Escuela STEAM
Autora: Chaymae Aouraghe El Founti · Director: D. Daniel Vicente Rodrigo Muñoz

Un robot recibe una orden escrita como «quiero ir a la cafetería», interpreta el destino sobre un catálogo
semántico del edificio, elige la ruta teniendo en cuenta a las personas, la ejecuta con Nav2 y verifica la
llegada. La arquitectura se valida en simulación con TurtleBot3 Waffle y se transfiere a un modelo de Spot.

## Paquetes (`src/`)

| Paquete | Contenido |
| --- | --- |
| `tfm_university_gazebo` | Universidad de 44 × 32 m modelada en Blender e integrada en Gazebo, con TurtleBot3 Waffle |
| `tfm_university_slam` | Cartografía con SLAM Toolbox, misión de cobertura y evaluación del mapa |
| `tfm_university_nav2` | Localización con AMCL y navegación con Nav2 (SmacPlanner2D + Regulated Pure Pursuit) |
| `tfm_university_perception` | Detección de personas con cámara (YOLO) y LiDAR, y peatones simulados |
| `tfm_university_social_nav` | Supervisor social, capa social del mapa de costes y selección de ruta |
| `tfm_university_iot` | Telemetría ROS 2 → MQTT → SQLite → InfluxDB → Grafana |
| `tfm_university_prediction` | Campaña de datos y predicción de ocupación (Random Forest) integrada en la decisión de ruta |
| `tfm_university_semantic` | Catálogo semántico, intérprete de órdenes por texto, detector de objetos, OCR e interfaz web |
| `tfm_university_spot` | Transferencia a Spot: locomoción, odometría, localización, navegación y capa semántica |
| `tfm_spot_trial`, `spot_tfm` | Modelo de Spot y pruebas iniciales de integración |

Cada paquete tiene su `README.md` con los comandos de uso y un `INFORME_*.md` con los resultados de su fase.
Las evidencias de cada ensayo están en la carpeta `validation/` de cada paquete.

## Entorno

ROS 2 Jazzy y Gazebo Harmonic, en el contenedor Docker `ros2_lab_container`, con el workspace en `/root/ros2_ws`.
Todo se ejecuta en CPU; las métricas usan el reloj simulado.

```bash
docker exec -it ros2_lab_container bash
source /opt/ros/jazzy/setup.bash
cd /root/ros2_ws && colcon build && source install/setup.bash
```

Demo semántica completa (universidad, TurtleBot3, Nav2, percepción, supervisor social e interfaz web en
http://localhost:8088):

```bash
bash src/tfm_university_semantic/scripts/run_cached.sh gui:=true rviz:=true
```

## Notas

- Las grabaciones ROS (`*.mcap`, `*.db3`) no se incluyen en el repositorio por su tamaño; las métricas y
  los informes derivados de ellas sí.
- `spot_ros2` es software de terceros y se instala aparte (ver `tfm_university_spot/README.md`).
- Todos los resultados son en simulación, sin hardware ni personas reales.
