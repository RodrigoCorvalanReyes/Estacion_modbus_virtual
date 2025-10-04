# Changelog - Corrección de Unidades de Medida

## Fecha: 1 de Octubre, 2025

## Archivos Modificados

### 1. `register_table_PM2120.json`
**Cambios realizados:**
- ✅ Agregado campo `"unit"` a todos los 76 registros del PM2120
- ✅ Asignadas unidades de medida correctas según tipo de parámetro:

| Tipo de Parámetro | Unidad | Ejemplos |
|-------------------|--------|----------|
| Corrientes | A | Current A, Current B, Current C, Current N, Current G, Current Avg |
| Voltajes | V | Voltage A-B, Voltage B-C, Voltage A-N, etc. |
| Potencias Activas | W | Active Power A, Active Power B, Active Power Total |
| Potencias Reactivas | VAR | Reactive Power A, Reactive Power B, Reactive Power Total |
| Potencias Aparentes | VA | Apparent Power A, Apparent Power B, Apparent Power Total |
| Frecuencia | Hz | Frequency |
| Desbalances | % | Current Unbalance A, Voltage Unbalance A-B, etc. |
| Factores de Potencia | "" | Power Factor A, Power Factor Total (sin unidad - adimensional) |
| Energías Activas | Wh | Active Energy Delivered, Active Energy Received |
| Energías Reactivas | VARh | Reactive Energy Delivered, Reactive Energy Received |
| Energías Aparentes | VAh | Apparent Energy Delivered, Apparent Energy Received |
| Tiempos de Demanda | min/s | Power Demand Interval Duration, Subinterval Duration |

### 2. `app.js`
**Cambios realizados:**
- ✅ Modificada función de API `/api/pm2120` para usar directamente `reg.unit` del JSON
- ✅ Eliminada función `getUnitForParameter()` que deducía unidades desde la descripción
- ✅ Agregada función `registersToInt64()` para conversión correcta de enteros de 64 bits
- ✅ Completada función de mapeo de datos para manejar todos los tipos de datos (FLOAT32, INT64, DATETIME, INT16, INT16U)

### 3. `public/index.html`
**Estado:** 
- ✅ No requirió modificaciones - ya estaba configurado para mostrar las unidades desde la API
- ✅ El campo `item.unit` se muestra correctamente en la interfaz web

## Verificación de Funcionamiento

### Pruebas Realizadas:
1. ✅ Aplicación se ejecuta sin errores
2. ✅ API `/api/pm2120` devuelve datos con unidades correctas
3. ✅ Verificación de unidades por tipo de parámetro:
   - Corrientes: A ✅
   - Voltajes: V ✅  
   - Potencias activas: W ✅
   - Potencias reactivas: VAR ✅
   - Potencias aparentes: VA ✅
   - Frecuencia: Hz ✅
   - Factores de potencia: (sin unidad) ✅
   - Desbalances: % ✅

### Registro de Prueba:
```json
{
  "description": "Current A",
  "displayValue": "25.283",
  "unit": "A"
},
{
  "description": "Voltage A-B", 
  "displayValue": "225.466",
  "unit": "V"
},
{
  "description": "Active Power A",
  "displayValue": "2315.540", 
  "unit": "W"
}
```

## Beneficios de los Cambios

1. **Precisión**: Unidades correctas según estándares eléctricos internacionales
2. **Mantenibilidad**: Unidades definidas centralmente en el JSON de configuración
3. **Escalabilidad**: Fácil agregar nuevos parámetros con sus unidades correspondientes
4. **Consistencia**: Eliminada lógica de deducción de unidades basada en texto
5. **Visualización**: Interfaz web muestra correctamente las unidades en tiempo real

## Notas Técnicas

- Los factores de potencia son adimensionales (sin unidad) como corresponde
- Las energías usan las unidades correctas: Wh, VARh, VAh
- Los tiempos de demanda usan minutos (min) y segundos (s) según corresponde
- Todos los 76 parámetros del PM2120 tienen sus unidades correctamente asignadas