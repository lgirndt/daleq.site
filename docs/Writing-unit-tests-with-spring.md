---
layout: doc
title: Writing Unit Tests with Spring
---

Daleq is a framework to describe test data in a relational database. We've written Daleq to be used in Unit Tests. How you write your Unit Tests is up to you, even how you access your database in your tests. All Daleq needs is a ```javax.sql.DataSource``` to talk to.

Nonetheless, we offer a best practice for Unit Testing by using Spring.

The following paragraphs will sketch this approach.

## Choosing your Database

There is one decision you have to face: Do you write your tests against the same database type you use in production or do you use an embedded database. Both approaches have pros and cons.

Choosing the same database type will give you strong guarantees that your queries, if tested thoroughly, are correct. But usually these databases are applications like Mysql or Oracle, which are not so easily integrated into build environment and therefore put some burden on the build process. 

On the other hand, an embedded database like HSQLDB, H2 or SQLite differ slightly to your production system and eventually you will be faced with this difference. Nonetheless, they are integrated easily by just adding another dependency to project. And since they run in memory, they are usally blazingly fast. 

At the end of the day, it is up to you, how you specify your DataSource Bean.

We've chosen to use HSQLDB. 

## Why Spring?

Why do we suggest to use Spring for unit testing? The answer is simple: The SpringSource guys already did an awesome job to provide an environment for database unit tests. The [[AbstractTransactionalJUnit4SpringContextTests|http://static.springsource.org/spring/docs/current/javadoc-api/org/springframework/test/context/junit4/AbstractTransactionalJUnit4SpringContextTests.html]] is the only tool we need: Each test method is wrapped by a transaction and it is rolled back after each test is run.

## Setting up the Application Context

It's the age of Spring 3.x, hence we use Spring's Java Config. But nonetheless, the same approach would also work with the classic XML configuration.

{% highlight java %}
@Configuration
public class TestConfig {

    @Bean
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder().addScript("schema.sql").build();
    }

    @Bean
    public PlatformTransactionManager transactionManager(final DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }

    @Bean
    public DaleqSupport daleqSupport(final ConnectionFactory connectionFactory) {
        return DbUnitDaleqSupport.createInstance(connectionFactory);
    }

    @Bean
    public ConnectionFactory connectionFactory(final DataSource dataSource) {
        final SpringConnectionFactory connectionFactory = new SpringConnectionFactory();
        connectionFactory.setDataSource(dataSource);
        return connectionFactory;
    }
}
{% endhighlight %}

The [[EmbeddedDatabaseBuilder|http://static.springsource.org/spring/docs/current/javadoc-api/org/springframework/jdbc/datasource/embedded/EmbeddedDatabaseBuilder.html]] is awesome. It sets up the whole embedded datbase when the application context is built and runs a script on it to set up the schema. 

If you've read [[Understanding the DSL]], you are already familiar with ```DaleqSupport```. This is were it gets instantiated. Furthermore we need a ```ConnectionFactory``` that knows about Spring. Since each JDBC connection is executed in a transaction managed by the PlatformTransactionManager, Daleq needs a Connection in the same Transaction as well. That's the job of ```SpringConnectionFactory```.


## Writing the Test

Everything is set up, we are about to write our unit tests:

{% highlight java%}
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = TestConfig.class)
public class JdbcProductDaoTest extends AbstractTransactionalJUnit4SpringContextTests {

    @Autowired
    private DaleqSupport daleq;

    private JdbcProductDao productDao;

    @Override
    @Autowired
    public void setDataSource(final DataSource dataSource) {
        super.setDataSource(dataSource);
        this.productDao = new JdbcProductDao(dataSource);
    }
}
{% endhighlight %}

There is no magic here. We extend ```AbstractTransactionalJUnit4SpringContextTests```, hence we need ```@RunWith(SpringJUnit4ClassRunner.class)``` as well. ```JdbcProductDao``` is our class under test. 

The Test Fixture is ready, we can write our tests as we've learned it in [[Understanding the DSL]].