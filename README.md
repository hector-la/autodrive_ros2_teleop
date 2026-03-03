# AutoDRIVE - ROS 2 Humble (AIROS) 🏎️

Este repositorio proporciona una guía detallada para instalar el simulador AutoDriver y configurar el puente de comunicación entre el **AutoDRIVE Simulator** y **ROS 2 Humble**. El objetivo es permitir el control y la adquisición de datos del vehículo **F1TENTH** para proyectos de robótica y navegación autónoma.

---

## 📋 Requisitos Previos

Antes de comenzar, asegúrate de contar con el siguiente entorno (probado en **ASUS TUF Gaming A15**):

* **Sistema Operativo:** Ubuntu 22.04 
* **Middleware:** ROS 2 Humble Hawksbill.

---

## 🕹️ 1. Instalación del Simulador

Sigue estos pasos para preparar el entorno de simulación:

1. **Descarga** el archivo comprimido (.zip o .tar.gz) del simulador para Linux.
   
2. **Guarda** el archivo en un lugar de fácil acceso y **descomprímelo** (ej. `~/Downloads/AutoDRIVE_Sim`).

3. **Accede a la terminal** y dirígete a la carpeta donde descomprimiste el simulador. Ejemplo:
   ```bash
   cd ~/Downloads/AutoDRIVE_Sim
4. Otorga permisos de ejecución al binario principal:
      ```bash
   chmod +x "AutoDRIVE Simulator.x86_64"
6. Inicia el simulador ejecutando el siguiente comando en la terminal:
     ```bash
   ./"AutoDRIVE Simulator.x86_64"

