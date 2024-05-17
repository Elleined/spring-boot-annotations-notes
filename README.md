# spring-boot-annotations-notes

# Spring annotations
## 1. @Component
The `@Component` annotation indicates that an annotated class is a "component". Such classes are considered as candidates for auto-detection when using annotation-based configuration and classpath scanning.


**Usage Example:**
```java
@Component
public class MyComponent {
    // class body
}
```


## 2. @Primary
The `@Primary` annotation indicates that a bean should be given preference when multiple beans qualify for autowiring. It can be used in conjunction with `@Component`, `@Service`, `@Repository`, etc.


**Usage Example:**
```java
@Component
@Primary
public class PrimaryBean implements MyService {
    // class body
}
```


## 3. @Required
The `@Required` annotation applies to bean setter methods. It indicates that the property must be set during configuration; otherwise, the container will throw a `BeanInitializationException`.


**Usage Example:**
```java
public class MyBean {
    private String myProperty;


    @Required
    public void setMyProperty(String myProperty) {
        this.myProperty = myProperty;
    }
}
```


## 4. @Qualifier
The `@Qualifier` annotation is used in conjunction with `@Autowired` to provide more control over dependency injection, specifying which bean should be injected when multiple candidates exist.


**Usage Example:**
```java
@Component
public class MyService {
    private final AnotherService anotherService;


    @Autowired
    public MyService(@Qualifier("specificBean") AnotherService anotherService) {
        this.anotherService = anotherService;
    }
}
```


## 5. @Autowired
The `@Autowired` annotation is used for automatic dependency injection. It can be applied to constructors, fields, and setter methods.


**Usage Example:**
```java
@Component
public class MyComponent {
    @Autowired
    private MyDependency myDependency;
    
    // class body
}
```


## 6. @Bean
The `@Bean` annotation is used on methods in a `@Configuration` class to define a bean. The method's return type is the type of the bean, and the method's name is the name of the bean.


**Usage Example:**
```java
@Configuration
public class MyConfig {
    
    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
}
```


## 7. @Configuration
The `@Configuration` annotation indicates that a class declares one or more `@Bean` methods and may be processed by the Spring container to generate bean definitions and service requests for those beans at runtime.


**Usage Example:**
```java
@Configuration
public class AppConfig {
    
    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
}
```


## 8. @PropertySource
The `@PropertySource` annotation is used to declare a set of properties to be loaded from a specified resource, typically a properties file.


**Usage Example:**
```java
@Configuration
@PropertySource("classpath:app.properties")
public class AppConfig {
    // class body
}
```


## 9. @Value
The `@Value` annotation is used for expression-driven dependency injection. It can inject values from property files or system properties.


**Usage Example:**
```java
@Component
public class MyComponent {


    @Value("${my.property}")
    private String myProperty;


    // class body
}
```


## 10. @Scope
The `@Scope` annotation is used to specify the scope of a bean. Common scopes include `singleton` (default), `prototype`, `request`, `session`, and `application`.


**Usage Example:**
```java
@Component
@Scope("prototype")
public class PrototypeBean {
    // class body
}
```
 
# Spring MVC Annotations
### `@Controller`
Indicates that a class serves as a web controller, typically used for handling web requests and returning views.


**Code Sample:**
```java
@Controller
public class HomeController {
    
    @RequestMapping("/")
    public String home() {
        return "home";  // returns home.jsp or home.html
    }
}
```


### `@Service`
Indicates that a class provides business logic or service functions.


**Code Sample:**
```java
@Service
public class UserService {
    
    public String getUser() {
        return "John Doe";
    }
}
```


### `@RestController`
A specialized version of `@Controller` that includes `@ResponseBody` by default. Used for RESTful web services.


**Code Sample:**
```java
@RestController
public class UserRestController {
    
    @GetMapping("/user")
    public User getUser() {
        return new User("John", "Doe");
    }
}
```


### `@ControllerAdvice`
Allows centralized exception handling across multiple `@Controller` classes.


**Code Sample:**
```java
@ControllerAdvice
public class GlobalExceptionHandler {


    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleException(Exception e) {
        return new ResponseEntity<>(e.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```


### `@ExceptionHandler`
Defines a method to handle specific exceptions thrown by controller methods.


**Code Sample:**
```java
@RestController
public class ProductController {
    
    @GetMapping("/product/{id}")
    public Product getProduct(@PathVariable int id) {
        if (id <= 0) {
            throw new IllegalArgumentException("Invalid product ID");
        }
        return new Product(id, "Product Name");
    }
    
    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> handleIllegalArgumentException(IllegalArgumentException e) {
        return new ResponseEntity<>(e.getMessage(), HttpStatus.BAD_REQUEST);
    }
}
```


### `@PathVariable`
Extracts values from the URI path.


