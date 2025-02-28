Informe del Diseño de la Base de Datos
1. Visión General
La base de datos está pensada para un concesionario de vehículos. Su objetivo es gestionar la información sobre los vehículos en stock, la actividad de mantenimiento, los clientes, las ventas y los servicios asociados. Se han definido tablas para representar cada entidad principal y se han establecido relaciones que garantizan la integridad referencial y facilitan la trazabilidad de cada transacción.

2. Detalle de las Tablas y sus Relaciones
EMPRESA

Atributos: id_empresa (PK), nombre, direccion, telefono.
Motivo: Representa al concesionario, permitiendo agrupar y asociar los vehículos que le pertenecen.
Relación: Un concesionario puede tener múltiples vehículos.
VEHICULO

Atributos: id_vehiculo (PK), id_empresa (FK), marca, modelo, anio, vin (único), precio, color, combustible, transmision, estado, disponible, id_factura (FK).
Motivo: Almacena la información de cada vehículo, su estado (nuevo/usado) y su disponibilidad. Cuando se vende, se vincula a una factura.
Relaciones:
Se asocia a la empresa (cada vehículo pertenece a un concesionario).
Se relaciona con el mantenimiento mediante la tabla intermedia MANTENIMIENTO_VEHICULO.
Se vincula a la venta (a través de DETALLE_VENTA) y al inventario/estado para llevar un control.
MANTENIMIENTO_VEHICULO

Atributos: id_mant_vehiculo (PK), id_vehiculo (FK), id_mantenimiento (FK).
Motivo: Tabla intermedia que permite relacionar un vehículo con uno o varios servicios de mantenimiento.
Relación: Facilita la relación entre VEHICULO y MANTENIMIENTO.
MANTENIMIENTO

Atributos: id_mantenimiento (PK), tipo_servicio, costo, fecha_servicio.
Motivo: Registra los servicios de mantenimiento (preventivo, correctivo, etc.) realizados.
Relación:
Se conecta con MANTENIMIENTO_VEHICULO para asociar cada servicio al vehículo correspondiente.
Se vincula a ARREGLOS para detallar las reparaciones o arreglos realizados.
ARREGLOS

Atributos: id_arreglo (PK), id_mantenimiento (FK), id_detalle_mantenimiento (FK).
Motivo: Tabla intermedia que relaciona un servicio de mantenimiento con sus detalles específicos.
Relación: Une MANTENIMIENTO con DETALLE_MANTENIMIENTO.
DETALLE_MANTENIMIENTO

Atributos: id_detalle (PK), id_cliente (FK), descripcion.
Motivo: Almacena la descripción detallada de cada arreglo o reparación realizado, pudiendo asociar la solicitud al cliente en caso de que aplique.
Relación:
Se une a ARREGLOS para formar la relación de muchos a muchos con el mantenimiento.
Se vincula a CLIENTE, registrando quién solicitó el servicio.
CLIENTE

Atributos: id_cliente (PK), nombre, telefono, email, direccion.
Motivo: Registra la información de los clientes, quienes pueden comprar vehículos o solicitar servicios de mantenimiento.
Relación:
Un cliente puede realizar varias compras y ventas.
Se asocia a la solicitud de mantenimiento (vía DETALLE_MANTENIMIENTO) y a la atención por parte de vendedores.
COMPRA

Atributos: id_compra (PK), id_cliente (FK), fecha_compra, total.
Motivo: Documenta cada operación de compra realizada por un cliente.
Relación: Cada compra genera una factura.
FACTURA

Atributos: id_factura (PK), id_compra (FK), fecha_factura, total.
Motivo: Emite el comprobante de la compra realizada.
Relación:
Una factura se asocia de forma uno a uno con una compra.
Se vincula a METODOS_PAGO para registrar cómo se realizó el pago y a VEHICULO para identificar el vehículo vendido.
METODOS_PAGO

Atributos: id_metodo (PK), id_factura (FK), tipo_pago, detalles.
Motivo: Permite registrar los diferentes métodos de pago (tarjeta, transferencia, etc.) usados en la factura.
Relación: Cada factura puede tener varios métodos de pago asociados.
INVENTARIO

