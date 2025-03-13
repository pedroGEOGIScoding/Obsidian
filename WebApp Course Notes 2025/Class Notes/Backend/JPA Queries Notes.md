Go to here for official docs:
[https://docs.spring.io/spring-data/jpa/reference/jpa.html]()
[https://docs.spring.io/spring-data/jpa/reference/repositories/query-methods-details.html]()

![[Architecture.jpg]]

## Multiplicity in Entity Relationships

Multiplicities are of the following types: one-to-one, one-to-many, many-to-one, and many-to-many:

- **One-to-one**: Each entity instance is related to a single instance of another entity. For example, to model a physical warehouse in which each storage bin contains a single widget, StorageBin and Widget would have a one-to-one relationship. One-to-one relationships use the javax.persistence.OneToOne annotation on the corresponding persistent property or field.

- **One-to-many**: An entity instance can be related to multiple instances of the other entities. A sales order, for example, can have multiple line items. In the order application, Order would have a one-to-many relationship with LineItem. One-to-many relationships use the javax.persistence.OneToMany annotation on the corresponding persistent property or field.

- **Many-to-one**: Multiple instances of an entity can be related to a single instance of the other entity. This multiplicity is the opposite of a one-to-many relationship. In the example just mentioned, the relationship to Order from the perspective of LineItem is many-to-one. Many-to-one relationships use the javax.persistence.ManyToOne annotation on the corresponding persistent property or field.

- **Many-to-many**: The entity instances can be related to multiple instances of each other. For example, each college course has many students, and every student may take several courses. Therefore, in an enrollment application, Course and Student would have a many-to-many relationship. Many-to-many relationships use the javax.persistence.ManyToMany annotation on the corresponding persistent property or field.
  
  ## Bidirectional vs. Unidirectional queries
  
  [https://docs.oracle.com/cd/E19798-01/821-1841/bnbqi/index.html]()
  
  #### Bidirectional Relationships
  
  In a bidirectional relationship, each entity has a relationship field or property that refers to the other entity. Through the relationship field or property, an entity class’s code can access its related object. If an entity has a related field, the entity is said to “know” about its related object. For example, if Order knows what LineItem instances it has and if LineItem knows what Order it belongs to, they have a bidirectional relationship.

Bidirectional relationships must follow these rules:

1. The inverse side of a bidirectional relationship must refer to its owning side by using the mappedBy element of the @OneToOne, @OneToMany, or @ManyToMany annotation. The mappedBy element designates the property or field in the entity that is the owner of the relationship.
2. The many side of many-to-one bidirectional relationships must not define the mappedBy element. The many side is always the owning side of the relationship.
3. For one-to-one bidirectional relationships, the owning side corresponds to the side that contains the corresponding foreign key.
4. For many-to-many bidirectional relationships, either side may be the owning side.
   
   #### Unidirectional Relationships
   
   In a unidirectional relationship, only one entity has a relationship field or property that refers to the other. For example, LineItem would have a relationship field that identifies Product, but Product would not have a relationship field or property for LineItem. In other words, LineItem knows about Product, but Product doesn’t know which LineItem instances refer to it.

#### Fetch Types

[https://thorben-janssen.com/entity-mappings-introduction-jpa-fetchtypes/]()
This is one of the most important decisions when defining your entity mapping. You can choose between EAGER and LAZY loading. The first one fetches an association immediately, and the other only when you use it. I explain both options in this article.

This default depends on the cardinality of the association. All to-one associations use **FetchType.EAGER** and all to-many associations **FetchType.LAZY**.

FetchType.**EAGER** tells Hibernate to get the associated entities with the initial query. This can look very efficient because it fetches all entities with only one query. But in most cases, it creates a huge overhead because Hibernate fetches these entities even if your business code doesn’t use them.

You can prevent this with FetchType.**LAZY**. It tells Hibernate to delay the initialization of the association until you access it in your business code.

#### Cascade Operations and Relationships

Entities that use relationships often have dependencies on the existence of the other entity in the relationship. For example, a line item is part of an order; if the order is deleted, the line item also should be deleted. This is called a cascade delete relationship.

![[CascadeOperations.png]]
