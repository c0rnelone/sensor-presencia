# Sensor de presencia mmWave · HLK-LD2410C + nanoESP32-C6

*[English version below](#english)*

Sensor de presencia humana para Home Assistant basado en el radar mmWave **HLK-LD2410C** soldado directamente sobre una **nanoESP32-C6**, con firmware **ESPHome** y caja impresa en 3D (con soportes de pared opcionales).

<p align="center">
  <img src="docs/img/caja_abierta.jpg" width="45%">
  <img src="docs/img/caja_cerrada.jpg" width="45%">
</p>

## Hardware

| Componente | Notas |
|---|---|
| nanoESP32-C6 | Placa con dos USB-C (serigrafía «nanoESP32-C6 V1.0») |
| HLK-LD2410C | Radar mmWave de 24 GHz |
| Cable USB-C + alimentador 5 V | |

### Conexión

El radar va soldado directamente sobre los pines del ESP, sin cables:

| LD2410C | nanoESP32-C6 | Uso |
|---|---|---|
| VCC | 5V | Alimentación |
| GND | GND | |
| OUT | GPIO9 | No se usa (GPIO9 es pin de arranque del C6) |
| RX | GPIO18 | TX del ESP |
| TX | GPIO19 | RX del ESP |

Toda la información llega por UART a 256000 baudios.

<p align="center"><img src="docs/img/conexion_radar_esp.jpg" width="40%"></p>

## Firmware (ESPHome)

Hay dos configuraciones:

| Archivo | Conexión con Home Assistant |
|---|---|
| [`sensor-presencia.yaml`](sensor-presencia.yaml) | **Zigbee** (Zigbee2MQTT), sin WiFi |
| [`sensor-presencia-wifi.yaml`](sensor-presencia-wifi.yaml) | **WiFi** + API nativa de ESPHome |

**Versión Zigbee.** El ESP32-C6 funciona como dispositivo final Zigbee. Por Zigbee solo viajan entidades `binary_sensor`, `sensor`, `switch` y `number`, así que los `select` y `button` del radar quedan internos y se manejan con interruptores y números «puente». Los nombres van sin tildes porque Zigbee solo admite ASCII.

**Versión WiFi.** Copia `secrets.yaml.example` como `secrets.yaml` y rellena tus datos.

Para compilar y flashear por USB:

```bash
esphome run sensor-presencia.yaml --device COMx
```

### Entidades principales

- Presencia (ocupación), movimiento y presencia estática
- Distancia de detección, distancias y energías de movimiento y estáticas
- Luz (solo con el modo ingeniería activado)
- Ajustes del radar: tiempo de espera, puertas máximas, umbrales por puerta (G0–G8), resolución, función de luz, Bluetooth y modo ingeniería
- Reinicio y reset de fábrica del radar, lectura de parámetros, reinicio del ESP

## Caja impresa en 3D

Tres versiones, diseñadas para impresión FDM sin soportes (Bambu Lab X1C, boquilla 0,4 mm, ABS):

| Versión | | Descarga |
|---|---|---|
| **Caja compacta** (32 × 57 × 16 mm). Tapa con clic y placa sujeta con clips. | <img src="docs/img/caja_despiece.jpg" width="200"> | MakerWorld *(enlace pendiente)* |
| **Soporte de pared de doble bisagra.** Giro e inclinación, bloqueo con 2 × M3. | <img src="docs/img/soporte_bisagra.jpg" width="200"> | MakerWorld *(enlace pendiente)* |
| **Soporte de pared de rótula.** ±35° y giro de 360°, bloqueo con 1 × M3. | <img src="docs/img/soporte_rotula.jpg" width="200"> | MakerWorld *(enlace pendiente)* |

Los archivos para imprimir (STL) solo se publican en MakerWorld.

## Licencia

Firmware y documentación bajo licencia [MIT](LICENSE). Los modelos 3D se rigen por la licencia indicada en MakerWorld.

---

<a name="english"></a>
# mmWave presence sensor · HLK-LD2410C + nanoESP32-C6

Human presence sensor for Home Assistant built from an **HLK-LD2410C** mmWave radar soldered straight onto a **nanoESP32-C6**, running **ESPHome**, with a 3D-printed enclosure (and optional wall mounts).

## Hardware

- nanoESP32-C6 (dual USB-C board, silkscreen "nanoESP32-C6 V1.0")
- HLK-LD2410C 24 GHz mmWave radar
- USB-C cable + 5 V power supply

### Wiring

The radar is soldered directly onto the ESP header, no wires:

| LD2410C | nanoESP32-C6 | Use |
|---|---|---|
| VCC | 5V | Power |
| GND | GND | |
| OUT | GPIO9 | Not used (GPIO9 is a C6 strapping pin) |
| RX | GPIO18 | ESP TX |
| TX | GPIO19 | ESP RX |

All data comes over UART at 256000 baud.

## Firmware (ESPHome)

| File | Link to Home Assistant |
|---|---|
| [`sensor-presencia.yaml`](sensor-presencia.yaml) | **Zigbee** (Zigbee2MQTT), no WiFi |
| [`sensor-presencia-wifi.yaml`](sensor-presencia-wifi.yaml) | **WiFi** + native ESPHome API |

**Zigbee version.** The ESP32-C6 runs as a Zigbee end device. Only `binary_sensor`, `sensor`, `switch` and `number` entities go over Zigbee, so the radar's `select` and `button` entities are internal and driven through "bridge" switches and numbers. Entity names are ASCII-only because Zigbee requires it.

**WiFi version.** Copy `secrets.yaml.example` to `secrets.yaml` and fill in your values.

Flash over USB:

```bash
esphome run sensor-presencia.yaml --device COMx
```

Entity names are in Spanish.

## 3D-printed enclosure

Three versions, designed for support-free FDM printing (Bambu Lab X1C, 0.4 mm nozzle, ABS):

- **Compact enclosure** (32 × 57 × 16 mm): snap-fit lid, clip-held PCB.
- **Dual-hinge wall mount**: pan and tilt, locks with 2 × M3.
- **Ball-joint wall mount**: ±35° plus 360° rotation, locks with 1 × M3.

Printable files (STL) are published on MakerWorld only *(links coming soon)*.

## License

Firmware and documentation under the [MIT](LICENSE) license. 3D models are licensed as stated on MakerWorld.
