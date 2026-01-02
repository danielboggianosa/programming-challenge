# Prueba Técnica Backend - API de Clima
## Objetivo

Desarrollar una API REST en **Node.js** que consuma la API gratuita de **Open-Meteo** para consultar datos meteorológicos. La prueba evalúa tu capacidad para construir endpoints limpios, transformar datos y aplicar buenas prácticas en tiempo limitado.

---

## Stack Tecnológico

**Obligatorio:**
- Node.js (v16+)
- Express.js o NestJS (tu elección)
- TypeScript (preferido) o JavaScript

**Opcional:**
- Cache en memoria
- Axios o node-fetch para HTTP

---

## Requisitos Funcionales

### Endpoint 1: GET /weather/current (30 puntos)
Obtener clima actual para una ubicación específica.

**Query Parameters:**
- `latitude` (number, requerido): Latitud. Ejemplo: `52.52`
- `longitude` (number, requerido): Longitud. Ejemplo: `13.41`

**Ejemplo Request:**
```bash
GET /weather/current?latitude=52.52&longitude=13.41
```

**Ejemplo Response:**
```json
{
  "location": {
    "latitude": 52.52,
    "longitude": 13.41
  },
  "current": {
    "temperature": 15.3,
    "humidity": 65,
    "wind_speed": 12.5,
    "weather_code": 3,
    "description": "Nublado"
  },
  "timestamp": "2024-12-26T10:00"
}
```

**API de Open-Meteo:**
```
GET https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&current=temperature_2m,relative_humidity_2m,wind_speed_10m,weather_code
```

**Weather Codes (WMO):**
- 0: Despejado
- 1,2,3: Parcialmente nublado, nublado
- 45,48: Niebla
- 51,53,55: Llovizna
- 61,63,65: Lluvia
- 71,73,75: Nieve
- 95: Tormenta

---

### Endpoint 2: GET /weather/forecast (30 puntos)
Obtener pronóstico de 7 días para una ubicación.

**Query Parameters:**
- `latitude` (number, requerido): Latitud
- `longitude` (number, requerido): Longitud
- `days` (number, opcional): Días a mostrar (1-7). Default: 7

**Ejemplo Request:**
```bash
GET /weather/forecast?latitude=52.52&longitude=13.41&days=3
```

**Ejemplo Response:**
```json
{
  "location": {
    "latitude": 52.52,
    "longitude": 13.41
  },
  "forecast": [
    {
      "date": "2024-12-26",
      "temperature_max": 18.5,
      "temperature_min": 12.3,
      "precipitation_sum": 2.5,
      "weather_code": 61,
      "description": "Lluvia"
    },
    {
      "date": "2024-12-27",
      "temperature_max": 20.1,
      "temperature_min": 14.2,
      "precipitation_sum": 0.0,
      "weather_code": 1,
      "description": "Parcialmente nublado"
    },
    {
      "date": "2024-12-28",
      "temperature_max": 19.8,
      "temperature_min": 13.9,
      "precipitation_sum": 1.2,
      "weather_code": 51,
      "description": "Llovizna"
    }
  ]
}
```

**API de Open-Meteo:**
```
GET https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&daily=temperature_2m_max,temperature_2m_min,precipitation_sum,weather_code&forecast_days=3
```

---

### Endpoint 3: POST /weather/compare (20 puntos)
Comparar el clima actual de dos ubicaciones.

**Body:**
```json
{
  "location1": {
    "name": "Lima",
    "latitude": -12.04,
    "longitude": -77.03
  },
  "location2": {
    "name": "Madrid",
    "latitude": 40.42,
    "longitude": -3.70
  }
}
```

**Ejemplo Response:**
```json
{
  "comparison": [
    {
      "name": "Lima",
      "temperature": 22.5,
      "humidity": 80,
      "wind_speed": 8.5,
      "description": "Nublado"
    },
    {
      "name": "Madrid",
      "temperature": 12.3,
      "humidity": 55,
      "wind_speed": 15.2,
      "description": "Despejado"
    }
  ],
  "difference": {
    "temperature": 10.2,
    "warmer_location": "Lima"
  }
}
```

**Cómo implementarlo:**
```javascript
// Hacer 2 requests paralelos a Open-Meteo
const [data1, data2] = await Promise.all([
  axios.get('https://api.open-meteo.com/v1/forecast?latitude=-12.04&longitude=-77.03&current=...'),
  axios.get('https://api.open-meteo.com/v1/forecast?latitude=40.42&longitude=-3.70&current=...')
]);

// Calcular diferencia
const tempDiff = Math.abs(data1.temperature - data2.temperature);
```

