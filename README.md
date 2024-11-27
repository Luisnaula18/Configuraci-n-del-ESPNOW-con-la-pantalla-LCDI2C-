# Configuraci-n-del-ESPNOW-con-la-pantalla-LCDI2C-

import espnow
import time
import network
from machine import Pin, I2C
import lcd

# Configurar el ESP32 como estación Wi-Fi (aunque no se conectará a una red Wi-Fi)
wlan = network.WLAN(network.STA_IF)
wlan.active(True)

# Inicializar ESP-NOW
espnow.init()

# Dirección MAC del receptor (obtén la dirección MAC del receptor)
mac_receptor = b'\x30\xae\xa4\x12\x34\x56'  # Cambiar por la MAC real del receptor

# Inicializar el I2C para el LCD (conectar a la pantalla LCD en los pines correspondientes)
i2c = I2C(0, scl=Pin(22), sda=Pin(21), freq=400000)
lcd = lcd.LCD(i2c)

# Configurar el LCD
lcd.clear()

# Enviar datos
while True:
    mensaje = "Hola desde emisor !"
    espnow.send(mac_receptor, mensaje.encode())  # Enviar el mensaje al receptor
    print("Mensaje enviado:", mensaje)
    lcd.clear()
    lcd.putstr("Enviado: " + mensaje)
    time.sleep(2)  # Esperar 2 segundos antes de volver a enviar
