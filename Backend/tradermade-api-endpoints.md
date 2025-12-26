# Prueba Técnica Backend - API de Forex
**Posición:** Backend Senior (Node.js)  
**Duración:** 60 minutos  

---

## Objetivo

Desarrollar una API REST en **Node.js** que consuma la API de TraderMade para consultar tipos de cambio. La prueba evalúa tu capacidad para construir endpoints limpios, manejar errores y aplicar buenas prácticas en tiempo limitado.

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

### Endpoint 1: GET /forex/live (30 puntos)
Obtener tipos de cambio en tiempo real.

**Query Parameters:**
- `currency` (string, requerido): Par de divisas. Ejemplo: `EURUSD`

**Ejemplo Request:**
```bash
GET /forex/live?currency=EURUSD
```

**Ejemplo Response:**
```json
{
  "currency": "EURUSD",
  "bid": 1.18181,
  "ask": 1.18183,
  "mid": 1.18182,
  "timestamp": 1605267771
}
```

**API de TraderMade:**
```
GET https://marketdata.tradermade.com/api/v1/live?currency=EURUSD&api_key=YOUR_KEY
```

---

### Endpoint 2: GET /forex/historical (30 puntos)
Obtener datos históricos OHLC.

**Query Parameters:**
- `currency` (string, requerido): Par de divisas
- `date` (string, requerido): Fecha en formato YYYY-MM-DD

**Ejemplo Request:**
```bash
GET /forex/historical?currency=EURUSD&date=2024-12-20
```

**Ejemplo Response:**
```json
{
  "currency": "EURUSD",
  "date": "2024-12-20",
  "open": 1.09571,
  "high": 1.09905,
  "low": 1.0952,
  "close": 1.09702
}
```

**API de TraderMade:**
```
GET https://marketdata.tradermade.com/api/v1/historical?currency=EURUSD&date=2024-12-20&api_key=YOUR_KEY
```

---

### Endpoint 3: POST /forex/convert (20 puntos)
Convertir un monto entre divisas.

**Body:**
```json
{
  "from": "EUR",
  "to": "USD",
  "amount": 1000
}
```

**Ejemplo Response:**
```json
{
  "from": "EUR",
  "to": "USD",
  "amount": 1000,
  "rate": 1.18182,
  "result": 1181.82
}
```

**API de TraderMade:**
```
GET https://marketdata.tradermade.com/api/v1/convert?from=EUR&to=USD&amount=1000&api_key=YOUR_KEY
```

---

## Requisitos Técnicos (20 puntos)

### 1. Manejo de Errores (10 puntos)
- Validar parámetros de entrada
- Manejar errores de la API externa
- Retornar mensajes descriptivos en español

**Ejemplo de error:**
```json
{
  "error": "El parámetro 'currency' es requerido",
  "statusCode": 400
}
```

### 2. Variables de Entorno (5 puntos)
- `TRADERMADE_API_KEY`
- `PORT` (default: 3000)

### 3. Código Limpio (5 puntos)
- Funciones pequeñas y claras
- Nombres descriptivos
- Separación de responsabilidades

---

## Criterios de Evaluación

| Criterio | Puntos |
|----------|--------|
| Endpoint /forex/live funciona | 30 |
| Endpoint /forex/historical funciona | 30 |
| Endpoint /forex/convert funciona | 20 |
| Manejo de errores correcto | 10 |
| Variables de entorno | 5 |
| Calidad de código | 5 |
| **TOTAL** | **100** |

---

## Bonus (Opcional - 20 puntos extra)

- **Cache simple (10 pts)**: Implementar cache en memoria para evitar llamadas repetidas
- **Validación robusta (5 pts)**: Validar formato de divisas y fechas
- **Middleware de logging (5 pts)**: Registrar cada request

---

## Entregables

1. **Código fuente**
   - Carpeta con el proyecto
   - README.md con instrucciones

2. **Instrucciones mínimas en README:**
   ```markdown
   # Forex API
   
   ## Instalación
   npm install
   
   ## Configuración
   Crear archivo .env:
   TRADERMADE_API_KEY=your_key
   PORT=3000
   
   ## Ejecución
   npm start
   
   ## Endpoints
   - GET /forex/live?currency=EURUSD
   - GET /forex/historical?currency=EURUSD&date=2024-12-20
   - POST /forex/convert
   ```