---

## Requisitos Técnicos (20 puntos)

### 1. Manejo de Errores (10 puntos)
- Validar coordenadas (-90 a 90 para latitud, -180 a 180 para longitud)
- Manejar errores de la API externa
- Retornar mensajes descriptivos en español

**Ejemplo de error:**
```json
{
  "error": "La latitud debe estar entre -90 y 90",
  "statusCode": 400
}
```

### 2. Transformación de Datos (5 puntos)
- Convertir weather_code a descripción legible en español
- Formatear temperaturas a 1 decimal
- Incluir timestamps en formato ISO

### 3. Código Limpio (5 puntos)
- Funciones pequeñas y claras
- Nombres descriptivos
- Separación de responsabilidades
- Constantes para weather codes

---

## Criterios de Evaluación

| Criterio | Puntos |
|----------|--------|
| Endpoint /weather/current funciona | 30 |
| Endpoint /weather/forecast funciona | 30 |
| Endpoint /weather/compare funciona | 20 |
| Manejo de errores correcto | 10 |
| Transformación de datos | 5 |
| Calidad de código | 5 |
| **TOTAL** | **100** |

---

## Bonus (Opcional - 20 puntos extra)

- **Cache simple (10 pts)**: Cachear datos del clima por 10 minutos
- **Geocoding inverso (5 pts)**: Dado lat/lon, retornar nombre de ciudad
- **Helper function (5 pts)**: Función para convertir Celsius a Fahrenheit

---

## Entregables

1. **Código fuente**
   - Carpeta con el proyecto
   - README.md con instrucciones

2. **Instrucciones mínimas en README:**
   ```markdown
   # Weather API
   
   ## Instalación
   npm install
   
   ## Configuración
   Crear archivo .env (opcional):
   PORT=3000
   OPEN_METEO_BASE_URL=https://api.open-meteo.com/v1
   
   ## Ejecución
   npm start
   
   ## Endpoints
   - GET /weather/current?latitude=52.52&longitude=13.41
   - GET /weather/forecast?latitude=52.52&longitude=13.41&days=3
   - POST /weather/compare
   ```

---

## Recursos

### API Open-Meteo (Gratuita - Sin API Key)
```
Base URL: https://api.open-meteo.com/v1
```

**Características:**
- ✅ Completamente gratuita
- ✅ Sin límites razonables (<10,000 calls/día)
- ✅ No requiere API key ni registro
- ✅ Datos históricos de 80 años
- ✅ Actualización horaria
- ✅ Respuestas <10ms

### Documentación
- Open-Meteo: https://open-meteo.com
- Express: https://expressjs.com
- Node.js: https://nodejs.org/docs

### Coordenadas de Ciudades Comunes
```javascript
const CITIES = {
  lima: { lat: -12.04, lon: -77.03 },
  madrid: { lat: 40.42, lon: -3.70 },
  london: { lat: 51.51, lon: -0.13 },
  newYork: { lat: 40.71, lon: -74.01 },
  tokyo: { lat: 35.68, lon: 139.65 },
  sydney: { lat: -33.87, lon: 151.21 }
};
```

---

## Ejemplo de Estructura

```
weather-api/
├── src/
│   ├── index.js          # Punto de entrada
│   ├── routes/
│   │   └── weather.js    # Rutas
│   ├── controllers/
│   │   └── weatherController.js
│   ├── services/
│   │   └── openMeteoService.js
│   └── utils/
│       └── weatherCodes.js  # Mapeo de códigos
├── .env
├── .env.example
├── package.json
└── README.md
```

---

## Validación Rápida

### Probar endpoints:
```bash
# Current weather
curl "http://localhost:3000/weather/current?latitude=52.52&longitude=13.41"

# Forecast
curl "http://localhost:3000/weather/forecast?latitude=-12.04&longitude=-77.03&days=3"

# Compare
curl -X POST http://localhost:3000/weather/compare \
  -H "Content-Type: application/json" \
  -d '{
    "location1": {"name":"Lima","latitude":-12.04,"longitude":-77.03},
    "location2": {"name":"Madrid","latitude":40.42,"longitude":-3.70}
  }'
```

### Probar API directamente (para entender la estructura):
```bash
# Current weather
curl "https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&current=temperature_2m,relative_humidity_2m,wind_speed_10m,weather_code"

# Forecast
curl "https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&daily=temperature_2m_max,temperature_2m_min,precipitation_sum,weather_code&forecast_days=7"
```

