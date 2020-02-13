### 学习SpringData的第一天
## 1、SpringData简介
&nbsp;&nbsp;在我的理解看来，**SpringData的作用与MyBatis以及Hibernate的作用相仿**，都是对数据库进行操作。 <br />
  SpringData以及JPA规范为我们提供了Repository层的实现，我们可能只需要声明我们将要做的操作的java方法，以一定的规范声明方法的方法名，使springdata对方法自动识别，然后生成对应的sql语句并对数据库操作。而且springdata+jpa还提供注解表示实体类，并自动在数据库中创建表。  <br />
  可能我们**只需要按规范创建实体类，声明要对数据库进行操作的方法以及方法名，springdata底层就会帮我们写sql语句、创建数据表**。
  
## 2、SpringData小例子
### 2.1、添加依赖
  创建一个maven项目，并添加web、springdata+jpa、springtest、mysql依赖
  ```java
    <dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
    
    <!-- springData 的启动器 -->
    <!-- springBoot情况下 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
    <!-- 其他情况下 -->
   <!-- 
    <dependency>        
      <groupId>org.springframework.data</groupId>        
      <artifactId>spring-data-jpa</artifactId>    
    </dependency>
   --> 
   
		<!-- mysql 数据库驱动 -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>
  ```
### 2.2、创建实体类
  ```java
 import javax.persistence.*;//注解所在包

@Entity//标注为实体类
@Table(name = "t_Users")//标注为数据表，name指定数据表的表名
public class Users {
    @Id//指定为id
   @GeneratedValue(strategy = GenerationType.IDENTITY)
    /**
     * @GeneratedValue 标注为数据表唯一主键
     *  有两个属性 strategy和generator
     *  generator：属性值为字符串，默认为""，声明了主键生成器的名称
     *  strategy：提供四种值
     *      -AUTO   主键由程序控制, 默认选项
     *      -IDENTITY   主键由数据库生成, 采用数据库自增长, (Oracle不支持这种方式)
     *      -SEQUENCE   通过数据库的序列产生主键, (MYSQL不支持)
     *      -Table  提供特定的数据库产生主键, 该方式更有利于数据库的移植
     */
    @Column(name = "id")
    /**
     * @Column 标识实体类中的属性与数据表中字段的对应关系
     *      name属性标注在数据表中对应字段的名称
     */
    private Integer id;
    @Column(name = "name")
    private String name;
    @Column(name = "age")
    private Integer age;
    @Column(name = "address")
    private String address;

---getter setter tostring 方法---
  ```
### 2.3、创建Repository
 ```java
  public interface UsersRepository extends JpaRepository<Users,Integer> {

    //根据id获取用户信息
    //select * from t_Users where t_Users.id = {id}
    public Users getById(Integer id);
}
 ```
 
### 2.4、测试方法
```java
  @SpringBootTest
class SpringBootSpringdataTestApplicationTests {

	@Autowired
	private UsersRepository usersRepository;

	@Test
	void contextLoads() {
	}

	@Test
	public void testSave(){
		Users users = new Users();
		users.setAddress("北京市海淀");
		users.setAge(20);
		users.setName("张三");
		this.usersRepository.save(users);//save方法为Repository内部自带方法
	}

	@Test
	public void testGetById(){
		Users users = usersRepository.getById(1);
		System.out.println(users);
	}
}
```
**欢迎尝试**
