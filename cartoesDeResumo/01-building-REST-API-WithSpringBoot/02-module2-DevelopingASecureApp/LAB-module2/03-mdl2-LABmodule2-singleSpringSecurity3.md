Traduza integralmente o seguinte texto:

3: Add the Spring Security Dependency
We can add support for Spring Security by adding the appropriate dependency.

Add the dependency.

Add the following to the build.gradle file in the dependencies {} section:

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'

    // Add the following dependency
    implementation 'org.springframework.boot:spring-boot-starter-security'
    ...
Run the tests.

We've added Spring Security capabilities to our application, but changed no code.

So, what do we expect to happen when we run the tests?

Note that we'll always run ./gradlew test to run the tests.

[~/exercises] $ ./gradlew test
...
CashCardApplicationTests > shouldReturnASortedPageOfCashCards() FAILED
...
CashCardApplicationTests > shouldReturnACashCardWhenDataIsSaved() FAILED
...
CashCardApplicationTests > shouldCreateANewCashCard() FAILED
...
CashCardApplicationTests > shouldReturnAPageOfCashCards() FAILED
...
CashCardApplicationTests > shouldReturnAllCashCardsWhenListIsRequested() FAILED
...
CashCardApplicationTests > shouldReturnASortedPageOfCashCardsWithNoParametersAndUseDefaultValues() FAILED
...
CashCardApplicationTests > shouldNotReturnACashCardWithAnUnknownId() FAILED
11 tests completed, 7 failed
> Task :test FAILED
Things are really broken!

Every test method within CashCardApplicationTests failed.

Many failures are similar to the one below:

expected: <SOME NUMBER>
 but was: 0
In most cases, our tests expect CashCard data to be returned from our API, but nothing was returned.

Why do you think all tests of our Cash Card API are failing after adding the Spring Security dependency?

Understand why everything is broken.

What happened here?

When we added the Spring Security dependency to our application, security was enabled by default.

Since we haven't specified how authentication and authorization are performed within our Cash Card API, Spring Security has completely locked down our API.

Well, better safe than sorry, right?

Next, let's configure Spring Security for our application.