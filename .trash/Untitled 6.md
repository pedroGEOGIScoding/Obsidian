## Entities

An entity is a lightweight persistence domain object. Typically, an entity represents a table in a relational database, and each entity instance corresponds to a row in that table. The primary programming artifact of an entity is the entity class, although entities can use helper classes.

The persistent state of an entity is represented through either persistent fields or persistent properties. These fields or properties use object/relational mapping annotations to map the entities and entity relationships to the relational data in the underlying data store.

### Requirements for Entity Classes

An entity class must follow these requirements.

- The class must be annotated with the javax.persistence.Entity annotation.
    
- The class must have a public or protected, no-argument constructor. The class may have other constructors.
    
- The class must not be declared final. No methods or persistent instance variables must be declared final.
    
- If an entity instance is passed by value as a detached object, such as through a session bean’s remote business interface, the class must implement the Serializable interface.
    
- Entities may extend both entity and non-entity classes, and non-entity classes may extend entity classes.
    
- Persistent instance variables must be declared private, protected, or package-private and can be accessed directly only by the entity class’s methods. Clients must access the entity’s state through accessor or business methods.
    

### Persistent Fields and Properties in Entity Classes

The persistent state of an entity can be accessed through either the entity’s instance variables or properties. The fields or properties must be of the following Java language types:

- Java primitive types
    
- java.lang.String
    
- Other serializable types, including:
    
    - Wrappers of Java primitive types
        
    - java.math.BigInteger
        
    - java.math.BigDecimal
        
    - java.util.Date
        
    - java.util.Calendar
        
    - java.sql.Date
        
    - java.sql.Time
        
    - java.sql.TimeStamp
        
    - User-defined serializable types
    - byte[]
    - Byte[]
    - char[]
    - Character[]
        
- Enumerated types
- Other entities and/or collections of entities
- Embeddable classes
    
Entities may use persistent fields, persistent properties, or a combination of both. If the mapping annotations are applied to the entity’s instance variables, the entity uses persistent fields. If the mapping annotations are applied to the entity’s getter methods for JavaBeans-style properties, the entity uses persistent properties.

#### Persistent Fields

If the entity class uses persistent fields, the Persistence runtime accesses entity-class instance variables directly. All fields not annotated javax.persistence.Transient or not marked as Java transient will be persisted to the data store. The object/relational mapping annotations must be applied to the instance variables.

#### Persistent Properties

If the entity uses persistent properties, the entity must follow the method conventions of JavaBeans components. JavaBeans-style properties use getter and setter methods that are typically named after the entity class’s instance variable names. For every persistent property _property_ of type _Type_ of the entity, there is a getter method get_Property_ and setter method set_Property_. If the property is a Boolean, you may use is_Property_ instead of get_Property_. For example, if a Customer entity uses persistent properties and has a private instance variable called firstName, the class defines a getFirstName and setFirstName method for retrieving and setting the state of the firstName instance variable.

The method signature for single-valued persistent properties are as follows:

Type getProperty()
void setProperty(Type type)

The object/relational mapping annotations for persistent properties must be applied to the getter methods. Mapping annotations cannot be applied to fields or properties annotated @Transient or marked transient.

#### Using Collections in Entity Fields and Properties

Collection-valued persistent fields and properties must use the supported Java collection interfaces regardless of whether the entity uses persistent fields or properties. The following collection interfaces may be used:

- java.util.Collection
    
- java.util.Set
    
- java.util.List
    
- java.util.Map
    

If the entity class uses persistent fields, the type in the preceding method signatures must be one of these collection types. Generic variants of these collection types may also be used. For example, if it has a persistent property that contains a set of phone numbers, the Customer entity would have the following methods:

Set<PhoneNumber> getPhoneNumbers() { ... }
void setPhoneNumbers(Set<PhoneNumber>) { ... }

If a field or property of an entity consists of a collection of basic types or embeddable classes, use the javax.persistence.ElementCollection annotation on the field or property.

