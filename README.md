# Pedidos - Patrones de Comportamiento

Proyecto en **Spring Boot 3.2.0** que implementa los patrones **Chain of Responsibility** y **Command con Undo** en un sistema de comercio electrónico.  
El sistema valida pedidos mediante una cadena secuencial de validadores y permite ejecutar acciones reversibles sobre los pedidos (confirmar, aplicar descuento).

---

## 📂 Estructura del Proyecto

```text
pedidos-comportamiento/
├── pom.xml
└── src/
    ├── main/java/com/universidad/pedidos/
    │   ├── PedidosApp.java
    │   ├── modelo/
    │   │   └── Pedido.java
    │   ├── cor/
    │   │   ├── ValidadorPedido.java
    │   │   ├── ValidadorStock.java
    │   │   ├── ValidadorMonto.java
    │   │   └── ValidadorCredito.java
    │   └── command/
    │       ├── Comando.java
    │       ├── ComandoConfirmar.java
    │       ├── ComandoAplicarDescuento.java
    │       └── HistorialComandos.java
    └── test/java/com/universidad/pedidos/
        └── CadenaValidacionTest.java
```

---

## 🔗 Patrón Chain of Responsibility
La validación de pedidos se implementa como una cadena de validadores:
```bash
ValidadorPedido cadena = new ValidadorStock()
    .setNext(new ValidadorMonto())
    .setNext(new ValidadorCredito());
```

---

## Pseudocódigo del flujo
```bash
Pedido -> ValidadorStock -> ValidadorMonto -> ValidadorCredito -> Resultado
```
Stock: rechaza si la cantidad supera el inventario disponible.

Monto: rechaza si el total es inferior al mínimo permitido.

Crédito: rechaza si el cliente no tiene crédito aprobado.

✅ Ventaja: cada validador es independiente, cumple una sola responsabilidad y puede añadirse o reordenarse sin modificar los demás.

---

## 📡 Patrón Command con Undo
Cada acción sobre el pedido se encapsula como un Comando:

ComandoConfirmar: cambia el estado a CONFIRMADO.

ComandoAplicarDescuento: aplica un porcentaje de descuento al total.

HistorialComandos: invocador que guarda los comandos en una pila y permite deshacer (undo).

Pseudocódigo del flujo: 
```bash
Historial.ejecutar(ComandoConfirmar)
Historial.ejecutar(ComandoAplicarDescuento)
Historial.deshacer() -> revierte el último comando
```
✅ Justificación: encapsular acciones como objetos permite revertir cambios, mantener un historial y extender fácilmente con nuevas operaciones.

---

## Ejecución
```bash
mvn spring-boot:run
```
<img width="561" height="345" alt="image" src="https://github.com/user-attachments/assets/9bd10fbc-3587-495e-9a30-0c104947963c" />

---

## Ejecución de Pruebas
```bash
mvn test
```
<img width="820" height="346" alt="image" src="https://github.com/user-attachments/assets/5bf61ea4-0992-4597-9b22-101b7e545470" />


