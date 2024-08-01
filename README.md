# FiveChan
___

## Propósito del Proyecto
FiveChan es una plataforma diseñada para la gestión eficiente de usuarios y temas, utilizando una arquitectura RESTful. El proyecto tiene como objetivo principal proporcionar una interfaz intuitiva y funcional para la creación, actualización y eliminación de usuarios y temas, facilitando la interacción entre los usuarios y el sistema.

## Funcionalidades

### Diagrama de Casos de Uso
![Diagrama de Casos de Uso](diagrama_casos_uso.png)

### Funcionalidades de Alto Nivel
1. **Gestión de Usuarios**:
   - Crear usuarios.
   - Actualizar información de usuarios.
   - Eliminar usuarios.
   - Buscar usuarios por diferentes criterios (ID, nombre de usuario, email).

2. **Gestión de Temas**:
   - Crear temas.
   - Actualizar información de temas.
   - Eliminar temas.
   - Buscar temas por diferentes criterios (ID de usuario, título).

### Prototipo (GUI)
![Prototipo de la GUI](prototipo_gui.png)

## Modelo de Dominio

### Diagrama de Clases
![Diagrama de Clases](diagrama_clases.png)

### Módulos
- **Usuario**: Maneja la información y operaciones relacionadas con los usuarios.
- **Tema**: Maneja la información y operaciones relacionadas con los temas.

## Arquitectura y Patrones

### Diagrama de Componentes o Paquetes
![Diagrama de Componentes](diagrama_componentes.png)

### Patrones Aplicados
- **MVC (Model-View-Controller)**: Separación de las responsabilidades en Modelos, Vistas y Controladores.
- **Repository Pattern**: Abstracción de la capa de acceso a datos.

## Prácticas de Codificación Limpia Aplicadas

### Nombrado Significativo
Los nombres de las clases y métodos son claros y descriptivos.

**Ejemplo:**
```java
public class UserEntity {
    @Id
    private UUID id;
    private String name;
    private String description;
    private String avatar;
    private String email;

    // Métodos
}
```
###Funciones Pequeñas y Centradas###
Cada método realiza una tarea específica, lo que facilita su comprensión y mantenimiento.

Ejemplo:
```java
@Override
@Transactional
public void save(User user) {
    UserEntity userEntity = UserEntity.fromDomain(user);
    entityManager.persist(userEntity);
}

```

##Estilos de Programación Aplicados##
###Cookbook###
Métodos en JpaUserRepository que siguen un patrón repetitivo y bien definido para las operaciones CRUD.

Ejemplo:
```java
@Override
@Transactional
public void save(User user) {
    UserEntity userEntity = UserEntity.fromDomain(user);
    entityManager.persist(userEntity);
}

@Override
public User findById(UUID id) {
    UserEntity userEntity = entityManager.find(UserEntity.class, id);
    return userEntity != null ? userEntity.toDomain() : null;
}

```
###Pipeline###
Procesamiento de datos a través de una serie de pasos o etapas.

Ejemplo:
```java
public User toDomain() {
    return new User(id, name, description, avatar, email);
}

public static UserEntity fromDomain(User user) {
    return new UserEntity(user.getId(), user.getName(), user.getDescription(), user.getAvatar(), user.getEmail());
}
```
###Restful###
Controlador REST para la gestión de usuarios.

Ejemplo:
```java
@RestController
@RequestMapping("/users")
public class UserController {
    private final UserService userService;

    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }

    @PostMapping
    public void createUser(@RequestBody UserDTO user) {
        UUID id = UUID.randomUUID();
        this.userService.createUser(id, user.getName(), user.getDescription(), user.getAvatar(), user.getEmail());
    }
}
```
##Principios SOLID Aplicados##
###Principio de Responsabilidad Única###
Cada clase tiene una única responsabilidad.

Ejemplo:
```java
@Service
public class UserService {
    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public void createUser(UUID id, String name, String description, String avatar, String email) {
        this.userRepository.save(new User(id, name, description, avatar, email));
    }
}
```
###Open/Closed Principle (OCP)###
Las clases son abiertas para extenderse pero cerradas para modificarse.
Ejemplo:
```java
public interface UserRepository {
    void save(User user);
    List<User> findAll();
    User findById(UUID id);
    User findByUsername(String username);
    User findByEmail(String email);
    void deleteById(UUID id);
    void updateById(UUID id, User user);
}
```
##Conceptos DDD Aplicados##
###Entidades###
Definen objetos con identidad única.

Ejemplo:   
```java

@Entity
public class UserEntity {
    @Id
    private UUID id;
    private String name;
    private String description;
    private String avatar;
    private String email;
}
```
###Objetos de Valor###
Definen objetos que no tienen identidad única pero contienen atributos significativos.

###Servicios de Dominio###
Encapsulan la lógica de negocio que no pertenece a una entidad o un objeto de valor.

###Repositorios###
Proporcionan una abstracción para la capa de acceso a datos.

Ejemplo:  
```java
@Repository
public class JpaUserRepository implements UserRepository {
    @PersistenceContext
    protected EntityManager entityManager;

    @Override
    @Transactional
    public void save(User user) {
        UserEntity userEntity = UserEntity.fromDomain(user);
        entityManager.persist(userEntity);
    }
}
  ```
###Arquitectura en Capas###
Separación del código en diferentes capas como presentación, aplicación, dominio y persistencia.

##Contribución##
Para contribuir al proyecto, por favor sigue estos pasos:

Haz un fork del repositorio.
Crea una nueva rama para tu funcionalidad (git checkout -b feature/nueva-funcionalidad).
Realiza tus cambios y realiza commits descriptivos (git commit -m 'Agregar nueva funcionalidad').
Empuja tus cambios a tu fork (git push origin feature/nueva-funcionalidad).
Abre un Pull Request en este repositorio.
##Licencia##
Este proyecto está bajo la Licencia MIT. Puedes ver más detalles en el archivo LICENSE.

##Agradecimientos##
Agradecemos a todos los integrantes del equipo por su esfuerzo y dedicación en el desarrollo de este proyecto.
