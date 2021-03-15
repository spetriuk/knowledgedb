# @Before vs @BeforeClass vs @BeforeEach vs @BeforeAll | Baeldung

> Learn about the difference between JUnit annotations that can be used to run logic before tests.

**1\. Introduction**[](#introduction)
-------------------------------------

In this short tutorial, we're going to explain the differences between the _@Before_, _@BeforeClass_, _@BeforeEach_ and _@BeforeAll_ annotations in JUnit 4 and 5 – with practical examples of how to use them.

We'll also cover briefly their _@After_ complementary annotations.

Let's start with JUnit 4.

**2\. _@Before_**[](#before)
----------------------------

**Methods annotated with the _@Before_ annotation are executed before each test.** This is useful when we want to execute some common code before running a test.

Let's see an example where we initialize a list and add some values:

    @RunWith(JUnit4.class)
    public class BeforeAndAfterAnnotationsUnitTest {
    
        
    
        private List<String> list;
    
        @Before
        public void init() {
            LOG.info("startup");
            list = new ArrayList<>(Arrays.asList("test1", "test2"));
        }
    
        @After
        public void teardown() {
            LOG.info("teardown");
            list.clear();
        }
    }

**Notice that we also added another method annotated with _@After_ in order to clear the list after the execution of each test.**

After that, let's add some tests to check the size of our list:

    @Test
    public void whenCheckingListSize_thenSizeEqualsToInit() {
        LOG.info("executing test");
        assertEquals(2, list.size());
    
        list.add("another test");
    }
    
    @Test
    public void whenCheckingListSizeAgain_thenSizeEqualsToInit() {
        LOG.info("executing another test");
        assertEquals(2, list.size());
    
        list.add("yet another test");
    }

In this case, **it's crucial to make sure that test environment is properly set up before running each test** since the list is modified during every test execution.

If we take a look at the log output we can check that the _init_ and _teardown_ methods were executed once per test:

    ... startup
    ... executing another test
    ... teardown
    ... startup
    ... executing test
    ... teardown

**3. _@BeforeClass_**[](#beforeclass)
-------------------------------------

When we want to execute an expensive common operation before each test, **it's preferable to execute it only once before running all tests using _@BeforeClass_**. Some examples of common expensive operations are the creation of a database connection or the startup of a server.

Let's create a simple test class that simulates the creation of a database connection:

    @RunWith(JUnit4.class)
    public class BeforeClassAndAfterClassAnnotationsUnitTest {
    
        
        
        @BeforeClass
        public static void setup() {
            LOG.info("startup - creating DB connection");
        }
    
        @AfterClass
        public static void tearDown() {
            LOG.info("closing DB connection");
        }
    }

Notice that **these methods have to be static**, so they'll be executed before running the tests of the class.

As we did before, let's also add some simple tests:

    @Test
    public void simpleTest() {
        LOG.info("simple test");
    }
    
    @Test
    public void anotherSimpleTest() {
        LOG.info("another simple test");
    }

This time, if we take a look at the log output we can check that the _setup_ and _tearDown_ methods were executed only once:

    ... startup - creating DB connection
    ... simple test
    ... another simple test
    ... closing DB connection

**4. _@BeforeEach_ and _@BeforeAll_**[](#beforeeach-and-beforeall)
------------------------------------------------------------------

**_@BeforeEac_h and _@BeforeAll_ are the JUnit 5 equivalents of _@Before_ and _@BeforeClass_**. These annotations were renamed with clearer names to avoid confusion.

Let's duplicate our previous classes using these new annotations, starting with the _@BeforeEach_ and _@AfterEach_ annotations:

    @RunWith(JUnitPlatform.class)
    class BeforeEachAndAfterEachAnnotationsUnitTest {
    
        
        
        private List<String> list;
        
        @BeforeEach 
        void init() {
            LOG.info("startup");
            list = new ArrayList<>(Arrays.asList("test1", "test2"));
        }
    
        @AfterEach
        void teardown() {
            LOG.info("teardown");
            list.clear();
        }
    
        
    }

If we check logs, we can confirm that it works in the same way as with the _@Before_ and _@After_ annotations:

    ... startup
    ... executing another test
    ... teardown
    ... startup
    ... executing test
    ... teardown

Finally, let's do the same with the other test class to see the _@BeforeAll_ and _@AfterAll_ annotations in action:

    @RunWith(JUnitPlatform.class)
    public class BeforeAllAndAfterAllAnnotationsUnitTest {
    
        
        
        @BeforeAll
        public static void setup() {
            LOG.info("startup - creating DB connection");
        }
    
        @AfterAll
        public static void tearDown() {
            LOG.info("closing DB connection");
        }
    
        
    }

And the output is the same as with the old annotation:

    ... startup - creating DB connection
    ... simple test
    ... another simple test
    ... closing DB connection

**5\. Conclusion**[](#conclusion)
---------------------------------

In this article, we showed the differences between the _@Before_, _@BeforeClass_, _@BeforeEach_ and _@BeforeAll_ annotations in JUnit and when each of them should be used.

As always, the full source code of the examples is available [over on GitHub](https://github.com/eugenp/tutorials/tree/master/testing-modules/junit-5-basics).


[Source](https://www.baeldung.com/junit-before-beforeclass-beforeeach-beforeall)