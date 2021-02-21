# Presentation

Trantor is a basic test reporting tool which reports are intended for a person outside the development team. The generated reports are
more meaningful than the reports produced by a tool such as Surefire, without require too much time for a developer. For
each test case, we have its description, the expected result, and whether the test is successful or not.

Trantor aims to be as easy as possible for developers to use, and does not require the use of an external tool such as a
Maven or Gradle plugin. Just add in each class of test a test recorder, then for each method, describe the case of test.
Generated at the same time as the execution of the unit tests, the reports produced are in the form of HTML files, which
can be deployed where anyone can access them.

__Example for Junit4__

First, create a simple static class to instantiate the test reporter:

```java
public class ReportConfiguration {

    private static final String ROOT = "target";
    private static final String APPLICATION_NAME = "Trantor demo application";

    public static Junit4TestsReporter junit4HtmlReporter(String title, String... descriptions) {
        return Junit4TestsReporter.htmlReporter(ROOT, APPLICATION_NAME, title, descriptions);
    }

}
```

Next, in each test class, we add a `ClassRule` and a `TestWatcher` :

```java
public class UserTest {

    @ClassRule
    public static Junit4TestsReporter reporter = ReportConfigurationJava.junit4HtmlReporter(
            "BookServiceTest for JUnit4 java code",
            "Just a little CRUD test service"
    );

    @Rule
    public TestWatcher watcher = new Junit4TestResultRecorder(reporter);

    // ...
}
```

Finally, in test method, we describe the test case :

```java
public class UserTest {

    // ...
    @Test
    public void createBookTest() {

        reporter.describeNewTest(
                "Book creation test", // test name 
                0, // order of test in report
                "We create just one Book"); // description of test

        reporter.nominalCase(
                "Book creation", // case description 
                "Book is saved in database"); // expected result

        // exécution du test
    }
}
```

When all the tests are completed, the test report can be found in the
`target / trantor` folder. It is composed of an `index.html` file with a list links to a file per test class.

Trantor is written in Kotlin, and there are examples for both Kotlin and Java.

