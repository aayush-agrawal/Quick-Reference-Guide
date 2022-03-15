# Mockito Walk Through
---

#### Setting Up Mockito
---

**Using JUnit <5 Annotations**
```
import org.mockito.junit.MockitoJUnitRunner;
import org.junit.runner.RunWith;

@RunWith(MockJUnitRunner.class)
public class MyClassTest {
    ...
}
```

**Using JUnit 5 Annotations**
```
import org.mockito.junit.jupiter.MockitoExtension;
import org.junit.jupiter.api.extension.ExtendWith;

@ExtendWith(MockitoExtension.class)
public class MyClassTest {
    ...
}
```

**Using initMocks**
Useful if you need more than one RunWith/Extend (e.g. SpringJunit4ClassRunner)
```
import org.mockito.MockitoAnnotations;
import org.junit.Before;

public class MyClassTest {
    @Before
    public void before() throw Exception {
        Mockito.initMocks(this);
    }
    ...
}
```

#### Creating Mocks and Spies
---
***Mock***: By default, all methods are stubbed unless you say otherwise.
***Spy***: By default, all methods use real implementation unless you say otherwise.

##### Creating a Mock
 **Use Annotations on Fields**
```
import org.mockito.MockitoAnnotations;
import org.junit.Before;
import org.mockito.Mock;

public class MyClassTest {

    @Mock
    private MyClass myObject;

    @Before
    public void before() throw Exception {
        Mockito.initMocks(this);
        // myObject is now a mock
    }
    ...
}
```

**Create from Method**
```
import org.mockito.MockitoAnnotations;
import org.junit.Before;
import org.mockito.Mock;

public class MyClassTest {

    private MyClass myObject;

    @Before
    public void before() throw Exception {
        Mockito.initMocks(this);
        myObject = Mockito.mock(MyClass.class);
    }
    ...
}
```

**Create from Method using Initialised Object**
```
import org.mockito.MockitoAnnotations;
import org.junit.Before;
import org.mockito.Mock;

public class MyClassTest {
    
    private Person person = Person("name", 18);

    @Before
    public void before() throw Exception {
        Mockito.initMocks(this);
        person = Mockito.mock(person);
    }
    ...
}
```

##### Creating a Spy
**Use Annotations on Fields**
```
import org.mockito.MockitoAnnotations;
import org.junit.Before;
import org.mockito.Spy;

public class MyClassTest {

    @Spy
    private MyClass myObject;

    @Before
    public void before() throw Exception {
        Mockito.initMocks(this);
        // myObject is now a spy
    }
    ...
}
```

**Create from Method**
```
import org.mockito.MockitoAnnotations;
import org.junit.Before;
import org.mockito.Spy;

public class MyClassTest {

    private MyClass myObject;

    @Before
    public void before() throw Exception {
        Mockito.initMocks(this);
        myObject = Mockito.spy(MyClass.class);
    }
    ...
}
```

**Create from method using Initialised Object**
```
import org.mockito.MockitoAnnotations;
import org.junit.Before;
import org.mockito.Spy;

public class MyClassTest {
    
    private Person person = Person("name", 18);

    @Before
    public void before() throw Exception {
        Mockito.initMocks(this);
        person = Mockito.spy(person);
    }
    ...
}
```

#### Injecting Mocks
---
**Using @InjectMocks Annotation**
```
public class InjectTest {

    @Mock
    private BlogRepository blogRepository;
    @InjectMocks
    private BlogPostService blogPostService;

    @Before
    public void setup() {
        MockitoAnnotations.initMocks(this);
        System.out.println(blogPostService);
    }
}
```

**Using Constructor**
```
public class InjectTest {

    @Mock
    private BlogRepository blogRepository;
    @InjectMocks
    private BlogPostService blogPostService;

    @Before
    public void setup() {
        MockitoAnnotations.initMocks(this);
        blogPostService = new BlogPostService(blogRepository);
    }
}
```

#### Stubbing a method
---
**Returning from a stubbed Method**
```
@Test
public void getAllBlogPosts_withBlogPostsInDb_returnsBlogPosts() {
    // arrange
    List<BlogPost> expected = Arrays.asList(new BlogPost("Spring for humans"), 
            new BlogPost("Mockito Cheatsheet"));
    BlogRepository blogRepository = Mockito.mock(BlogRepository.class);
    Mockito.when(blogRepository.getAllBlogPosts()).thenReturn(expected);
    BlogPostService service = new BlogPostService(blogRepository);
    
    // act
    List<BlogPost> actual = service.getAllBlogPosts();
    
    // assert
    assertEquals(expected, actual);
}
```

