Traduza integralmente o seguinte texto:

6: Testing Basic Authentication
As we learned in the accompanying lesson, there are many ways of providing user authentication and authorization information for a Spring Boot application using Spring Security.

For our tests, we'll configure a test-only service that Spring Security will use for this purpose: An InMemoryUserDetailsManager.

Similar to how we configured an in-memory database using H2 for testing Spring Data, we'll configure an in-memory service with test users to test Spring Security.

Configure a test-only UserDetailsService.

Which username and password should we submit in our test HTTP requests?

When you reviewed changes to src/test/resources/data.sql you should've seen that we set an OWNER value for each CashCard in the database to the username sarah1. For example:

INSERT INTO CASH_CARD(ID, AMOUNT, OWNER) VALUES (100, 1.00, 'sarah1');
Let's provide a test-only UserDetailsService with the user sarah1.

Add the following Bean to SecurityConfig.

  @Bean
  UserDetailsService testOnlyUsers(PasswordEncoder passwordEncoder) {
   User.UserBuilder users = User.builder();
   UserDetails sarah = users
     .username("sarah1")
     .password(passwordEncoder.encode("abc123"))
     .roles() // No roles for now
     .build();
   return new InMemoryUserDetailsManager(sarah);
  }
This UserDetailsService configuration should be understandable: configure a user named sarah1 with password abc123.

Spring's IoC container will find the UserDetailsService Bean and Spring Data will use it when needed.

Configure Basic Auth in HTTP tests.

Select one test method that uses restTemplate.getForEntity, and update it with basic authentication for sarah1.

void shouldReturnACashCardWhenDataIsSaved() {
    ResponseEntity<String> response = restTemplate
            .withBasicAuth("sarah1", "abc123") // Add this
            .getForEntity("/cashcards/99", String.class);
    assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
    ...
Run the tests.

The updated test that provides the basics should now pass!

[~/exercises] $ ./gradlew test
...
CashCardApplicationTests > shouldReturnACashCardWhenDataIsSaved() PASSED
...
Update all remaining CashCardApplicationTests tests and rerun tests.

Now for some tedium: Update all remaining restTemplate-based tests to supply .withBasicAuth("sarah1", "abc123") with every HTTP request.

When finished, rerun the test.

[~/exercises] $ ./gradlew test
...
BUILD SUCCESSFUL in 9s
Everything passes!

Congratulations, you've implemented and tested Basic Auth!

Verify Basic Auth with additional tests.

Now let's add tests that expect a 401 UNAUTHORIZED response when incorrect credentials are submitted using basic authentication.

@Test
void shouldNotReturnACashCardWhenUsingBadCredentials() {
    ResponseEntity<String> response = restTemplate
      .withBasicAuth("BAD-USER", "abc123")
      .getForEntity("/cashcards/99", String.class);
    assertThat(response.getStatusCode()).isEqualTo(HttpStatus.UNAUTHORIZED);

    response = restTemplate
      .withBasicAuth("sarah1", "BAD-PASSWORD")
      .getForEntity("/cashcards/99", String.class);
    assertThat(response.getStatusCode()).isEqualTo(HttpStatus.UNAUTHORIZED);
}
This test should pass...

[~/exercises] $ ./gradlew test
...
CashCardApplicationTests > shouldNotReturnACashCardWhenUsingBadCredentials() PASSED
Success! Now that we've implemented authentication, let's move on to implement authorization next.