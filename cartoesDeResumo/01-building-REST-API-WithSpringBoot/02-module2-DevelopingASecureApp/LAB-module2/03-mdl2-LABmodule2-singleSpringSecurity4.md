Traduza integralmente o seguinte texto:

4: Satisfy Spring Security's Dependencies
Next, we'll focus on getting our tests passing again by providing the minimum configuration needed by Spring Security.

We've provided another file on our behalf: example/cashcard/SecurityConfig.java. This will be the Java Bean where we'll configure Spring Security for our application.

Uncomment SecurityConfig.java and review.

Open SecurityConfig.

Notice that most of the file is commented.

Uncomment all commented lines within SecurityConfig, including all methods and import statements.

package example.cashcard;
...
class SecurityConfig {

    SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        return http.build();
    }

    @Bean
    PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
...
filterChain returns http.build(), which is the minimum needed for now.

Note: Please ignore the method passwordEncoder() for now.

Enable Spring Security.

At the moment SecurityConfig is just an un-referenced Java class as nothing is using it.

Let's turn SecurityConfig into our configuration Bean for Spring Security.

// Add this Annotation
@Configuration
class SecurityConfig {

    // Add this Annotation
    @Bean
    SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        return http.build();
    }
...
Understand the Annotations.

@Configuration
class SecurityConfig {...}
The @Configuration annotation tells Spring to use this class to configure Spring and Spring Boot itself. Any Beans specified in this class will now be available to Spring's Auto Configuration engine.

@Bean
SecurityFilterChain filterChain
Spring Security expects a Bean to configure its Filter Chain, which you learned about in the Simple Spring Security lesson. Annotating a method returning a SecurityFilterChain with the @Bean satisfies this expectation.

Run the tests.

When you run the tests you'll see that once again all tests pass – except for the test for creating a new CashCard via a POST.

[~/exercises] $ ./gradlew test
...
CashCardApplicationTests > shouldCreateANewCashCard() FAILED
    org.opentest4j.AssertionFailedError:
    expected: 201 CREATED
     but was: 403 FORBIDDEN
...
11 tests completed, 1 failed
This is expected. We'll cover this in depth a bit later on.