The two attributes of @ElementCollection are targetClass and fetch. The targetClass attribute specifies the class name of the basic or embeddable class and is optional if the field or property is defined using Java programming language generics. The optional fetch attribute is used to specify whether the collection should be retrieved lazily or eagerly, using the javax.persistence.FetchType constants of either LAZY or EAGER, respectively. By default, the collection will be fetched lazily.

The following entity, Person, has a persistent field, nicknames, which is a collection of String classes that will be fetched eagerly. The targetClass element is not required, because it uses generics to define the field.

@Entity
public class Person {
    ...
    @ElementCollection(fetch=EAGER)
    protected Set<String> nickname = new HashSet();
    ...
}

Collections of entity elements and relationships may be represented by java.util.Map collections. A Map consists of a key and a value.

When using Map elements or relationships, the following rules apply.

- The Map key or value may be a basic Java programming language type, an embeddable class, or an entity.
    
- When the Map value is an embeddable class or basic type, use the @ElementCollection annotation.
    
- When the Map value is an entity, use the @OneToMany or @ManyToMany annotation.
    
- Use the Map type on only one side of a bidirectional relationship.
    

If the key type of a Map is a Java programming language basic type, use the annotation javax.persistence.MapKeyColumn to set the column mapping for the key. By default, the name attribute of @MapKeyColumn is of the form _RELATIONSHIP-FIELD/PROPERTY-NAME__KEY. For example, if the referencing relationship field name is image, the default name attribute is IMAGE_KEY.

If the key type of a Map is an entity, use the javax.persistence.MapKeyJoinColumn annotation. If the multiple columns are needed to set the mapping, use the annotation javax.persistence.MapKeyJoinColumns to include multiple @MapKeyJoinColumn annotations. If no @MapKeyJoinColumn is present, the mapping column name is by default set to _RELATIONSHIP-FIELD/PROPERTY-NAME__KEY. For example, if the relationship field name is employee, the default name attribute is EMPLOYEE_KEY.

If Java programming language generic types are not used in the relationship field or property, the key class must be explicitly set using the javax.persistence.MapKeyClass annotation.

If the Map key is the primary key or a persistent field or property of the entity that is the Map value, use the javax.persistence.MapKey annotation. The @MapKeyClass and @MapKey annotations cannot be used on the same field or property.

If the Map value is a Java programming language basic type or an embeddable class, it will be mapped as a collection table in the underlying database. If generic types are not used, the @ElementCollection annotation’s targetClass attribute must be set to the type of the Map value.

If the Map value is an entity and part of a many-to-many or one-to-many unidirectional relationship, it will be mapped as a join table in the underlying database. A unidirectional one-to-many relationship that uses a Map may also be mapped using the @JoinColumn annotation.

If the entity is part of a one-to-many/many-to-one bidirectional relationship, it will be mapped in the table of the entity that represents the value of the Map. If generic types are not used, the targetEntity attribute of the @OneToMany and @ManyToMany annotations must be set to the type of the Map value.

#### Validating Persistent Fields and Properties

The Java API for JavaBeans Validation (Bean Validation) provides a mechanism for validating application data. Bean Validation is integrated into the Java EE containers, allowing the same validation logic to be used in any of the tiers of an enterprise application.

Bean Validation constraints may be applied to persistent entity classes, embeddable classes, and mapped superclasses. By default, the Persistence provider will automatically perform validation on entities with persistent fields or properties annotated with Bean Validation constraints immediately after the PrePersist, PreUpdate, and PreRemove lifecycle events.

Bean Validation constraints are annotations applied to the fields or properties of Java programming language classes. Bean Validation provides a set of constraints as well as an API for defining custom constraints. Custom constraints can be specific combinations of the default constraints, or new constraints that don’t use the default constraints. Each constraint is associated with at least one validator class that validates the value of the constrained field or property. Custom constraint developers must also provide a validator class for the constraint.

