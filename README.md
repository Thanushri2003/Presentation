**Hibernate Program**

Files:
1.Hibernate.cfg.xml
2.pom.xml
3.pojo file(eg:Student.java)
4.operations file(CRUD java files)

**Need for Hibernate:**
Hibernate is a popular ORM (Object-Relational Mapping) framework for Java that simplifies database interactions. It eliminates the need for complex JDBC code and improves performance, maintainability, and portability.
Maps Java objects to database tables using annotations or XML.


**1.hibernate.cfg.xml**

<?xml version="1.0" encoding="UTF-8"?>
<hibernate-configuration>  //Root element of the Hibernate configuration file. 
<session-factory>         //Defines a session factory to manage database connections,created once per database and is thread-safe.
<property name="connection.driver_class">com.mysql.cj.jdbc.Driver</property>
<property name="connection.url">jdbc:mysql://localhost:3306/**db name**</property>
<property name="connection.username">root</property>
<property name="connection.password">Password@12</property>
<property name="dialect">org.hibernate.dialect.MySQL8Dialect</property> // This tells Hibernate that the application is using MySQL 8 as the database.
<property name="hbm2ddl.auto">update</property>//
<property name="show_sql">true</property> //Enables logging of SQL queries executed by Hibernate.
<property name="format_sql">true</property> //Formats SQL queries to improve readability in logs.
<mapping class="com.mphasis.Hibernate.Student"/> //Maps the Student entity to a corresponding database table.
</session-factory>
</hibernate-configuration>


Hibernates executed query looks like:
**eg:**
To print the details of the student with the specified id;

Hibernate: 
    select
        student0_.id as id1_0_0_,
        student0_.city as city2_0_0_,
        student0_.name as name3_0_0_ 
    from
        Student student0_ 
    where
        student0_.id=?


**2.pom.xml**
pom.xml (Project Object Model) is the configuration file for Maven.It defines project dependencies, build settings, plugins, and configurations.

<dependencies>
	<!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-core -->
	
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>5.4.5.Final</version>
</dependency>

 <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.33</version>
 </dependency>
 
<dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
</dependency>
</dependencies>



**3.Pojo file(Student.java)**

@Entity
public class Student {

	@Id
	private int id;
	private String name;
	private String city;}
 
Methods:
 1.getmethod
 2.setmethod
 3.constructors


 ** 4.Crud operation file:**

public class Operations {
	public static void main(String[] args){
	System.out.println("Project started..");

	Configuration cfg = new Configuration(); 		 //Creates a Configuration object, which is the starting point of Hibernate.
	cfg.configure();					//Reads the hibernate.cfg.xml file.
	SessionFactory factory = cfg.buildSessionFactory();	//holds metadata of db,converts the xml to java objects
 
//SessionFactory factory = new Configuration().configure().buildSessionFactory();

	Session session= factory.openSession();			//Opens a new Session, which acts as a bridge between the Java application and the database,to interact with db.
	Transaction tx =session.beginTransaction(); 		//Hibernate starts a database transaction.
									//Hibernate internally sends an SQL command: BEGIN TRANSACTION;
**//for insert operation**
        Student st = new Student();
	st.setId(101);        
	st.setName("Alice");   
	st.setCity("New York"); 
	session.save(st);				//the data is not immediately written to the database. It is first stored in Hibernate's cache (temporary memory).
 
**//for retrive operation**
 	 Student s1=(Student)session.get(Student.class, id);//select query
	 System.out.println(s1);
	 System.out.println("Fetching object using load:");
	 Student s2=(Student)session.load(Student.class,101); //select query
	 System.out.println(s2);

**//for update operation**
	Student s1=(Student)session.get(Student.class, 102); 
        s1.setName("Bijoy");
        session.save(s1); 

 **//for delete operation**
 	Student s=session.get(Student.class, id); //get id
        session.delete(s); 
        tx.commit(); 
        				
        }

        tx.commit();					//Executes the actual database operation statement and saves the data permanently.
	session.close();				//Closes the session (disconnects from the database).
        factory.close();				//Closes the SessionFactory (shuts down Hibernate).
}
 

