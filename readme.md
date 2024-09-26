# Ejercicios de patrones de diseño
***
## Patrón Singleton

### Ejercicio 1: Gestionar Configuración Global del Sistema
Objetivo: Implementar un patrón Singleton para gestionar la configuración global de la aplicación de inventario.
● Crear una clase Configuracion que siga el patrón Singleton.
● Esta clase debe almacenar propiedades como idioma, rutaBaseDatos y nivelRegistro.
● Agregar métodos para obtener y actualizar cada una de estas propiedades.
● Asegurar que solo exista una instancia de Configuracion en toda la aplicación.

```typescript
class Configuracion {
    private static instancia: Configuracion;
    private idioma: string;
    private rutaBaseDatos: string;
    private nivelRegistro: string;

    private constructor() {
        this.idioma = 'es';
        this.rutaBaseDatos = 'localhost';
        this.nivelRegistro = 'info';
    }

    public static getInstance(): Configuracion {
        if (!Configuracion.instancia) {
            Configuracion.instancia = new Configuracion();
        }
        return Configuracion.instancia;
    }

    public getIdioma(): string {
        return this.idioma;
    }

    public setIdioma(idioma: string): void {
        this.idioma = idioma;
    }

    public getRutaBaseDatos(): string {
        return this.rutaBaseDatos;
    }

    public setRutaBaseDatos(rutaBaseDatos: string): void {
        this.rutaBaseDatos = rutaBaseDatos;
    }

    public getNivelRegistro(): string {
        return this.nivelRegistro;
    }

    public setNivelRegistro(nivelRegistro: string): void {
        this.nivelRegistro = nivelRegistro;
    }
}
```

### Ejercicio 2: Control de Conexiones a la Base de Datos
Objetivo: Utilizar el patrón Singleton para manejar la conexión a la base de datos de inventario.
● Crear una clase ConexionDB que siga el patrón Singleton.
● Esta clase debe manejar la conexión a una base de datos ficticia, con propiedades como host, puerto y usuario.
● Implementar métodos para conectar y desconectar la base de datos.
● Garantizar que todas las partes de la aplicación utilicen la misma instancia de ConexionDB.

```typescript
class ConexionDB {
    private static instancia: ConexionDB;
    private host: string;
    private puerto: number;
    private usuario: string;

    private constructor() {
        this.host = 'localhost';
        this.puerto = 3000;
        this.usuario = 'root';
    }

    public static getInstance(): ConexionDB {
        if (!ConexionDB.instancia) {
            ConexionDB.instancia = new ConexionDB();
        }
        return ConexionDB.instancia;
    }

    public conectar(): void {
        console.log(`Conectando a ${this.host}:${this.puerto} con el usuario ${this.usuario}`);
    }

    public desconectar(): void {
        console.log('Desconectando...');
    }
}
```
***
***
## Patrón Factory Method

### Ejercicio 1: Crear Dispositivos de Entrada
Objetivo: Utilizar el patrón Factory Method para crear diferentes tipos de dispositivos de entrada.
● Crear una clase DispositivoEntradaFactory con un método crearDispositivo que, basado en el tipo de dispositivo ("Teclado", "Ratón", "Scanner"), devuelva una instancia de la clase adecuada.
● Crear clases específicas para cada tipo de dispositivo (Teclado, Ratón, Scanner), cada una con sus propias propiedades (Ej.: tipoConexion, marca).
● Integrar la creación de estos dispositivos en el sistema de inventario.

```typescript
interface DispositivoEntrada {
    conectar(): void;
    desconectar(): void;
}

class Teclado implements DispositivoEntrada {
    private tipoConexion: string;
    private marca: string;

    constructor(tipoConexion: string, marca: string) {
        this.tipoConexion = tipoConexion;
        this.marca = marca;
    }

    public conectar(): void {
        console.log(`Conectando el teclado ${this.marca} por ${this.tipoConexion}`);
    }

    public desconectar(): void {
        console.log('Desconectando el teclado');
    }
}

class Ratón implements DispositivoEntrada {
    private tipoConexion: string;
    private marca: string;

    constructor(tipoConexion: string, marca: string) {
        this.tipoConexion = tipoConexion;
        this.marca = marca;
    }

    public conectar(): void {
        console.log(`Conectando el ratón ${this.marca} por ${this.tipoConexion}`);
    }

    public desconectar(): void {
        console.log('Desconectando el ratón');
    }
}

class Scanner implements DispositivoEntrada {
    private tipoConexion: string;
    private marca: string;

    constructor(tipoConexion: string, marca: string) {
        this.tipoConexion = tipoConexion;
        this.marca = marca;
    }

    public conectar(): void {
        console.log(`Conectando el scanner ${this.marca} por ${this.tipoConexion}`);
    }

    public desconectar(): void {
        console.log('Desconectando el scanner');
    }
}

class DispositivoEntradaFactory {
    public crearDispositivo(tipo: string, tipoConexion: string, marca: string): DispositivoEntrada {
        switch (tipo) {
            case 'Teclado':
                return new Teclado(tipoConexion, marca);
            case 'Ratón':
                return new Ratón(tipoConexion, marca);
            case 'Scanner':
                return new Scanner(tipoConexion, marca);
            default:
                throw new Error('Tipo de dispositivo no soportado');
        }
    }
}
```
***
***
### Ejercicio 2: Fabricar Periféricos de Salida
Objetivo: Implementar el patrón Factory Method para crear diferentes periféricos de salida.
● Crear una clase PerifericoSalidaFactory con un método crearPeriferico que, basado en el tipo ("Monitor", "Impresora", "Proyector"), devuelva una instancia de la clase correspondiente.
● Crear clases específicas para cada tipo de periférico (Monitor, Impresora, Proyector), con propiedades particulares (Ej.: resolución para Monitor, tipoImpresión para Impresora).
● Asegurar que el factory maneje correctamente la creación de cada periférico según el tipo especificado.

