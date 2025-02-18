**Spring program**

**Application.java**
package com.example.Spring;
import org.springframework.context.ApplicationContext;
public class Application
{
	public static void main(String[] args)
	{
	ApplicationContext context=new ClassPathXmlApplicationContext("constructorconfig.xml");//ClassPathXmlApplicationContext loads Spring configuration from an XML file in the classpath.
	BeanFactory factory =new ClassPathXmlApplicationContext("constructorconfig.xml") ;//Beanfactory used for small applications 

 	Employee emp=(Employee)context.getBean("employee"); 	//Spring retrieves the Employee bean from the container, casts it to an Employee object, and assigns it to the emp variable.
	emp.show();
	}
}


**Employee.java**-pojo class

package com.example.Spring;
public  class Employee{
	public  Employee(){}

	public Employee(int id,String,Address a)
	{
	this.id=id;
	this.name=name;
	this.ad=a;
	}
 
	void show()
	{
	System.out.println(id+" "+name);
	System.out.println(ad.tostring());
	}

	public int getId()
	{return id;}

	public void setId(int id)
	{this.id=id;}

	public String getName()
	{return name;}

	public void setName(String name)
	{this.name=name;}

	public void displayInfo()
	{System.out.println("Employee id:"+id+",name"+name);}
}


**Address.java**

package com.example.Spring;
public class Address
{
	private String city;
	private String country;

	public Address(String city,String country)
	{
	this.city=city;
	this.country=country;
	}

 	@override
	public String toString()
	{		
	return city+" "+country;
	}
}

**constructorconfig.xml**

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <!-- Define Address bean first -->
    <bean id="address" class="com.example.Spring.Address">
        <constructor-arg value="ghaziabad"/>
        <constructor-arg value="India"/>
    </bean>

    <!-- Define Employee bean and inject Address -->
    <bean id="employee" class="com.example.Spring.Employee">
        <constructor-arg value="101"/>
        <constructor-arg value="John Doe"/>
	
 	<constructor-arg ref="address"/>  <!-- Injecting the Address bean -->

 	<property name="name" value="John Doe"/> //property based injection
    </bean>

</beans>

