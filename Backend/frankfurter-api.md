# Prueba Técnica Backend - API de Forex
**Posición:** Backend Senior (Node.js)  
**Duración:** 60 minutos  

---

## Objetivo

Desarrollar una API REST en **Node.js** que consuma la API gratuita de **Frankfurter** para consultar tipos de cambio. La prueba evalúa tu capacidad para construir endpoints limpios, manejar errores y aplicar buenas prácticas en tiempo limitado.

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

### Endpoint 1: GET /forex/latest (30 puntos)
Obtener tipos de cambio actuales para una divisa base.

**Query Parameters:**
- `base` (string, opcional): Divisa base. Default: `EUR`. Ejemplo: `USD`
- `symbols` (string, opcional): Divisas destino separadas por coma. Ejemplo: `GBP,JPY`

**Ejemplo Request:**
```bash
GET /forex/latest?base=USD&symbols=EUR,GBP,JPY
```

**Ejemplo Response:**
```json
{
  "base": "USD",
  "date": "2024-12-26",
  "rates": {
    "EUR": 0.95,
    "GBP": 0.79,
    "JPY": 157.82
  }
}
```

**API de Frankfurter:**
```
GET https://api.frankfurter.dev/v1/latest?base=USD&symbols=EUR,GBP,JPY
```

---

### Endpoint 2: GET /forex/historical (30 puntos)
Obtener tipos de cambio de una fecha específica.

**Query Parameters:**
- `date` (string, requerido): Fecha en formato YYYY-MM-DD
- `base` (string, opcional): Divisa base. Default: `EUR`
- `symbols` (string, opcional): Divisas destino separadas por coma

**Ejemplo Request:**
```bash
GET /forex/historical?date=2024-12-20&base=USD&symbols=EUR,GBP
```

**Ejemplo Response:**
```json
{
  "base": "USD",
  "date": "2024-12-20",
  "rates": {
    "EUR": 0.95,
    "GBP": 0.79
  }
}
```

**API de Frankfurter:**
```
GET https://api.frankfurter.dev/v1/2024-12-20?base=USD&symbols=EUR,GBP
```

---

### Endpoint 3: POST /forex/convert (20 puntos)
Convertir un monto entre divisas.

**Body:**
```json
{
  "from": "USD",
  "to": "EUR",
  "amount": 1000
}
```

**Ejemplo Response:**
```json
{
  "from": "USD",
  "to": "EUR",
  "amount": 1000,
  "rate": 0.95,
  "result": 950.00,
  "date": "2024-12-26"
}
```

**Cómo implementarlo:**
```javascript
// Obtener la tasa desde Frankfurter
GET https://api.frankfurter.dev/v1/latest?base=USD&symbols=EUR

// Calcular conversión
result = amount * rate
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
  "error": "El parámetro 'date' es requerido",
  "statusCode": 400
}
```

### 2. Variables de Entorno (5 puntos)
- `PORT` (default: 3000)
- `FRANKFURTER_BASE_URL` (opcional)

**Nota:** No se requiere API key, Frankfurter es completamente gratuita.

### 3. Código Limpio (5 puntos)
- Funciones pequeñas y claras
- Nombres descriptivos
- Separación de responsabilidades

---

## Criterios de Evaluación

| Criterio | Puntos |
|----------|--------|
| Endpoint /forex/latest funciona | 30 |
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
   Crear archivo .env (opcional):
   PORT=3000
   FRANKFURTER_BASE_URL=https://api.frankfurter.dev/v1
   
   ## Ejecución
   npm start
   
   ## Endpoints
   - GET /forex/latest?base=USD&symbols=EUR,GBP
   - GET /forex/historical?date=2024-12-20&base=USD
   - POST /forex/convert
   ```

---

## Recursos

### API Frankfurter (Gratuita - Sin API Key)
```
Base URL: https://api.frankfurter.dev/v1
```

**Características:**
- ✅ Completamente gratuita
- ✅ Sin límites de uso
- ✅ No requiere API key
- ✅ Datos históricos desde 1999
- ✅ Actualización diaria

### Documentación
- Frankfurter: https://frankfurter.dev
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
│       └── frankfurterService.js
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
# Latest rates
curl "http://localhost:3000/forex/latest?base=USD&symbols=EUR,GBP,JPY"

# Historical
curl "http://localhost:3000/forex/historical?date=2024-12-20&base=USD&symbols=EUR"

# Convert
curl -X POST http://localhost:3000/forex/convert \
  -H "Content-Type: application/json" \
  -d '{"from":"USD","to":"EUR","amount":1000}'
```

### Probar API directamente (para entender la estructura):
```bash
# Latest
curl "https://api.frankfurter.dev/v1/latest?base=USD&symbols=EUR,GBP"

# Historical
curl "https://api.frankfurter.dev/v1/2024-12-20?base=USD"

# Todas las divisas disponibles
curl "https://api.frankfurter.dev/v1/currencies"
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

const BASE_URL = process.env.FRANKFURTER_BASE_URL || 'https://api.frankfurter.dev/v1';

// GET /forex/latest
app.get('/forex/latest', async (req, res) => {
  try {
    const { base = 'EUR', symbols } = req.query;
    
    const params = { base };
    if (symbols) params.symbols = symbols;

    const response = await axios.get(`${BASE_URL}/latest`, { params });

    res.json(response.data);

  } catch (error) {
    res.status(500).json({ 
      error: 'Error al consultar tipos de cambio',
      statusCode: 500
    });
  }
});

// GET /forex/historical
app.get('/forex/historical', async (req, res) => {
  try {
    const { date, base = 'EUR', symbols } = req.query;
    
    if (!date) {
      return res.status(400).json({ 
        error: "El parámetro 'date' es requerido",
        statusCode: 400
      });
    }

    const params = { base };
    if (symbols) params.symbols = symbols;

    const response = await axios.get(`${BASE_URL}/${date}`, { params });

    res.json(response.data);

  } catch (error) {
    res.status(500).json({ 
      error: 'Error al consultar datos históricos',
      statusCode: 500
    });
  }
});

// POST /forex/convert
app.post('/forex/convert', async (req, res) => {
  try {
    const { from, to, amount } = req.body;

    if (!from || !to || !amount) {
      return res.status(400).json({
        error: 'Los campos from, to y amount son requeridos',
        statusCode: 400
      });
    }

    // Obtener tasa de cambio
    const response = await axios.get(`${BASE_URL}/latest`, {
      params: { base: from, symbols: to }
    });

    const rate = response.data.rates[to];
    const result = parseFloat((amount * rate).toFixed(2));

    res.json({
      from,
      to,
      amount,
      rate,
      result,
      date: response.data.date
    });

  } catch (error) {
    res.status(500).json({ 
      error: 'Error al convertir divisas',
      statusCode: 500
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

**Evaluador:** Daniel Boggiano
**Disponible para:** Dudas sobre requisitos, problemas técnicos de setup  
**NO disponible para:** Ayuda con código, solución de bugs

---

**¡Buena suerte! 🚀**