**Code Sample:**
```java
@RestController
public class ItemController {
    
    @GetMapping("/item/{id}")
    public Item getItem(@PathVariable int id) {
        return new Item(id, "Item Name");
    }
}
```


### `@RequestParam`
Extracts query parameters from the URL.


**Code Sample:**
```java
@RestController
public class SearchController {
    
    @GetMapping("/search")
    public String search(@RequestParam String query) {
        return "Search results for: " + query;
    }
}
```


### `@RequestPart`
Handles file uploads and data submitted as part of a multipart request.


**Code Sample:**
```java
@RestController
public class FileUploadController {
    
    @PostMapping("/upload")
    public String handleFileUpload(@RequestPart("file") MultipartFile file) {
        return "File uploaded: " + file.getOriginalFilename();
    }
}
```


### `@RequestMapping`
Maps web requests to specific handler classes or methods.


**Code Sample:**
```java
@RestController
@RequestMapping("/api")
public class ApiController {
    
    @GetMapping("/hello")
    public String hello() {
        return "Hello, API!";
    }
}
```


### `@ModelAttribute`
Binds a method parameter or return value to a named model attribute.


**Code Sample:**
```java
@Controller
public class ProfileController {
    
    @ModelAttribute("user")
    public User getUser() {
        return new User("John", "Doe");
    }
    
    @GetMapping("/profile")
    public String profile() {
        return "profile";  // returns profile.jsp or profile.html
    }
}
```


### `@SessionAttribute`
Retrieves session attributes.


**Code Sample:**
```java
@RestController
public class SessionController {
    
    @GetMapping("/session")
    public String getSessionAttribute(@SessionAttribute("user") User user) {
        return "User: " + user.getName();
    }
}
```


### `@ResponseBody`
Indicates that the return value of a method should be used as the response body.


**Code Sample:**
```java
@Controller
public class MessageController {
    
    @GetMapping("/message")
    @ResponseBody
    public String getMessage() {
        return "Hello, World!";
    }
}
```


### `@RequestBody`
Maps the body of the web request to a Java object.


**Code Sample:**
```java
@RestController
public class OrderController {
    
    @PostMapping("/order")
    public String createOrder(@RequestBody Order order) {
        return "Order created for: " + order.getProductName();
    }
}
```


### `@PostMapping`
Shortcut for `@RequestMapping(method = RequestMethod.POST)`.


**Code Sample:**
```java
@RestController
public class CreateController {
    
    @PostMapping("/create")
    public String createItem(@RequestBody Item item) {
        return "Item created: " + item.getName();
    }
}
```


### `@GetMapping`
Shortcut for `@RequestMapping(method = RequestMethod.GET)`.


**Code Sample:**
```java
@RestController
public class ReadController {
    
    @GetMapping("/read")
    public String readItem() {
        return "Reading item";
    }
}
```


### `@PutMapping`
Shortcut for `@RequestMapping(method = RequestMethod.PUT)`.


**Code Sample:**
```java
@RestController
public class UpdateController {
    
    @PutMapping("/update")
    public String updateItem(@RequestBody Item item) {
        return "Item updated: " + item.getName();
    }
}
```


### `@DeleteMapping`
Shortcut for `@RequestMapping(method = RequestMethod.DELETE)`.


**Code Sample:**
```java
@RestController
public class DeleteController {
    
    @DeleteMapping("/delete/{id}")
    public String deleteItem(@PathVariable int id) {
        return "Item deleted with ID: " + id;
    }
}
```


### `@PatchMapping`
Shortcut for `@RequestMapping(method = RequestMethod.PATCH)`.


**Code Sample:**
```java
@RestController
public class PatchController {
    
    @PatchMapping("/patch/{id}")
    public String patchItem(@PathVariable int id, @RequestBody Item item) {
        return "Item patched with ID: " + id + ", Name: " + item.getName();
    }
}
```


### `@CrossOrigin`
Enables cross-origin requests.


**Code Sample:**
```java
@RestController
@CrossOrigin(origins = "http://example.com")
public class CorsController {
    
    @GetMapping("/cors")
    public String corsEnabled() {
        return "CORS enabled";
    }
}
```


# Spring Data JPA Annotations
1. `@Query`: Used to define custom JPQL (Java Persistence Query Language) queries or native SQL queries.


```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    @Query("SELECT u FROM User u WHERE u.age > :age")
    List<User> findByAgeGreaterThan(@Param("age") int age);
}
```


2. `@Param`: Used to bind parameters in a Spring Data query.


```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    @Query("SELECT u FROM User u WHERE u.name = :name")
    User findByName(@Param("name") String name);
}
```


3. `@Modifying`: Marks a method as modifying query. Useful for update or delete operations in JPQL.


```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    @Modifying
    @Query("UPDATE User u SET u.age = :age WHERE u.id = :id")
    int updateUserAge(@Param("age") int age, @Param("id") Long id);
}
```