```typescript
interface PerifericoSalida {
    conectar(): void;
    desconectar(): void;
}

class Monitor implements PerifericoSalida {
    private resolucion: string;
    private marca: string;

    constructor(resolucion: string, marca: string) {
        this.resolucion = resolucion;
        this.marca = marca;
    }

    public conectar(): void {
        console.log(`Conectando el monitor ${this.marca} con resolución ${this.resolucion}`);
    }

    public desconectar(): void {
        console.log('Desconectando el monitor');
    }
}

class Impresora implements PerifericoSalida {
    private tipoImpresion: string;
    private marca: string;

    constructor(tipoImpresion: string, marca: string) {
        this.tipoImpresion = tipoImpresion;
        this.marca = marca;
    }

    public conectar(): void {
        console.log(`Conectando la impresora ${this.marca} con tipo de impresión ${this.tipoImpresion}`);
    }

    public desconectar(): void {
        console.log('Desconectando la impresora');
    }
}

class Proyector implements PerifericoSalida {
    private resolucion: string;
    private marca: string;

    constructor(resolucion: string, marca: string) {
        this.resolucion = resolucion;
        this.marca = marca;
    }

    public conectar(): void {
        console.log(`Conectando el proyector ${this.marca} con resolución ${this.resolucion}`);
    }

    public desconectar(): void {
        console.log('Desconectando el proyector');
    }
}

class PerifericoSalidaFactory {
    public crearPeriferico(tipo: string, resolucion: string, marca: string): PerifericoSalida {
        switch (tipo) {
            case 'Monitor':
                return new Monitor(resolucion, marca);
            case 'Impresora':
                return new Impresora(resolucion, marca);
            case 'Proyector':
                return new Proyector(resolucion, marca);
            default:
                throw new Error('Tipo de periférico no soportado');
        }
    }
}
```
***
***
## Patrón Observer

### Ejercicio 1: Notificación de Mantenimiento Preventivo
Objetivo: Utilizar el patrón Observer para notificar al departamento de mantenimiento cuando un equipo alcanza un cierto tiempo de uso.
● Crear una clase DepartamentoMantenimiento que actúe como observador y reciba notificaciones cuando un equipo necesite mantenimiento preventivo.
● Implementar la clase Equipo que permita agregar observadores y notifique a los observadores cuando su tiempo de uso alcance un umbral definido.
● Definir propiedades como nombre, tipo, estado y tiempoUso en la clase Equipo.

```typescript
interface Observador {
    notificar(): void;
}

class DepartamentoMantenimiento implements Observador {
    private nombre: string;

    constructor(nombre: string) {
        this.nombre = nombre;
    }

    public notificar(): void {
        console.log(`El departamento de mantenimiento ${this.nombre} ha sido notificado`);
    }
}

class Equipo {
    private nombre: string;
    private tipo: string;
    private estado: string;
    private tiempoUso: number;
    private observadores: Observador[] = [];

    constructor(nombre: string, tipo: string, estado: string, tiempoUso: number) {
        this.nombre = nombre;
        this.tipo = tipo;
        this.estado = estado;
        this.tiempoUso = tiempoUso;
    }

    public agregarObservador(observador: Observador): void {
        this.observadores.push(observador);
    }

    public notificarObservadores(): void {
        for (const observador of this.observadores) {
            observador.notificar();
        }
    }

    public getTiempoUso(): number {
        return this.tiempoUso;
    }

    public setTiempoUso(tiempoUso: number): void {
        this.tiempoUso = tiempoUso;
        if (this.tiempoUso >= 1000) {
            this.notificarObservadores();
        }
    }
}
```

## Ejercicio 2: Actualización de Inventario en Tiempo Real
Objetivo: Implementar el patrón Observer para actualizar en tiempo real la interfaz de usuario cuando se realicen cambios en el inventario.
● Crear una clase InterfazUsuario que actúe como observador y actualice la visualización del inventario cuando se agreguen, eliminen o modifiquen equipos.
● Modificar la clase Inventario para que permita agregar observadores y notifique a los observadores correspondientes cuando ocurra un cambio en la lista de equipos.
● Asegurar que múltiples instancias de InterfazUsuario puedan recibir y manejar las notificaciones adecuadamente.