---

## Rúbrica de Aprobación

| Puntaje | Resultado |
|---------|-----------|
| 80-120 pts | **Aprobado** - Excelente |
| 60-79 pts | **Aprobado** - Cumple expectativas |
| 40-59 pts | **Condicional** - Revisar en siguiente fase |
| <40 pts | **No Aprobado** |

---

## Notas Importantes

### Para el Candidato
- **Prioriza funcionalidad sobre perfección**
- Puedes usar Google y documentación oficial
- Pregunta dudas al inicio
- **Gestiona tu tiempo: 20 min por endpoint**

### Timing Sugerido
- **0-5 min:** Setup del proyecto
- **5-25 min:** Endpoint /weather/current
- **25-45 min:** Endpoint /weather/forecast
- **45-55 min:** Endpoint /weather/compare
- **55-60 min:** Manejo de errores y pruebas

---

## Checklist de Entrega

Antes de entregar, verificar:

- [ ] Los 3 endpoints responden correctamente
- [ ] Weather codes convertidos a español
- [ ] Validación de coordenadas
- [ ] Manejo de errores implementado
- [ ] README con instrucciones claras
- [ ] El código compila/ejecuta sin errores
- [ ] Puedes explicar decisiones técnicas

---

## Preguntas Post-Código (5 minutos)

Al finalizar la codificación, se discutirán:

### **Arquitectura y Diseño**
1. ¿Por qué organizaste el código de esta manera?
2. ¿Cómo transformaste los weather codes a descripciones?

### **Escalabilidad**
3. Si necesitaras agregar más servicios de clima (OpenWeather, WeatherAPI), ¿cómo estructurarías el código?
4. ¿Cómo manejarías caché distribuido para múltiples instancias?

### **Datos**
5. ¿Qué validaciones adicionales agregarías para producción?

---

## Ejemplo de Implementación Mínima

### weatherCodes.js
```javascript
const WEATHER_CODES = {
  0: 'Despejado',
  1: 'Parcialmente nublado',
  2: 'Parcialmente nublado',
  3: 'Nublado',
  45: 'Niebla',
  48: 'Niebla',
  51: 'Llovizna ligera',
  53: 'Llovizna moderada',
  55: 'Llovizna intensa',
  61: 'Lluvia ligera',
  63: 'Lluvia moderada',
  65: 'Lluvia intensa',
  71: 'Nevada ligera',
  73: 'Nevada moderada',
  75: 'Nevada intensa',
  95: 'Tormenta',
  96: 'Tormenta con granizo',
  99: 'Tormenta con granizo'
};

function getWeatherDescription(code) {
  return WEATHER_CODES[code] || 'Desconocido';
}

module.exports = { getWeatherDescription, WEATHER_CODES };
```

