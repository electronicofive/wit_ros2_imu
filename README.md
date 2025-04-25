# WitImu ROS2 Package

Este repositorio contiene instrucciones y código para integrar un módulo IMU de diez ejes en ROS2. Las siguientes instrucciones están basadas en Ubuntu 20.04 con ROS2 Foxy.

## Requisitos previos

- Ubuntu 20.04
- ROS2 Foxy
- Módulo IMU de diez ejes (conectado por USB)
- El paquete utiliza un espacio de trabajo llamado "WitImu_ws"
- La velocidad de comunicación predeterminada es 9600 baudios

## Instalación

### 1. Compilar el paquete

Cree un espacio de trabajo y clone este repositorio:

```bash
mkdir -p WitImu_ws/src
cd WitImu_ws/src
# Clone el repositorio o descomprima el archivo wit_ros2_imu aquí
# git clone https://github.com/tu-usuario/wit_ros2_imu.git
```

Compile el paquete:

```bash
cd ~/WitImu_ws
colcon build
```

Añada el espacio de trabajo a su archivo `.bashrc`:

```bash
echo "source ~/WitImu_ws/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

### 2. Configurar el puerto serie

Para evitar problemas de identificación cuando se conectan múltiples dispositivos USB, vincule el puerto serial del módulo IMU a `/dev/imu_usb`:

```bash
cd ~/WitImu_ws/src/wit_ros_imu
sudo chmod 777 bind_usb.sh
sudo sh bind_usb.sh
```

Desconecte y vuelva a conectar el cable USB del módulo IMU. Verifique que la vinculación del puerto se ha realizado correctamente:

```bash
ll /dev/imu_usb
```

> **Nota**: El dispositivo no tiene que ser necesariamente `ttyUSB0`, lo importante es que se muestre como un dispositivo USB.

## Uso

### Ejecutar el nodo y visualizar en RViz

```bash
ros2 launch wit_ros2_imu rviz_and_imu.launch.py
```

### Ver los datos publicados

```bash
ros2 topic echo /imu/data
```

## Configuración avanzada

### Modificar la velocidad de baudios

Por defecto, el programa utiliza una velocidad de 9600 baudios. Si ha modificado la velocidad en el software del ordenador principal, necesitará actualizar también el código fuente.

El archivo que necesita modificar es:
`~/WitImu_ws/src/wit_ros2_imu/wit_ros2_imu/wit_ros2_imu.py`

Busque la línea 149 aproximadamente:

```python
def driver_loop(self, port_name):
    # open serial port
    try:
        wt_imu = serial.Serial(port="/dev/imu_usb", baudrate=9600, timeout=0.5)
```

Cambie `9600` por la velocidad de baudios que haya configurado. Después, guarde el archivo, salga y recompile el paquete:

```bash
cd ~/WitImu_ws
colcon build
```

## Contribuir

Las contribuciones son bienvenidas. Por favor, abra un issue para discutir los cambios que desea realizar.

## Licencia

[Incluir información de licencia aquí]
