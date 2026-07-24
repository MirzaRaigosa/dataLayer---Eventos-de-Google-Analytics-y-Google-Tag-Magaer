
# Implementación de Data Layer para Eventos de Google
Implementación de Data Layer y eventos personalizados para Google Tag Manager y Google Analytics 

## Descripción del Proyecto
Este proyecto consiste en el diseño e implementación de una capa de datos (Data Layer) personalizada para capturar el comportamiento de los usuarios en el sitio web. La estructura fue creada en un entorno de desarrollo moderno para integrarse de forma limpia con Google Tag Manager (GTM) y enviar datos precisos a Google Analytics 4 (GA4).

## Tecnologías Utilizadas
* **Lenguajes:** TypeScript (TS)
* **Arquitectura:** Node.js / Arquitectura Microfrontend
* **Herramientas de Analítica:** Google Tag Manager (GTM) & Google Analytics 4 (GA4) E-commerce Standard
* **Control de Versiones:** Git & GitHub

## Arquitectura de Eventos Implementados

### 1. Tipos de Eventos Principales (`event`)

| Evento | Uso / Contexto | Aplicación técnica |
| :--- | :--- | :--- |
| `ga4.trackEvent` | General | Utilizado en casi todos los eventos del flujo |
| `authentication` | Seguridad | Solo post identidad digital / biometría (Uso de hashes) |

### 2. Eventos Personalizados (`eventName` cuando `event === "ga4.trackEvent"`)

| `eventName` | Descripción y Propósito |
| :--- | :--- |
| `application_step` | Carga / avance de pantallas (login, OTP, carrito, éxito, express…) |
| `user_interaction` | Clicks específicos (validar OTP, continuar, regresar, términos, etc.) |
| `form_interaction` | Interacción inicial con los formularios de captura |
| `form_submited` / `form_submitted` | Envío de formulario exitoso (Soporte para ambas grafías del sistema) |
| `event_error` | Captura de errores técnicos (teléfono, OTP, bloqueo, compra, conexión…) |
| `view_item_list` | Visualización del listado de productos disponibles |
| `select_item` | Selección de un producto específico |
| `add_to_cart` | Acción de agregar un producto al carrito de compras |
| `begin_checkout` | Inicio formal del proceso de checkout |
| `purchase` | Compra o pago exitoso de la transacción (Incluye flujo express) |

## Ejemplo de Código de la Capa de Datos (TypeScript)
Para asegurar el tipado de los datos y enviar la información estructurada a Google Tag Manager, se desarrolló la siguiente arquitectura de funciones de rastreo:

```typescript
export function trackExpressConfirmarPagarClick(
 maxValue: string,
 weeklyInstallment: string,
 weekly: string,
): void {
 pushGtmEvent({
   event: "ga4.trackEvent",
   eventName: "user_interaction",
   eventParams: {
     element: "confirmar_pagar",
     flow: "personaliza_tus_plazos",
     flow_name: "flujo_express",
     step_number: "1",
     max_value: maxValue,
     weekly_installment: weeklyInstallment,
     weekly,
     section: "oferta_final",
     step_name: "tu_mejor_oferta",
   },
 });
}
```

## Impacto del Proyecto

### Beneficios Técnicos y de Negocio Entregados
* **Construcción del Funnel de Conversión:** Habilitación de datos para graficar todo el flujo transaccional.
* **Segmentación de Experiencias:** Diferenciación clara entre flujos de compra Express y Multicategoría.
* **Mapeo del Abandono (Drop-off):** Localización exacta del paso donde los usuarios abandonan el proceso.
* **Optimización de Conversión (CRO):** Datos listos para reducir la fricción en el checkout.
* **Detección de Errores Críticos:** Identificación de fallas operativas en pasos como la validación OTP.

### Cumplimiento de Criterios de Aceptación (Alineación con el Negocio)
* **Visualización en Tiempo Real:** El equipo de Botón de Pago consulta el embudo de forma autónoma.
* **Diagnóstico de Fricción:** Capacidad de identificar si el usuario se detiene por usabilidad o fallas del sistema.
* **Decisiones Basadas en Datos:** Datos estructurados para que el equipo de producto priorice mejoras en la app.