### index.js (Express)
```javascript
require('dotenv').config();
const express = require('express');
const axios = require('axios');
const { getWeatherDescription } = require('./utils/weatherCodes');

const app = express();
app.use(express.json());

const BASE_URL = process.env.OPEN_METEO_BASE_URL || 'https://api.open-meteo.com/v1';

// Validar coordenadas
function validateCoordinates(lat, lon) {
  if (lat < -90 || lat > 90) {
    throw new Error('La latitud debe estar entre -90 y 90');
  }
  if (lon < -180 || lon > 180) {
    throw new Error('La longitud debe estar entre -180 y 180');
  }
}

// GET /weather/current
app.get('/weather/current', async (req, res) => {
  try {
    const latitude = parseFloat(req.query.latitude);
    const longitude = parseFloat(req.query.longitude);
    
    if (!latitude || !longitude) {
      return res.status(400).json({ 
        error: "Los parámetros 'latitude' y 'longitude' son requeridos",
        statusCode: 400
      });
    }

    validateCoordinates(latitude, longitude);

    const response = await axios.get(`${BASE_URL}/forecast`, {
      params: {
        latitude,
        longitude,
        current: 'temperature_2m,relative_humidity_2m,wind_speed_10m,weather_code'
      }
    });

    const current = response.data.current;

    res.json({
      location: { latitude, longitude },
      current: {
        temperature: parseFloat(current.temperature_2m.toFixed(1)),
        humidity: current.relative_humidity_2m,
        wind_speed: parseFloat(current.wind_speed_10m.toFixed(1)),
        weather_code: current.weather_code,
        description: getWeatherDescription(current.weather_code)
      },
      timestamp: current.time
    });

  } catch (error) {
    res.status(error.message.includes('entre') ? 400 : 500).json({ 
      error: error.message || 'Error al consultar el clima',
      statusCode: error.message.includes('entre') ? 400 : 500
    });
  }
});

// GET /weather/forecast
app.get('/weather/forecast', async (req, res) => {
  try {
    const latitude = parseFloat(req.query.latitude);
    const longitude = parseFloat(req.query.longitude);
    const days = parseInt(req.query.days) || 7;
    
    if (!latitude || !longitude) {
      return res.status(400).json({ 
        error: "Los parámetros 'latitude' y 'longitude' son requeridos",
        statusCode: 400
      });
    }

    if (days < 1 || days > 7) {
      return res.status(400).json({
        error: "El parámetro 'days' debe estar entre 1 y 7",
        statusCode: 400
      });
    }

    validateCoordinates(latitude, longitude);

    const response = await axios.get(`${BASE_URL}/forecast`, {
      params: {
        latitude,
        longitude,
        daily: 'temperature_2m_max,temperature_2m_min,precipitation_sum,weather_code',
        forecast_days: days
      }
    });

    const daily = response.data.daily;
    const forecast = daily.time.map((date, index) => ({
      date,
      temperature_max: parseFloat(daily.temperature_2m_max[index].toFixed(1)),
      temperature_min: parseFloat(daily.temperature_2m_min[index].toFixed(1)),
      precipitation_sum: parseFloat(daily.precipitation_sum[index].toFixed(1)),
      weather_code: daily.weather_code[index],
      description: getWeatherDescription(daily.weather_code[index])
    }));

    res.json({
      location: { latitude, longitude },
      forecast
    });

  } catch (error) {
    res.status(error.message.includes('entre') || error.message.includes('debe') ? 400 : 500).json({ 
      error: error.message || 'Error al consultar el pronóstico',
      statusCode: error.message.includes('entre') || error.message.includes('debe') ? 400 : 500
    });
  }
});

// POST /weather/compare
app.post('/weather/compare', async (req, res) => {
  try {
    const { location1, location2 } = req.body;

    if (!location1 || !location2) {
      return res.status(400).json({
        error: 'Se requieren location1 y location2',
        statusCode: 400
      });
    }

    validateCoordinates(location1.latitude, location1.longitude);
    validateCoordinates(location2.latitude, location2.longitude);

    const params = {
      current: 'temperature_2m,relative_humidity_2m,wind_speed_10m,weather_code'
    };

    const [data1, data2] = await Promise.all([
      axios.get(`${BASE_URL}/forecast`, {
        params: { latitude: location1.latitude, longitude: location1.longitude, ...params }
      }),
      axios.get(`${BASE_URL}/forecast`, {
        params: { latitude: location2.latitude, longitude: location2.longitude, ...params }
      })
    ]);

    const temp1 = data1.data.current.temperature_2m;
    const temp2 = data2.data.current.temperature_2m;
    const tempDiff = Math.abs(temp1 - temp2);

    res.json({
      comparison: [
        {
          name: location1.name,
          temperature: parseFloat(temp1.toFixed(1)),
          humidity: data1.data.current.relative_humidity_2m,
          wind_speed: parseFloat(data1.data.current.wind_speed_10m.toFixed(1)),
          description: getWeatherDescription(data1.data.current.weather_code)
        },
        {
          name: location2.name,
          temperature: parseFloat(temp2.toFixed(1)),
          humidity: data2.data.current.relative_humidity_2m,
          wind_speed: parseFloat(data2.data.current.wind_speed_10m.toFixed(1)),
          description: getWeatherDescription(data2.data.current.weather_code)
        }
      ],
      difference: {
        temperature: parseFloat(tempDiff.toFixed(1)),
        warmer_location: temp1 > temp2 ? location1.name : location2.name
      }
    });

  } catch (error) {
    res.status(400).json({ 
      error: error.message || 'Error al comparar ubicaciones',
      statusCode: 400
    });
  }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`API corriendo en http://localhost:${PORT}`);
});
```

---

## Contacto Durante la Prueba

**Evaluador:** Daniel (CTO Cuevatech)  
**Disponible para:** Dudas sobre requisitos, problemas técnicos de setup  
**NO disponible para:** Ayuda con código, solución de bugs

---

**¡Buena suerte! 🌤️**

---

**Versión:** 1.0 - Enero 2026