Atributos: id_inventario (PK), id_vehiculo (FK), disponible, fecha_actualizacion.
Motivo: Lleva el control en tiempo real de la disponibilidad de cada vehículo en stock.
Relación: Cada registro se asocia a un vehículo.
ESTADO_VEHICULO

Atributos: id_estado (PK), id_vehiculo (FK), estado, fecha_estado.
Motivo: Permite registrar el histórico de estados (disponible, vendido, etc.) de cada vehículo.
Relación: Cada vehículo puede tener múltiples registros de estado a lo largo del tiempo.
DETALLE_VENTA

Atributos: id_detalle_venta (PK), id_vehiculo (FK), id_venta (FK), precio_venta.
Motivo: Desglosa la información de cada venta, asociando uno o más vehículos a una transacción de venta.
Relación:
Une VEHICULO y VENTA, permitiendo registrar detalles de cada vehículo vendido.
VENTA

Atributos: id_venta (PK), id_cliente (FK), fecha_venta, total.
Motivo: Registra la transacción de venta realizada a un cliente.
Relación:
Cada venta se desglosa en DETALLE_VENTA y se vincula a CLIENTE.
CLIENTE_VENDEDOR

Atributos: id_cliente_vendedor (PK), id_cliente (FK), id_vendedor (FK).
Motivo: Establece la relación de atención entre clientes y vendedores, permitiendo conocer qué vendedor atendió a cada cliente.
Relación: Une CLIENTE y VENDEDOR.
VENDEDOR

Atributos: id_vendedor (PK), nombre, num_empleado, fecha_contratacion.
Motivo: Registra la información de los vendedores que trabajan en el concesionario.
Relación:
Se asocia a la tabla CLIENTE_VENDEDOR y a TRANZACCION_VENTA para registrar sus transacciones.
TRANZACCION_VENTA

Atributos: id_tranzaccion (PK), id_vendedor (FK), id_venta (FK), monto, fecha_tranzaccion.
Motivo: Permite detallar las transacciones de venta realizadas por cada vendedor.
Relación:
Une a VENDEDOR y VENTA, indicando qué vendedor realizó la venta y el monto de la transacción.