4. `@Transactional`: Specifies that a method should run within a transaction.


```java
@Service
@Transactional
public class UserService {
    @Autowired
    private UserRepository userRepository;


    public void updateUserAge(Long id, int newAge) {
        userRepository.updateUserAge(newAge, id);
    }
}
```


5. `@PersistenceContext`: Injects a `EntityManager` into a Spring-managed bean.


```java
@Repository
public class UserRepositoryImpl implements UserRepositoryCustom {
    @PersistenceContext
    private EntityManager entityManager;


    // Custom methods using EntityManager
}
```


6. `@Repository`: Indicates that an annotated class is a repository, typically used for database operations.


```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // Methods for database operations
}
```


7. `@Id`: Marks a field as the primary key of an entity.


```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    // Other fields and methods
}
```


8. `@GeneratedValue`: Specifies the generation strategy for the primary key.


```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```


9. `@SequenceGenerator`: Defines a sequence generator for the primary key.


```java
@Id
@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "user_seq")
@SequenceGenerator(name = "user_seq", sequenceName = "user_sequence", allocationSize = 1)
private Long id;
```


10. `@Entity`: Marks a class as an entity to be managed by the JPA provider.


```java
@Entity
public class User {
    // Entity class definition
}
```


11. `@Table`: Specifies the table to map the entity.


```java
@Entity
@Table(name = "users")
public class User {
    // Entity class definition
}
```


12. `@Column`: Specifies the mapping for a persistent property or field.


```java
@Column(name = "user_name", nullable = false)
private String name;
```


13. `@Enumerated`: Maps an enum type to a database column.


```java
public enum UserType {
    ADMIN,
    USER
}


@Entity
public class User {
    @Enumerated(EnumType.STRING)
    private UserType userType;
}
```


14. `@ForeignKey`: Specifies the foreign key constraint for a column.


```java
@Column(name = "address_id")
@ForeignKey(name = "fk_user_address")
private Long addressId;
```


15. `@JoinColumn`: Specifies the joining column for a relationship.


```java
@OneToOne
@JoinColumn(name = "address_id")
private Address address;
```


16. `@JoinTable`: Specifies the joining table for a many-to-many relationship.


```java
@ManyToMany
@JoinTable(
    name = "user_role",
    joinColumns = @JoinColumn(name = "user_id"),
    inverseJoinColumns = @JoinColumn(name = "role_id"))
private Set<Role> roles;
```


17. `@Inheritance`: Specifies the inheritance strategy for an entity class hierarchy.


```java
@Entity
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
@DiscriminatorColumn(name = "user_type")
public class User {
    // Common fields
}
```


18. `@OneToOne`: Defines a one-to-one relationship between two entities.


```java
@Entity
public class User {
    @OneToOne(mappedBy = "user")
    private UserProfile userProfile;
}
```


19. `@OneToMany`: Defines a one-to-many relationship between two entities.


```java
@Entity
public class User {
    @OneToMany(mappedBy = "user")
    private List<Order> orders;
}
```


20. `@ManyToOne`: Defines a many-to-one relationship between two entities.


```java
@Entity
public class Order {
    @ManyToOne
    @JoinColumn(name = "user_id")
    private User user;
}
```


21. `@ManyToMany`: Defines a many-to-many relationship between two entities.


```java
@Entity
public class User {
    @ManyToMany(mappedBy = "users")
    private Set<Role> roles;
}
```


22. `@Embedded`: Specifies a persistent field or property of an entity whose value is an instance of an embeddable class.


```java
@Entity
public class User {
    @Embedded
    private Address address;
}
```


23. `@Embeddable`: Specifies a class whose instances are stored as part of an owning entity.


```java
@Embeddable
public class Address {
    private String street;
    private String city;
}
```


24. `@Transient`: Specifies that a field should not be persisted to the database.


```java
@Transient
private String temporaryData;
```


# Spring test annotations
`@DataJpaTest`
`@SpringBootTest`

# Spring boot enablers annotations
`@EnableScheduling`
`@EnableTransactionManagement`
`@EnableDiscoveryClient`
`@EnableEurekaServer`
`@EnableFeignClient`

# Kafka annotation
`@KafkaListener` 

# Scheduler annotation
`@Scheduled`

# Execute SQL script
`@Sql`
`@SqlGroup` 
`@SqlConfig`

# Hibernate Validator annotations
`@Positive`
`@Email`
`@NotBlank`
`@NotNull`
`@NotEmpty`
`@Negative`
`@Valid`

# Jackson Library annotations
`@JsonProperty` 
`@JsonPropertyOrder`
`@JsonIgnore`

# JUnit annotations
`@Test`
`@BeforeEach`
`@AfterEach`
`@BeforeAll`
`@AfterAll`
`@Disabled`
`@Tag`
`@DisplayName`