---

## Recursos

### API Key de TraderMade
```
TRADERMADE_API_KEY=demo_key_12345
TRADERMADE_BASE_URL=https://marketdata.tradermade.com/api/v1
```

### Documentación
- TraderMade: https://tradermade.com/docs/restful-api
- Express: https://expressjs.com
- Node.js: https://nodejs.org/docs

---

## Ejemplo Mínimo de Estructura

```
forex-api/
├── src/
│   ├── index.js          # Punto de entrada
│   ├── routes/
│   │   └── forex.js      # Rutas
│   ├── controllers/
│   │   └── forexController.js
│   └── services/
│       └── tradermadeService.js
├── .env
├── .env.example
├── package.json
└── README.md
```

---

## Instrucciones de Inicio Rápido

### Opción 1: Express + JavaScript
```bash
mkdir forex-api && cd forex-api
npm init -y
npm install express axios dotenv
```

### Opción 2: Express + TypeScript
```bash
mkdir forex-api && cd forex-api
npm init -y
npm install express axios dotenv
npm install -D typescript @types/node @types/express ts-node
npx tsc --init
```

### Opción 3: NestJS
```bash
nest new forex-api
cd forex-api
npm install @nestjs/axios axios
```

---

## Validación Rápida

### Probar endpoints:
```bash
# Live
curl "http://localhost:3000/forex/live?currency=EURUSD"

# Historical
curl "http://localhost:3000/forex/historical?currency=EURUSD&date=2024-12-20"

# Convert
curl -X POST http://localhost:3000/forex/convert \
  -H "Content-Type: application/json" \
  -d '{"from":"EUR","to":"USD","amount":1000}'
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
- NO uses ChatGPT, Copilot o IAs
- Pregunta dudas al inicio
- **Gestiona tu tiempo: 20 min por endpoint**

### Timing Sugerido
- **0-5 min:** Setup del proyecto
- **5-25 min:** Endpoint /forex/live
- **25-45 min:** Endpoint /forex/historical
- **45-55 min:** Endpoint /forex/convert
- **55-60 min:** Manejo de errores y pruebas

---

## Checklist de Entrega

Antes de entregar, verificar:

- [ ] Los 3 endpoints responden correctamente
- [ ] Manejo básico de errores implementado
- [ ] Variables de entorno configuradas
- [ ] README con instrucciones claras
- [ ] El código compila/ejecuta sin errores
- [ ] Puedes explicar decisiones técnicas

---

## Preguntas Post-Código (5 minutos)

Tras completar el código, se harán estas preguntas:

1. **¿Por qué elegiste [Express/NestJS]?**
2. **¿Cómo mejorarías esta API para producción?**
3. **¿Qué harías si la API de TraderMade está caída?**

---

## Evaluación en Vivo

El evaluador observará:
- ✓ Velocidad de setup del proyecto
- ✓ Organización del código
- ✓ Manejo de errores al codificar
- ✓ Capacidad de debugging
- ✓ Gestión del tiempo

---

## Ejemplo de Implementación Mínima

### index.js (Express)
```javascript
require('dotenv').config();
const express = require('express');
const axios = require('axios');

const app = express();
app.use(express.json());

const API_KEY = process.env.TRADERMADE_API_KEY;
const BASE_URL = 'https://marketdata.tradermade.com/api/v1';

// GET /forex/live
app.get('/forex/live', async (req, res) => {
  try {
    const { currency } = req.query;
    
    if (!currency) {
      return res.status(400).json({ 
        error: "El parámetro 'currency' es requerido" 
      });
    }

    const response = await axios.get(`${BASE_URL}/live`, {
      params: { currency, api_key: API_KEY }
    });

    const quote = response.data.quotes[0];
    res.json({
      currency,
      bid: quote.bid,
      ask: quote.ask,
      mid: quote.mid,
      timestamp: response.data.timestamp
    });

  } catch (error) {
    res.status(500).json({ 
      error: 'Error al consultar tipos de cambio' 
    });
  }
});

// TODO: Implementar /forex/historical
// TODO: Implementar /forex/convert

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

**¡Buena suerte! 🚀**

---

**Confidencial**  
