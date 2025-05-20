
### Activitat 1 UF1844
##### Què és useContext en React i per què é important per gestionar dades globales? Quines són les seves característiques principals? Com es combina amb un custom hook? Què significa encapsular la lògica del context? Proporcioneu un example d'un custom hook que utilitzi useContext? 

El useContext de React es un hook de React que se utiliza para que los componentes de React puedan acceder a valores sin tener que pasarlos como props u otros métodos a otros componentes. Permite utilizar datos que se van a utilizar en muchos otros componente la aplicación y es mas fácil controlar el estado de estos valores.

const value = useContext(describe el Context aquí)

Evita el tener que pasar props de un componente a otro, simplifica el código y facilita la gestión de datos que se podrían llamar globales ya que cuando se actualicen en un componente, ese valor se actualizara también para todos los componentes evitando incoherencias.

Encapsular la lógica del context definido en la aplicación significa que se puede renderizar un componente fuera del componente principal de manera que el componente puede acceder a los valores del contexto proporcionado por el proveedor correspondiente. En definitiva, se encapsula el funcionamiento o logica del contexto creado para compartirla entre otros componentes de la aplicación. Al encapsular la lógica del context se crea un custom hook para poderlo utilizar o exportar a otros componentes.
Ejemplo:

**se crea el context y se llama al useContext**
import React, { createContext, useContext } from "react";

**En services see han agrupado la logica de varios componentes**
import Services from "./services";

**Aqui se realiza el hook customizado**
const AppServicesContext = createContext(Services);
export const useAppServices = () => {
  return useContext(AppServicesContext); 
  
**encapsulamiento y listo para ser llamado desde otros componentes**
export const AppServicesProvider = ({ children }) => {
  return (
    <AppServicesContext.Provider value={Services}>
      {children}
    </AppServicesContext.Provider>

### Activitat 2 UF1845
##### Què és la persistencia en JPA i per què és clau aplicacions SpringBoot? Quines són les seves principals característiques?Com s'aplica en la gestió d'entitats? Què significa mapear una entitat? Doneu un example práctic de com configurar una entitat amb anotacions JPA.

La persistencia en JPA es básicamente mantener los datos de una aplicación de manera segura y permanente. Da la capacidad de almacenar y recuperar datos y/o valores de forma persistente dentro de una base de datos y almacenarlos permanentemente. Incluso esos registros pueden ser accesibles sin correr la aplicación.

Esta característica es fundamental para que la aplicación sea consistente, segura ya que los datos no se van a perder nunca, se mantendrán seguros y accesibles cuando sea necesario sin peligro de alteraciones indeseadas.

En las entidades, JPA se aplica a base de anotaciones de JPA. Cada entidad de Java es una tabla de datos con sus registros y las anotaciones como @Id, @Entity, @Table, @Column dan las propiedades necesarias para establecer las relaciones entre las clases Java y sus correspondientes.

El mapeo de una entidad, o el atributo de una entidad es recorrer la columna de la tabla de datos de la entidad o clase Java basándose en un criterios de búsqueda para extraer los registros (rows) que corresponden con esa búsqueda. Como ejemplo básico de clase Flight de un ejercicio de clase:

@Entity # la declara como entidad
@AllArgsConstructor # que haga todos los constructores de la clase
@Setter # genera los setters de la clase
@Getter # genera los getters de la clase
public class Flight {

    @Id # es la primary key para mapear!!
    @UuidGenerator
    private String id;

    private String flightNumber;
    private String airline;
    private float flightDuration;
    private boolean isFlying;
    private String arrivalAirport;
    private String originAirport;

### Activitat 3 UF1846
##### Què és un framework full stack com Vaadin i per què és avantatjós per al desenvolupament web? Quines són les característiques clau de Flow i Hilla? Com simplifiquen el desenvolupament? Què significa integració tipus-segura? Doneu un example d'una aplicació desenvolupada amb Flow o Hilla.

El framework de Vaadin permite generar una aplicación web con un único esqueleto de código donde conviven tanto el front end como el back end. 
La ventaja principal es que simplifica el código al evitar llamadas a API's etc ya que tanto el Front y el Back están integrados en un único bloque de programación.

La característica clave de Flow es que todo se programa en Java, mientras que con Hilla en front es con TypeScript y el back con Java.

Trabajar con TypeScript, es decir un código fuertemente tipado da una robustez, estabilidad y seguridad a la aplicación que con JavaScript no se daría.

Con Vaadin se puede realizar todo con Java lo que disminuye conflictos entre el back y el front. Ofrece una extensa biblioteca de componentes de interfaz de usuario lo que hace ahorrar mucho tiempo de programación. También facilita que la aplicación se pueda escalar manteniendo su integridad. Las aplicaciones web de Lufthansa o el banco HSBC han sido construidas con Vaadin.

