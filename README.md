# numero1

import time
import pandas as pd
from geopy.geocoders import Nominatim

# Cargar el archivo de Excel
file_path = "C:\Users\atrig\Documents\progGithub\numero1\direcciones columnas caba.xlsx"
df = pd.read_excel(file_path, sheet_name="Cabacol")

# Inicializar el geocodificador (es gratuito y usa OpenStreetMap)
geolocator = Nominatim(user_agent="mi_conversor_caba")

# Recorrer las filas y obtener la dirección
direcciones = []
for index, row in df.iterrows():
  lat = row["Latitude, dd"]
  lon = row["Longitude, dd"]

  if pd.notna(lat) and pd.notna(lon):
    try:
      # Realiza la geocodificación inversa
      location = geolocator.reverse((lat, lon), timeout=10)
      if location:
        direcciones.append(location.address)
      else:
        direcciones.append("No encontrada")
    except Exception as e:
      direcciones.append(f"Error: {e}")
    # Pausa breve para respetar las políticas de uso del servidor gratuito
    time.sleep(1)
  else:
    direcciones.append("")

# Guardar la dirección en la columna correspondiente
df["Dirección"] = direcciones
df.to_excel("direcciones_caba_completas.xlsx", index=False)
print("¡Proceso finalizado con éxito!")



numero 1
