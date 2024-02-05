# Telusko Java, Spring and Microservices Notes

## CORE JAVA NOTES

For Java -> JDK->JRE->JVM

### Some common points to keep in mind

- Static keyword is used to make the variale or method common for the class and it can be accessed by class name, all the class-objects shares the same value.

- Static block will run only once irrespective of number times the object has been created.

- super() method is present inside the constructor of a class and used to call a constructor of a parent class depending upon the usecase.

- this() method can be used inside the constructor of a class to call some other constructor of a same class depending on the usecase.

- Every class in Java Extends the Object class by default.

- final keyword - IN JAVA
  - used to make the veriable constant.
  - used with class to stop it's inheritance.
  - used with methods to stop method overiding

### OOPS CONCEPT

#### Access Modifiers

- Public - From anywhere.
- Private - In the same class.
- Protected - In same package or Subclass(from same or diffrent package)
- Default - Same Package (In case of JAVA).

#### Abstarct Keyword

- Use to declare the methods (which does not have it's implementation) and class "abstract" so that the child class(concrete class) can implement this.
- These methods can only be called using child class object.
- It's the special case of method overiding.

#### Inheritance

- Child class has (is a) relation with a parent class. ex. An AdvanceCalc is a Clac where Clac is the parent class and AdvaceClac is a child class.

  - TYPES (SINGLE, MULTIPLE, MULTI-LEVEL). IN JAVA, MUTIPLE INHERITANCE DOES NOT WORK

  #### Method Overloading

  - Same method name with diff number of arguments or diff argument types.

  #### Method Overiding

  - Same method name, and arguments with diff menthod signature (works in case of inharitance where child class has same method as parent but with diff implimentation)

#### Polymorphism

- When your code show many Behavior depending on the input.
  - Function overloading promotes compile time polymorphism.
  - Function overiding promotes run time polymorphism.

#### Encapsulation

- The process of combining data and codes into a single unit. It helps keep code organized, as well as helps prevent accidental data manipulation and changes.
- We can use access modifiers to provide proper encapsulation.

#### Abstraction

- Abstraction is a process of hiding implementation details and exposes only the functionality to the user.
- In abstraction, we deal with ideas and not events. This means the user will only know “what it does” rather than “how it does”.

  #### Interface

  - Use to implement abstraction, It's not considered as class, every method inside it is by default public and abstract and every veriable is final and static.
  - Ulike abstract classes, a class can implements mutiple interfaces.
  - Types of Interface:
    - Normal: Has 2 or more methods
    - Function(SAM): Has only 1 method (lamba expression only work with functional interface)
    - Marker: Has no method
  - Important point:
    - class - class -> extends
    - class - interface -> implements
    - interface - interface -> extends

### Exception Handling

```
try {

}
catch(Exception e) {

}
```

- The code after the catch block will run as usual in case of exception.
- 'finally' keyword is used with the try to release the resources or any connection made, this will be called irrespective of any exception.

### Threads

- Can be created by extending Thread class or Runnable interface.
- The child or concrete class should implement run() method.
- State of threads - New -> Runnable -> Running -> (Waiting and Dead);
- Synchronized is used to make the mutable part of code thread safe and refrain it from race condition.
  <br> Code Snippit

  ```
  Runnable a = new A(); // (A) should implement run method

  Thread t1 = new Thread(a);

  t1.start();

  ```

## JDBC (Java DataBase Connectivity)

### Steps to make a JDBC Connection

1. Import the package [`java.sql.*`]
2. Load and Register the Driver [`Class.forName(com.mySql.jdbc.Driver)`]
3. Etablish Connection [`Connection con = DriverManager.getConnection(url,username,password)`]
4. Create Statement [`Statement st = con.createStatement()` || `con.preparedStatement(sqlQuery)`]
5. Execute Queries [`ResultSet rs = st.executeQuery('sqlQuery')` || `st.executeUpadate()`]
6. Process Result [`rs = rs.next()`, We can get the data using `rs.getString()` or any other method depending on data type]
7. Close the connection [`st.close() and con.close()`]

## Java Servlets

#### Code Example

```
@webServlet("/path")
public class DemoServlet extends HttpServlet{
  public void doGet(HttpServletRequest req,HttpServletResponse res)
  throws ServletException,IOException
  {
    res.setContentType("text/html");
    PrintWriter pw=res.getWriter();//get the stream to write the data
    pw.println("<html><body>");
    pw.println("Welcome to servlet");
    pw.println("</body></html>");

    pw.close();
  }
}
```

- Servlet is a Java Class used to write a server side logic and can be used to server dynamic web pages.
- it provides http request specific method such as service(), doGet() and doPost().
- The mapping of the servlets is desicribed in `web.xml`(Deploment descriptor).

  ```
    <web-app>

    <servlet>
    <servlet-name>sonoojaiswal</servlet-name>
    <servlet-class>DemoServlet</servlet-class>
    </servlet>

    <servlet-mapping>
    <servlet-name>sonoojaiswal</servlet-name>
    <url-pattern>/welcome</url-pattern>
    </servlet-mapping>

    </web-app>
  ```

- We can call another servlet from a servlet by dispatching using `RequestDispather rd = req.getRequestDispather("serveletName to dispatch the req") and rd.forward(req,res)` or by redirecting using `res.sendRedirect("servletName to dispatch the req")`.
- We can also use annotation `@webServlet(/path)` intead of using the `web.xml` to mark the class as servlet.

## JSP (Java Server Pages)

- JSP very much similar to the HTML with the special features of writing Java inside it using the below Tags.
- Some commonly used tags:
  - `<%  %>` (Scriplet) - Same as writing inside the `service()` of the Servlet. This tag has the access for all the objects we used in servlet like req, res, out etc.
  - `<%! %>` (Declaration) - Same declaring veriables inside the servlet but outside the `service()`.
  - `<% @page attribute="value" %>` (Directive) - Can be used for importing packages inside JSP.
  - `<% @Include file="fileName" %>` (Directive) - Can be used for importing other JSP files.
  - `<%= %>` (Expression) - For printing the value, can be used in place of out object.
- we can also use JSTL(The Jakarta Standard Tag Library (JSTL; formerly JavaServer Pages Standard Tag Library)) that provides tags to control the JSP page behavior.
- It is good practice to use JSP as a view and write the business logic in servlet.
- We can use filter Classes or multiple filters to perform filtering before hitting the servlet(JSP). `wb.xml` or `@WebFilter("/path")` can be used to define the mapping.

  #### Code Example

  ```
  @WebFilter("/path")
  public class FilterJSP implements Filter {

    public void doFilter(ServletRequest req, ServletResponse res, FilterChain fltrchain)
      throws java.io.IOException, ServletException {

     // perform filtering operation

      // Pass data to the filter chain
      fltrchain.doFilter(req, res);
    }
    // This is the last life cycle method
    public void destroy() {
      // This is the clean-up life cycle method
    }
  }
  ```

## Hibernate (ORM Framework)

#### Code Example

```
public class YourDAOClass {
  public void saveEntity(YourEntityClass entity) {
    // Create a Hibernate configuration
    Configuration configuration = new Configuration().configure();

    // Create a SessionFactory
    SessionFactory sessionFactory = configuration.buildSessionFactory();
    // Open a session
    Session session = sessionFactory.openSession();
    // Begin a transaction
    Transaction transaction = session.beginTransaction();

    // Save the entity
    session.save(entity);

    // Commit the transaction
    transaction.commit();
  }
}

```

- This is the Object Relational Mapping framework that allows you to map the Java objects(POJO) to DB table or schema's.
- It allows us to store and retrieve the data without write the DB queries.
- It also provides the concept of Lazy vs Eager fetch in case of relational mapping, its by default lazy and we can set it as Eager by passinh an argument to the mapping annotation `@OneToMany(fetch=FetchType.EAGER)`.
- It also provides cahing mechanisms, first level is by default, that is till the time the session get closed. and for 2nd level chaching we have to do some setups.
- It has its own query language called HQL to work with thw DB.
- Hybernate Object State/Persistence Life Cycle
  - New
  - Transient
  - Persistent
  - Detached
  - Removed
  - Grabage
- Some Impotant Annotation in case of POJO:

  - `@Entity` is use to make a class POJO, and it should has at least one `@Id` as primary key.
  - `@Table("custom name")` To give a custom name to our table inside DB by adding to our POJO class.
  - `@Transient` is used to stop the filed from getting stored in the DB.
  - `@Embeddable` is used to store POJO inside another POJO, Make the secondary class as Embeddable.
  - `@Cacheable` to make the object cacheable.
  - we can use diffrent mapping relation like `@OneToOne` and `@OneToMany` to define the relation between two Tables or POJO's.

## JPA (JAVA PERSISTANCE API)

#### Code Example

```
import javax.persistence.*;

@Entity
@Table(name = "employees")
public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "first_name")
    private String firstName;

    @Column(name = "last_name")
    private String lastName;

    // getters and setters
}

public class Main {
    public static void main(String[] args) {
        EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory("YourPersistenceUnitName");
        EntityManager entityManager = entityManagerFactory.createEntityManager();

        entityManager.getTransaction().begin();

        Employee employee = new Employee();
        employee.setFirstName("John");
        employee.setLastName("Doe");
        entityManager.persist(employee);

        entityManager.getTransaction().commit();

        entityManager.close();
        entityManagerFactory.close();
    }
}

```

- JPA is a Java specification for object-relational mapping and provides a standard way to interact with databases in Java applications.
- If you use Hibernate alone, you are tied to Hibernate-specific APIs. With JPA, you can switch to a different JPA implementation (like EclipseLink or DataNucleus) without changing your code significantly.

## Spring and Spring boot

<a href="https://ibb.co/WzLttzh"><img src="https://i.ibb.co/QH4ddHg/Spring-vs-Spring-Boot-Java-Guides-Net.png" alt="Spring-vs-Spring-Boot-Java-Guides-Net" width="900px" height="700px"></a>