```typescript
class InterfazUsuario implements Observador {
    private nombre: string;

    constructor(nombre: string) {
        this.nombre = nombre;
    }

    public notificar(): void {
        console.log(`La interfaz de usuario ${this.nombre} ha sido actualizada`);
    }
}

class Inventario {
    private equipos: Equipo[] = [];
    private observadores: Observador[] = [];

    public agregarEquipo(equipo: Equipo): void {
        this.equipos.push(equipo);
        this.notificarObservadores();
    }

    public eliminarEquipo(equipo: Equipo): void {
        const index = this.equipos.indexOf(equipo);
        if (index !== -1) {
            this.equipos.splice(index, 1);
            this.notificarObservadores();
        }
    }

    public modificarEquipo(equipo: Equipo): void {
        this.notificarObservadores();
    }

    public agregarObservador(observador: Observador): void {
        this.observadores.push(observador);
    }

    public notificarObservadores(): void {
        for (const observador of this.observadores) {
            observador.notificar();
        }
    }
}
```
***
***
## Patrón Adaptador

### Ejercicio 1: Integrar Sistema de Facturación Antiguo
Objetivo: Implementar el patrón Adaptador para integrar un sistema antiguo de facturación con el nuevo sistema de inventario.
● Crear una clase FacturacionVieja que tenga métodos como crearFactura y obtenerFactura.
● Implementar una clase AdaptadorFacturacion que permita utilizar FacturacionVieja con la nueva interfaz IFacturacion, la cual requiere métodos como generarFactura y consultarFactura.
● Asegurar que las facturas generadas a través del adaptador sean compatibles con el nuevo sistema de inventario.

```typescript
interface IFacturacion {
    generarFactura(): void;
    consultarFactura(): void;
}

class FacturacionVieja {
    public crearFactura(): void {
        console.log('Creando factura antigua');
    }

    public obtenerFactura(): void {
        console.log('Obteniendo factura antigua');
    }
}

class AdaptadorFacturacion implements IFacturacion {
    private facturacionVieja: FacturacionVieja;

    constructor(facturacionVieja: FacturacionVieja) {
        this.facturacionVieja = facturacionVieja;
    }

    public generarFactura(): void {
        this.facturacionVieja.crearFactura();
    }

    public consultarFactura(): void {
        this.facturacionVieja.obtenerFactura();
    }
}
```

### Ejercicio 2: Compatibilidad con APIs Externas
Objetivo: Utilizar el patrón Adaptador para integrar una API externa de proveedores con el sistema de inventario existente.
● Crear una clase ProveedorExternoAPI que ofrezca métodos como fetchProductos y updateStock.
● Implementar una clase AdaptadorProveedor que permita utilizar ProveedorExternoAPI con la interfaz IProveedor, que requiere métodos como obtenerProductos y actualizarInventario.
● Asegurar que los datos obtenidos de la API externa se adapten correctamente al formato requerido por el sistema de inventario.

```typescript
interface IProveedor {
    obtenerProductos(): void;
    actualizarInventario(): void;
}

class ProveedorExternoAPI {
    public fetchProductos(): void {
        console.log('Obteniendo productos de la API externa');
    }

    public updateStock(): void {
        console.log('Actualizando stock en la API externa');
    }
}

class AdaptadorProveedor implements IProveedor {
    private proveedorExternoAPI: ProveedorExternoAPI;

    constructor(proveedorExternoAPI: ProveedorExternoAPI) {
        this.proveedorExternoAPI = proveedorExternoAPI;
    }

    public obtenerProductos(): void {
        this.proveedorExternoAPI.fetchProductos();
    }

    public actualizarInventario(): void {
        this.proveedorExternoAPI.updateStock();
    }
}
```
***
# :smiley:
carita con gafas:
:smiley: :sunglasses: :smile: :grinning: :grin: :joy: :rofl: :relaxed: :blush: :innocent: :slightly_smiling_face: :upside_down_face: :wink: :heart_eyes: :kissing_heart: :kissing: :kissing_smiling_eyes: :kissing_closed_eyes: :yum: :stuck_out_tongue_winking_eye: :stuck_out_tongue_closed_eyes: :stuck_out_tongue: :money_mouth_face: :hugs: :nerd_face: :sunglasses: :clown_face: :cowboy_hat_face: :smirk: :unamused: :relieved: :pensive: :sleepy: :drooling_face: :sleeping: :mask: :face_with_thermometer: :face_with_head_bandage: :nauseated_face: :vomiting: :sneezing_face: :hot_face: :cold_face: :woozy_face: :dizzy_face: :exploding_head: :cowboy_hat_face: :partying_face: :smiling_imp: :imp: :japanese_ogre: :japanese_goblin: :skull: :ghost: :alien: :robot: :poop: :smiley_cat: :smile_cat: :joy_cat: :heart_eyes_cat: :smirk_cat: :kissing_cat: :scream_cat: :crying_cat_face: :pouting_cat: :see_no_evil: :hear_no_evil: :speak_no_evil: :baby