Bean Validation constraints are applied to the persistent fields or properties of persistent classes. When adding Bean Validation constraints, use the same access strategy as the persistent class. That is, if the persistent class uses field access, apply the Bean Validation constraint annotations on the class’s fields. If the class uses property access, apply the constraints on the getter methods.

[Table 9-2](https://docs.oracle.com/javaee/6/tutorial/doc/gircz.html#gkagk) lists Bean Validation’s built-in constraints, defined in the javax.validation.constraints package.

All the built-in constraints listed in [Table 9-2](https://docs.oracle.com/javaee/6/tutorial/doc/gircz.html#gkagk) have a corresponding annotation, _ConstraintName_.List, for grouping multiple constraints of the same type on the same field or property. For example, the following persistent field has two @Pattern constraints:

@Pattern.List({
    @Pattern(regexp="..."),
    @Pattern(regexp="...")
})

The following entity class, Contact, has Bean Validation constraints applied to its persistent fields.

@Entity
public class Contact implements Serializable {
    private static final long serialVersionUID = 1L;
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
    @NotNull
    protected String firstName;
    @NotNull
    protected String lastName;
    @Pattern(regexp="[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\\."
        +"[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*@"
        +"(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?",
             message="{invalid.email}")
    protected String email;
    @Pattern(regexp="^\\(?(\\d{3})\\)?[- ]?(\\d{3})[- ]?(\\d{4})$",
             message="{invalid.phonenumber}")
    protected String mobilePhone;
    @Pattern(regexp="^\\(?(\\d{3})\\)?[- ]?(\\d{3})[- ]?(\\d{4})$",
             message="{invalid.phonenumber}")
    protected String homePhone;
    @Temporal(javax.persistence.TemporalType.DATE)
    @Past
    protected Date birthday;
    ...
}

The @NotNull annotation on the firstName and lastName fields specifies that those fields are now required. If a new Contact instance is created where firstName or lastName have not been initialized, Bean Validation will throw a validation error. Similarly, if a previously created instance of Contact has been modified so that firstName or lastName are null, a validation error will be thrown.

The email field has a @Pattern constraint applied to it, with a complicated regular expression that matches most valid email addresses. If the value of email doesn’t match this regular expression, a validation error will be thrown.

The homePhone and mobilePhone fields have the same @Pattern constraints. The regular expression matches 10 digit telephone numbers in the United States and Canada of the form (_xxx_) _xxx_–_xxxx_.

The birthday field is annotated with the @Past constraint, which ensures that the value of birthday must be in the past.

### Primary Keys in Entities

Each entity has a unique object identifier. A customer entity, for example, might be identified by a customer number. The unique identifier, or **primary key**, enables clients to locate a particular entity instance. Every entity must have a primary key. An entity may have either a simple or a composite primary key.

Simple primary keys use the javax.persistence.Id annotation to denote the primary key property or field.

Composite primary keys are used when a primary key consists of more than one attribute, which corresponds to a set of single persistent properties or fields. Composite primary keys must be defined in a primary key class. Composite primary keys are denoted using the javax.persistence.EmbeddedId and javax.persistence.IdClass annotations.

The primary key, or the property or field of a composite primary key, must be one of the following Java language types:

- Java primitive types
    
- Java primitive wrapper types
    
- java.lang.String
    
- java.util.Date (the temporal type should be DATE)
    
- java.sql.Date
    
- java.math.BigDecimal
    
- java.math.BigInteger
    

Floating-point types should never be used in primary keys. If you use a generated primary key, only integral types will be portable.

A primary key class must meet these requirements.

- The access control modifier of the class must be public.
    
- The properties of the primary key class must be public or protected if property-based access is used.
    
- The class must have a public default constructor.
    
- The class must implement the hashCode() and equals(Object other) methods.
    
- The class must be serializable.
    
- A composite primary key must be represented and mapped to multiple fields or properties of the entity class or must be represented and mapped as an embeddable class.
    
- If the class is mapped to multiple fields or properties of the entity class, the names and types of the primary key fields or properties in the primary key class must match those of the entity class.
    

The following primary key class is a composite key, and the orderId and itemId fields together uniquely identify an entity:

public final class LineItemKey implements Serializable {
    public Integer orderId;
    public int itemId;

    public LineItemKey() {}

    public LineItemKey(Integer orderId, int itemId) {
        this.orderId = orderId;
        this.itemId = itemId;
    }

    public boolean equals(Object otherOb) {
        if (this == otherOb) {
            return true;
        }
        if (!(otherOb instanceof LineItemKey)) {
            return false;
        }
        LineItemKey other = (LineItemKey) otherOb;
        return (
                    (orderId==null?other.orderId==null:orderId.equals
                    (other.orderId)
                    )
                    &&
                    (itemId == other.itemId)
                );
    }

    public int hashCode() {
        return (
                    (orderId==null?0:orderId.hashCode())
                    ^
                    ((int) itemId)
                );
    }

    public String toString() {
        return "" + orderId + "-" + itemId;
    }
}

### Multiplicity in Entity Relationships

Multiplicities are of the following types: one-to-one, one-to-many, many-to-one, and many-to-many:

- **One-to-one**: Each entity instance is related to a single instance of another entity. For example, to model a physical warehouse in which each storage bin contains a single widget, StorageBin and Widget would have a one-to-one relationship. One-to-one relationships use the javax.persistence.OneToOne annotation on the corresponding persistent property or field.
    
- **One-to-many**: An entity instance can be related to multiple instances of the other entities. A sales order, for example, can have multiple line items. In the order application, Order would have a one-to-many relationship with LineItem. One-to-many relationships use the javax.persistence.OneToMany annotation on the corresponding persistent property or field.
    
- **Many-to-one**: Multiple instances of an entity can be related to a single instance of the other entity. This multiplicity is the opposite of a one-to-many relationship. In the example just mentioned, the relationship to Order from the perspective of LineItem is many-to-one. Many-to-one relationships use the javax.persistence.ManyToOne annotation on the corresponding persistent property or field.
    
- **Many-to-many**: The entity instances can be related to multiple instances of each other. For example, each college course has many students, and every student may take several courses. Therefore, in an enrollment application, Course and Student would have a many-to-many relationship. Many-to-many relationships use the javax.persistence.ManyToMany annotation on the corresponding persistent property or field.
    

### Direction in Entity Relationships

The direction of a relationship can be either bidirectional or unidirectional. A bidirectional relationship has both an owning side and an inverse side. A unidirectional relationship has only an owning side. The owning side of a relationship determines how the Persistence runtime makes updates to the relationship in the database.

#### Bidirectional Relationships

In a **bidirectional** relationship, each entity has a relationship field or property that refers to the other entity. Through the relationship field or property, an entity class’s code can access its related object. If an entity has a related field, the entity is said to “know” about its related object. For example, if Order knows what LineItem instances it has and if LineItem knows what Order it belongs to, they have a bidirectional relationship.

Bidirectional relationships must follow these rules.

- The inverse side of a bidirectional relationship must refer to its owning side by using the mappedBy element of the @OneToOne, @OneToMany, or @ManyToMany annotation. The mappedBy element designates the property or field in the entity that is the owner of the relationship.
    
- The many side of many-to-one bidirectional relationships must not define the mappedBy element. The many side is always the owning side of the relationship.
    
- For one-to-one bidirectional relationships, the owning side corresponds to the side that contains the corresponding foreign key.
    
- For many-to-many bidirectional relationships, either side may be the owning side.
    

#### Unidirectional Relationships

In a **unidirectional** relationship, only one entity has a relationship field or property that refers to the other. For example, LineItem would have a relationship field that identifies Product, but Product would not have a relationship field or property for LineItem. In other words, LineItem knows about Product, but Product doesn’t know which LineItem instances refer to it.

#### Queries and Relationship Direction

Java Persistence query language and Criteria API queries often navigate across relationships. The direction of a relationship determines whether a query can navigate from one entity to another. For example, a query can navigate from LineItem to Product but cannot navigate in the opposite direction. For Order and LineItem, a query could navigate in both directions because these two entities have a bidirectional relationship.

#### Cascade Operations and Relationships

Entities that use relationships often have dependencies on the existence of the other entity in the relationship. For example, a line item is part of an order; if the order is deleted, the line item also should be deleted. This is called a cascade delete relationship.

The javax.persistence.CascadeType enumerated type defines the cascade operations that are applied in the cascade element of the relationship annotations. [Table 32-1](https://docs.oracle.com/javaee/6/tutorial/doc/bnbqa.html#gjjnj) lists the cascade operations for entities.

Table 32-1 Cascade Operations for Entities

|   |   |
|---|---|
|Cascade Operation|Description|
|ALL|All cascade operations will be applied to the parent entity’s related entity. All is equivalent to specifying cascade={DETACH, MERGE, PERSIST, REFRESH, REMOVE}|
|DETACH|If the parent entity is detached from the persistence context, the related entity will also be detached.|
|MERGE|If the parent entity is merged into the persistence context, the related entity will also be merged.|
|PERSIST|If the parent entity is persisted into the persistence context, the related entity will also be persisted.|
|REFRESH|If the parent entity is refreshed in the current persistence context, the related entity will also be refreshed.|
|REMOVE|If the parent entity is removed from the current persistence context, the related entity will also be removed.|

Cascade delete relationships are specified using the cascade=REMOVE element specification for @OneToOne and @OneToMany relationships. For example:

@OneToMany(cascade=REMOVE, mappedBy="customer")
public Set<Order> getOrders() { return orders; }

#### Orphan Removal in Relationships

When a target entity in one-to-one or one-to-many relationship is removed from the relationship, it is often desirable to cascade the remove operation to the target entity. Such target entities are considered “orphans,” and the orphanRemoval attribute can be used to specify that orphaned entities should be removed. For example, if an order has many line items and one of them is removed from the order, the removed line item is considered an orphan. If orphanRemoval is set to true, the line item entity will be deleted when the line item is removed from the order.

The orphanRemoval attribute in @OneToMany and @oneToOne takes a Boolean value and is by default false.

The following example will cascade the remove operation to the orphaned order entity when the customer entity is deleted:

@OneToMany(mappedBy="customer", orphanRemoval="true")
public List<Order> getOrders() { ... }

### Embeddable Classes in Entities

Embeddable classes are used to represent the state of an entity but don’t have a persistent identity of their own, unlike entity classes. Instances of an embeddable class share the identity of the entity that owns it. Embeddable classes exist only as the state of another entity. An entity may have single-valued or collection-valued embeddable class attributes.

Embeddable classes have the same rules as entity classes but are annotated with the javax.persistence.Embeddable annotation instead of @Entity.

The following embeddable class, ZipCode, has the fields zip and plusFour:

@Embeddable
public class ZipCode {
  String zip;
  String plusFour;
...
}

This embeddable class is used by the Address entity:

@Entity
public class Address {
  @Id
  protected long id
  String street1;
  String street2;
  String city;
  String province;
  @Embedded
  ZipCode zipCode;
  String country;
...
}

Entities that own embeddable classes as part of their persistent state may annotate the field or property with the javax.persistence.Embedded annotation but are not required to do so.

Embeddable classes may themselves use other embeddable classes to represent their state. They may also contain collections of basic Java programming language types or other embeddable classes. Embeddable classes may also contain relationships to other entities or collections of entities. If the embeddable class has such a relationship, the relationship is from the target entity or collection of entities to the entity that owns the embeddable class.