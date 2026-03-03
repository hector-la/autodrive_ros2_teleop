# AutoDRIVE F1TENTH - ROS 2 Humble Setup (ESPOL) 🏎️

Este repositorio proporciona una guía detallada para instalar el simulador AutoDriver y configurar el puente de comunicación entre el **AutoDRIVE Simulator** y **ROS 2 Humble**. El objetivo es permitir el control y la adquisición de datos del vehículo **F1TENTH** para proyectos de robótica y navegación autónoma.

---

## 📋 Requisitos Previos

Antes de comenzar, asegúrate de contar con el siguiente entorno (probado en **ASUS TUF Gaming A15**):

* **Sistema Operativo:** Ubuntu 22.04 
* **Middleware:** ROS 2 Humble Hawksbill.

---

## 🕹️ 1. Instalación del Simulador

Sigue estos pasos para preparar el entorno de simulación:

1. **Descarga** el binario(archivo) del simulador para Linux.
   
3. **Descomprime** el archivo en una carpeta de fácil acceso (ej. `~/Downloads/AutoDRIVE_Sim`).
   
5. **Otorga permisos de ejecución** al binario principal abriendo una terminal en dicha carpeta:
   ```bash
   chmod +x "AutoDRIVE Simulator.x86_64"


