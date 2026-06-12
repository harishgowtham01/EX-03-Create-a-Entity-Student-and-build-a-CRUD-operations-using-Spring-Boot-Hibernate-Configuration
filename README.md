# Ex No: 03 – Entity Student and Build CRUD Operations Using Spring Boot Hibernate Configuration

### Name: HARISH GOWTHAM E
### Register Number: 2305002009

## AIM

To develop a Spring Boot application that performs CRUD (Create, Read, Update, Delete) operations on a Student entity using Spring Data JPA (Hibernate).

---

## ALGORITHM

1. Create a Spring Boot project using Maven.
2. Add the required dependencies:

   * Spring Web
   * Spring Data JPA
   * H2 Database (or MySQL)
3. Configure the database connection and Hibernate properties in `application.properties`.
4. Create the `Student` entity class and annotate it with `@Entity`.
5. Create the `StudentRepository` interface by extending `JpaRepository`.
6. Create a service layer (`StudentService`) to implement business logic.
7. Create a REST controller (`StudentController`) to expose CRUD endpoints.
8. Run the Spring Boot application.
9. Test the CRUD operations using Postman, browser, or curl.
10. Verify the results.

---

## PROJECT STRUCTURE

```text
orm/
├── mvnw
├── mvnw.cmd
├── pom.xml
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/orm/
│   │   │       ├── OrmApplication.java
│   │   │       ├── Student.java
│   │   │       ├── StudentRepository.java
│   │   │       ├── StudentService.java
│   │   │       └── StudentController.java
│   │   └── resources/
│   │       └── application.properties
│   └── test/
│       └── java/
│           └── com/example/orm/
│               └── OrmApplicationTests.java
```

---

## PROGRAM

### application.properties

```properties
spring.application.name=orm

spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
```

### Student.java

```java
package com.example.orm;

import jakarta.persistence.Entity;
import jakarta.persistence.Id;

@Entity
public class Student {

    @Id
    private int id;
    private String name;
    private String department;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDepartment() {
        return department;
    }

    public void setDepartment(String department) {
        this.department = department;
    }
}
```

### StudentRepository.java

```java
package com.example.orm;

import org.springframework.data.jpa.repository.JpaRepository;

public interface StudentRepository extends JpaRepository<Student, Integer> {
}
```

### StudentService.java

```java
package com.example.orm;

import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class StudentService {

    private final StudentRepository repo;

    public StudentService(StudentRepository repo) {
        this.repo = repo;
    }

    // CREATE
    public Student saveStudent(Student s) {
        return repo.save(s);
    }

    // READ
    public List<Student> getAllStudents() {
        return repo.findAll();
    }

    // UPDATE
    public Student updateStudent(int id, Student s) {
        Student data = repo.findById(id).orElse(null);

        if (data != null) {
            data.setName(s.getName());
            data.setId(s.getId());
            data.setDepartment(s.getDepartment());
            return repo.save(data);
        }

        return null;
    }

    // DELETE
    public String deleteStudent(int id) {
        repo.deleteById(id);
        return "Deleted Successfully";
    }
}
```

### StudentController.java

```java
package com.example.orm;

import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
public class StudentController {

    private final StudentService service;

    public StudentController(StudentService service) {
        this.service = service;
    }

    // CREATE
    @PostMapping("/save")
    public Student saveStudent(@RequestBody Student s) {
        return service.saveStudent(s);
    }

    // READ
    @GetMapping("/all")
    public List<Student> getAllStudents() {
        return service.getAllStudents();
    }

    // UPDATE
    @PutMapping("/{id}")
    public Student updateStudent(@PathVariable int id,
                                 @RequestBody Student s) {
        return service.updateStudent(id, s);
    }

    // DELETE
    @DeleteMapping("/{id}")
    public String deleteStudent(@PathVariable int id) {
        return service.deleteStudent(id);
    }
}
```

### OrmApplication.java

```java
package com.example.orm;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class OrmApplication {

    public static void main(String[] args) {
        SpringApplication.run(OrmApplication.class, args);
    }
}
```

---

## OUTPUT

<img width="1459" height="680" alt="image" src="https://github.com/user-attachments/assets/42524d9f-4b89-4b7b-a8ba-45aef30d5cd8" />


## RESULT

Thus, a Spring Boot application that performs CRUD (Create, Read, Update, Delete) operations on a Student entity using Spring Data JPA (Hibernate) was developed and executed successfully.