![image](https://github.com/user-attachments/assets/e4cf3251-0a1b-4c37-990c-08b42f27bea6)

CREATE DATABASE concesionario;
USE concesionario;

CREATE TABLE EMPRESA (
    id_empresa INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(255),
    direccion VARCHAR(255),
    telefono VARCHAR(50)
);

CREATE TABLE CLIENTE (
    id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(255),
    telefono VARCHAR(50),
    email VARCHAR(255),
    direccion VARCHAR(255)
);

CREATE TABLE VENDEDOR (
    id_vendedor INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(255),
    num_empleado VARCHAR(50),
    fecha_contratacion DATE
);

CREATE TABLE COMPRA (
    id_compra INT AUTO_INCREMENT PRIMARY KEY,
    id_cliente INT,
    fecha_compra DATE,
    total DECIMAL(10,2),
    FOREIGN KEY (id_cliente) REFERENCES CLIENTE(id_cliente)
);

CREATE TABLE FACTURA (
    id_factura INT AUTO_INCREMENT PRIMARY KEY,
    id_compra INT,
    fecha_factura DATE,
    total DECIMAL(10,2),
    FOREIGN KEY (id_compra) REFERENCES COMPRA(id_compra)
);

CREATE TABLE METODOS_PAGO (
    id_metodo INT AUTO_INCREMENT PRIMARY KEY,
    id_factura INT,
    tipo_pago VARCHAR(100),
    detalles VARCHAR(255),
    FOREIGN KEY (id_factura) REFERENCES FACTURA(id_factura)
);

CREATE TABLE VENTA (
    id_venta INT AUTO_INCREMENT PRIMARY KEY,
    id_cliente INT,
    fecha_venta DATE,
    total DECIMAL(10,2),
    FOREIGN KEY (id_cliente) REFERENCES CLIENTE(id_cliente)
);

CREATE TABLE VEHICULO (
    id_vehiculo INT AUTO_INCREMENT PRIMARY KEY,
    id_empresa INT,
    marca VARCHAR(255),
    modelo VARCHAR(255),
    anio INT,
    vin VARCHAR(100) UNIQUE,
    precio DECIMAL(10,2),
    color VARCHAR(50),
    combustible VARCHAR(50),
    transmision VARCHAR(50),
    estado VARCHAR(50),
    disponible BOOLEAN,
    id_factura INT,
    FOREIGN KEY (id_empresa) REFERENCES EMPRESA(id_empresa),
    FOREIGN KEY (id_factura) REFERENCES FACTURA(id_factura)
);

CREATE TABLE INVENTARIO (
    id_inventario INT AUTO_INCREMENT PRIMARY KEY,
    id_vehiculo INT,
    disponible BOOLEAN,
    fecha_actualizacion DATE,
    FOREIGN KEY (id_vehiculo) REFERENCES VEHICULO(id_vehiculo)
);

CREATE TABLE ESTADO_VEHICULO (
    id_estado INT AUTO_INCREMENT PRIMARY KEY,
    id_vehiculo INT,
    estado VARCHAR(50),
    fecha_estado DATE,
    FOREIGN KEY (id_vehiculo) REFERENCES VEHICULO(id_vehiculo)
);

CREATE TABLE DETALLE_VENTA (
    id_detalle_venta INT AUTO_INCREMENT PRIMARY KEY,
    id_vehiculo INT,
    id_venta INT,
    precio_venta DECIMAL(10,2),
    FOREIGN KEY (id_vehiculo) REFERENCES VEHICULO(id_vehiculo),
    FOREIGN KEY (id_venta) REFERENCES VENTA(id_venta)
);

CREATE TABLE TRANZACCION_VENTA (
    id_tranzaccion INT AUTO_INCREMENT PRIMARY KEY,
    id_vendedor INT,
    id_venta INT,
    monto DECIMAL(10,2),
    fecha_tranzaccion DATE,
    FOREIGN KEY (id_vendedor) REFERENCES VENDEDOR(id_vendedor),
    FOREIGN KEY (id_venta) REFERENCES VENTA(id_venta)
);

CREATE TABLE CLIENTE_VENDEDOR (
    id_cliente_vendedor INT AUTO_INCREMENT PRIMARY KEY,
    id_cliente INT,
    id_vendedor INT,
    FOREIGN KEY (id_cliente) REFERENCES CLIENTE(id_cliente),
    FOREIGN KEY (id_vendedor) REFERENCES VENDEDOR(id_vendedor)
);

CREATE TABLE MANTENIMIENTO (
    id_mantenimiento INT AUTO_INCREMENT PRIMARY KEY,
    tipo_servicio VARCHAR(100),
    costo DECIMAL(10,2),
    fecha_servicio DATE
);

CREATE TABLE MANTENIMIENTO_VEHICULO (
    id_mant_vehiculo INT AUTO_INCREMENT PRIMARY KEY,
    id_vehiculo INT,
    id_mantenimiento INT,
    FOREIGN KEY (id_vehiculo) REFERENCES VEHICULO(id_vehiculo),
    FOREIGN KEY (id_mantenimiento) REFERENCES MANTENIMIENTO(id_mantenimiento)
);

CREATE TABLE DETALLE_MANTENIMIENTO (
    id_detalle INT AUTO_INCREMENT PRIMARY KEY,
    id_cliente INT,
    descripcion VARCHAR(255),
    FOREIGN KEY (id_cliente) REFERENCES CLIENTE(id_cliente)
);

CREATE TABLE ARREGLOS (
    id_arreglo INT AUTO_INCREMENT PRIMARY KEY,
    id_mantenimiento INT,
    id_detalle_mantenimiento INT,
    FOREIGN KEY (id_mantenimiento) REFERENCES MANTENIMIENTO(id_mantenimiento),
    FOREIGN KEY (id_detalle_mantenimiento) REFERENCES DETALLE_MANTENIMIENTO(id_detalle)
);

