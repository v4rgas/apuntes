# Introducción a la integración
## Por qué integrar sistemas
- Procesos transparentes para la comunicación
- Interoperabilidad de sistemas
- Automatizacion de procesos
- Disponibilidad de infomración
- Uso eficiente de software
- Comunicacion de sistemas nuevos con existentes

## Soluciones
Comprar soluciones disponibles en el mercado
Construir para integrar sistemas existentes

## 4 aproximaciones a la ingracion de sistemas
1. Intercambio de archivos
2. Bases de datos compartidas
3. RPC (Remote Procedure Call)
4. Mensajeria

# Integración por eventos
A diferencia de un sistema basado en servicios no se le pregunta a un servidor por información sino que el servidor avisa al cliente

## Ventajas
- Mejor monitoreo
- Menos consultas
- Menor overhead y carga
- Información en tiempo real

## Definiciones
- Evento: Algo que sucede
- Emisor: Quien genera los eventos
- Suscriptores: Quienes reciben los eventos
- Notificar: Acción de propagar un evento

## Caracteristicas
- Bajo acoplamiento
- Multiples suscriptores
- Comunicación asíncrona (fire and forget)
- Iniciada por el emisor
- Solo un mensaje
- Suscriptores deben registrarse para recibir eventos

## Principios de diseño de eventos
Por lo general debe contener

- Accion (Que esta pasando)
- Timestamp (Cuando)
- Estado (Informacion relevante del evento)
- Auto contenido (Toda la informacion relevante en el mismo evento)

### Webhook
Permite usar la arquitectura de eventos entre servidores

