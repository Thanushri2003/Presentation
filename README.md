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


hibernates executed query looks like:
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
 Insert


public class Insert {
	public static void main(String[] args){
		System.out.println("Project started..");

		Configuration cfg = new Configuration();  //Creates a Configuration object, which is the starting point of Hibernate.
		cfg.configure();		//Reads and loads the hibernate.cfg.xml file.
		SessionFactory factory = cfg.buildSessionFactory();//holds metadata of db,converts the xml to java objects
		Session session= factory.openSession();
		Transaction tx =session.beginTransaction(); 

  //user input
	Scanner scanner = new Scanner(System.in);
	System.out.print("Enter the number of students: ");
        int numStudents = scanner.nextInt();
        scanner.nextLine(); // Consume newline

        for (int i = 1; i <= numStudents; i++) {
            System.out.println("Enter details for student " + i + ":");

            System.out.print("Enter ID: ");
            int id = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            System.out.print("Enter Name: ");
            String name = scanner.nextLine();

            System.out.print("Enter City: ");
            String city = scanner.nextLine();

            // Creating Student object
            Student st = new Student();
            st.setId(id);  // Manually setting ID
            st.setName(name);
            st.setCity(city);

            // Save student to database
            session.save(st);
        }

        // Commit transaction
        tx.commit();
        factory.close();
        scanner.close();
}
 