**Providing an Alternative Implementation for a Method**
```
@Test
public void getBlogPostById_withExistingBlogPostInDb_returnsBlogPost() {
    // arrange
    BlogPost expected = new BlogPost("Spring for Humans");
    BlogRepository blogRepository = Mockito.mock(BlogRepository.class);
    Mockito.when(blogRepository.getBlogPostById(Mockito.anyInt())).thenAnswer(invocationOnMock -> {
        int id = invocationOnMock.getArgument(0);
        if(id == 1) return Optional.of(expected);
        else return Optional.empty();
    });
    BlogPostService service = new BlogPostService(blogRepository);

    // act
    BlogPost actual = service.getBlogPostById(1).get();

    // assert
    assertEquals(expected, actual);
}
```

**Throwing an Exception from a Method**
```
@Test(expected = IllegalArgumentException.class)
public void getBlogPostById_withMissingBlogPost_throwsException() {
    // arrange
    BlogPost expected = new BlogPost("Spring for Humans");
    BlogRepository blogRepository = Mockito.mock(BlogRepository.class);
    Mockito.when(blogRepository.getBlogPostById(1)).thenThrow(new IllegalArgumentException());
    BlogPostService service = new BlogPostService(blogRepository);

    // act
    BlogPost actual = service.getBlogPostById(1).get();

    // assert
    // Test will pass if exception of type IllegalArgumentException is thrown
}
```

**Calling the Real Implementation**
```
@Test
public void getBlogPostById_withMissingBlogPost_returnsEmpty() {
    // arrange
    BlogRepository blogRepository = Mockito.mock(BlogRepository.class);
    Mockito.when(blogRepository.getBlogPostById(1)).thenCallRealMethod();
    BlogPostService service = new BlogPostService(blogRepository);

    // act
    Optional<BlogPost> actual = service.getBlogPostById(1);

    // assert
    assertFalse(actual.isPresent());
}
```

**Using Deep Stubs**
Deep stubs are used for methods which are chained, but you don't care about the intermediate values. These should be used sparingly, as discussed in the Mockito documentation.
```
// Without deep stubs
@Test
public void checkIfDriverIsPostgres_withPostgresDriver_returnsTrue() {
    // arrange
    Configuration configMock = Mockito.mock(Configuration.class);
    RepositoryConfiguration repositoryConfigMock = Mockito.mock(RepositoryConfiguration.class);
    Mockito.when(configMock.getRepositoryConfiguration()).thenReturn(repositoryConfigMock);
    DatabaseConfiguration databaseConfigMock = Mockito.mock(DatabaseConfiguration.class);
    Mockito.when(repositoryConfigMock.getDatabaseConfiguration()).thenReturn(databaseConfigMock);
    DatabaseConfiguration.DriverConfiguration driverConfigMock = Mockito.mock(DatabaseConfiguration.DriverConfiguration.class);
    Mockito.when(databaseConfigMock.getDriverConfiguration()).thenReturn(driverConfigMock);
    Mockito.when(driverConfigMock.getDriverType()).thenReturn("postgres");

    BlogRepository blogRepository = new BlogRepository(configMock);

    // act
    boolean actual = blogRepository.checkIfDriverIsPostgres();

    // assert
    Assert.assertTrue(actual);
}

// With deep stubs
@Test
public void usingDeepStubs_checkIfDriverIsPostgres_withPostgresDriver_returnsTrue() {
    // arrange
    Configuration configMock = Mockito.mock(Configuration.class, Mockito.RETURNS_DEEP_STUBS);
    Mockito.when(configMock.getRepositoryConfiguration()
            .getDatabaseConfiguration().getDriverConfiguration().getDriverType()).thenReturn("postgres");

    BlogRepository blogRepository = new BlogRepository(configMock);

    // act
    boolean actual = blogRepository.checkIfDriverIsPostgres();

    // assert
    Assert.assertTrue(actual);
}
```

#### Stubbing a Void Method
**Providing an Alternative Implementation for a Method**
```
@Test
public void getAllBlogPosts_withBlogPostsInDb_callsVerifyConnectionToDatabaseIsAlive() {
    // arrange
    Configuration configMock = Mockito.mock(Configuration.class);
    BlogRepository blogRepository = new BlogRepository(configMock);

    AtomicBoolean verifyMethodCalled = new AtomicBoolean(false);
    Mockito.doAnswer(invocationOnMock -> {
        // We can do whatever we want here, and it will be executed when
        // verifyConnectionToDatabaseIsAlive
        // If the method had any arguments, we can capture them here
        verifyMethodCalled.set(true);
        return null;
    }).when(configMock).verifyConnectionToDatabaseIsAlive();

    // act
    blogRepository.getAllBlogPosts();

    // assert
    assertTrue(verifyMethodCalled.get());
}
```

**Throwing an Exception from a Method**
```
@Test(expected = DatabaseDownException.class)
public void getAllBlogPosts_withConnectionToDbDown_throwsException() {
    // arrange
    Configuration configMock = Mockito.mock(Configuration.class);
    BlogRepository blogRepository = new BlogRepository(configMock);

    AtomicBoolean verifyMethodCalled = new AtomicBoolean(false);
    Mockito.doThrow(new DatabaseDownException()).when(configMock).verifyConnectionToDatabaseIsAlive();

    // act
    blogRepository.getAllBlogPosts();

    // assert
    // Test will pass if exception of type DatabaseDownException is thrown
}
```

**Calling the Real Implementation**
```
@Test
public void getAllBlogPosts_withBlogPostsInDb_returnsBlogPosts() {
    // arrange
    Configuration configMock = Mockito.mock(Configuration.class);
    BlogRepository blogRepository = new BlogRepository(configMock);

    AtomicBoolean verifyMethodCalled = new AtomicBoolean(false);
    Mockito.doCallRealMethod().when(configMock).verifyConnectionToDatabaseIsAlive();

    // act
    blogRepository.getAllBlogPosts();

    // assert
    // Test will pass if exception of type DatabaseDownException is thrown
}
```

#### Matchers
---
**Using a Real Parameter**
```
@Test
public void getBlogPostByAuthorAndAfterDate_withExistingBlogPostsInDb_returnsBlogPosts() {
    // arrange
    List<BlogPost> expected = Collections.singletonList(new BlogPost("Spring for Humans"));
    BlogRepository blogRepository = Mockito.mock(BlogRepository.class);
    Date date = new Date();
    String author = "Shane";
    Mockito.when(blogRepository.getBlogPostsByAuthorAndAfterDate(author, date)).thenReturn(expected);
    BlogPostService service = new BlogPostService(blogRepository);

    // act
    List<BlogPost> actual = service.getBlogPostsByAuthorAndAfterDate(author, date);

    // assert
    assertEquals(expected, actual);
}
```

**Using any()**
```
@Test
public void getBlogPostByAuthorAndAfterDate_withoutMatchingBlogPostsInDb_returnsEmptyList() {
    // arrange
    BlogRepository blogRepository = Mockito.mock(BlogRepository.class);
    Mockito.when(blogRepository.getBlogPostsByAuthorAndAfterDate(Mockito.any(), Mockito.any())).thenReturn(Collections.emptyList());
    BlogPostService service = new BlogPostService(blogRepository);

    // act
    List<BlogPost> actual = service.getBlogPostsByAuthorAndAfterDate("any author", new Date());

    // assert
    assertTrue(actual.isEmpty());
}
```

**Using anyClass()**
```
@Test
public void getBlogPostByAuthorAndAfterDate_withMatchingAuthorAndDate_returnsPostsSortedByDate() {
    // arrange
    BlogPost first = new BlogPost("Spring for Humans", "Shane", new Date(5));
    BlogPost second = new BlogPost("Mockito Cheatsheet", "Shane", new Date(6));
    BlogPost third = new BlogPost("?", "Shane", new Date(7));
    BlogRepository blogRepository = Mockito.mock(BlogRepository.class);
    // Good for overloaded methods, you can specify type of params so the call isn't ambiguous.
    Mockito.when(blogRepository.getBlogPostsByAuthorAndAfterDate(Mockito.any(String.class), Mockito.any(Date.class)))
            .thenReturn(Arrays.asList(second, third, first));
    BlogPostService service = new BlogPostService(blogRepository);

    List<BlogPost> expected = Arrays.asList(first, second, third);

    // act
    List<BlogPost> actual = service.getBlogPostsByAuthorAndAfterDate("any author", new Date());

    // assert
    assertEquals(expected.get(0), actual.get(0));
    assertEquals(expected.get(1), actual.get(1));
    assertEquals(expected.get(2), actual.get(2));
}
```

**Using eq()**
```
@Test
public void getBlogPostByAuthorAndAfterDate_withMatchingAuthorButFutureDate_returnsEmptyList() {
    // arrange
    List<BlogPost> expected = Collections.singletonList(new BlogPost("Spring for Humans"));
    BlogRepository blogRepository = Mockito.mock(BlogRepository.class);
    Date date = new Date();
    Mockito.when(blogRepository.getBlogPostsByAuthorAndAfterDate(Mockito.any(), Mockito.eq(date))).thenReturn(expected);
    BlogPostService service = new BlogPostService(blogRepository);

    // act
    List<BlogPost> actual = service.getBlogPostsByAuthorAndAfterDate("any author", date);

    // assert
    assertTrue(actual.isEmpty());
}
```

**Using Custom Matcher**
```
@Test
public void checkIfBlogPostHasBeenSaved_withBlogPost_returnsTrue() {
    // arrange
    BlogPost post = new BlogPost("Spring for Humans", "Shane", new Date(5));
    BlogRepository blogRepository = Mockito.mock(BlogRepository.class);

    ArgumentMatcher<BlogPost> blogPostMatcher = passedBlogPost ->
            "Shane".equals(passedBlogPost.getAuthor());

    Mockito.when(blogRepository.checkIfBlogPostHasBeenSaved(Mockito.argThat(blogPostMatcher)))
            .thenReturn(true);
    BlogPostService service = new BlogPostService(blogRepository);

    // act
    boolean actual = service.checkIfBlogPostHasBeenSaved(post);

    // assert
    assertTrue(actual);
}
```

**Verify a Stubbed Method Has Been Called**
```
@Test
public void getAllBlogPosts_withBlogPost_verifiesThatRepositoryWasCalled() {
    // arrange
    List<BlogPost> expected = Arrays.asList(new BlogPost("Spring for humans"),
            new BlogPost("Mockito Cheatsheet"));
    BlogRepository blogRepository = Mockito.mock(BlogRepository.class);
    Mockito.when(blogRepository.getAllBlogPosts()).thenReturn(expected);
    BlogPostService service = new BlogPostService(blogRepository);

    // act
    service.getAllBlogPosts();

    // assert
    Mockito.verify(blogRepository).getAllBlogPosts();
}
```

**Verify a Stubbed Method Hasn't Been Called**
```
@Test
public void getBlogPostById_withBlogPost_verifiesThatCorrectMethodWasCalled() {
    // arrange
    BlogRepository blogRepository = Mockito.mock(BlogRepository.class);
    BlogPost expoected = new BlogPost("Spring for humans");
    Mockito.when(blogRepository.getBlogPostById(1)).thenReturn(Optional.of(expoected));
    BlogPostService service = new BlogPostService(blogRepository);

    // act
    service.getBlogPostById(1);

    // assert
    Mockito.verify(blogRepository, Mockito.never()).getBlogPostById(100);
    Mockito.verify(blogRepository).getBlogPostById(1);
}
```

#### Capturing Arguments
---
```
@Test
public void getBlogPostById_withBlogPost_verifiesCorrectArgumentPassed() {
    // arrange
    BlogRepository blogRepository = Mockito.mock(BlogRepository.class);
    int expected = 1;
    Mockito.when(blogRepository.getBlogPostById(expected))
            .thenReturn(Optional.of(new BlogPost("Spring for humans")));
    BlogPostService service = new BlogPostService(blogRepository);
    ArgumentCaptor<Integer> captor = ArgumentCaptor.forClass(Integer.class);

    // act
    service.getBlogPostById(expected);
    Mockito.verify(blogRepository).getBlogPostById(captor.capture());
    int actual = captor.getValue();

    // assert
    assertEquals(expected, actual);
}
```