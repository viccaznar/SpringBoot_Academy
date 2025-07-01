Traduza integralmente o seguinte texto:

7: Support Authorization
As we learned in the accompanying lesson, Spring Security supports many forms of authorization.

Here we'll implement Role-Based Access Control (RBAC).

It's likely that a user service will provide access to many authenticated users, but only "card owners" should be allowed to access Family Cash Cards managed by our application. Let's make those updates now.

Add users and roles to the UserDetailsService Bean.

To test authorization, we need multiple test users with a variety of roles.

Update SecurityConfig.testOnlyUsers and add the CARD-OWNER role to sarah1.

Also, let's add a new user named "hank-owns-no-cards" with a role of NON-OWNER.

...
@Bean
UserDetailsService testOnlyUsers(PasswordEncoder passwordEncoder) {
  User.UserBuilder users = User.builder();
  UserDetails sarah = users
    .username("sarah1")
    .password(passwordEncoder.encode("abc123"))
    .roles("CARD-OWNER") // new role
    .build();
  UserDetails hankOwnsNoCards = users
    .username("hank-owns-no-cards")
    .password(passwordEncoder.encode("qrs456"))
    .roles("NON-OWNER") // new role
    .build();
  return new InMemoryUserDetailsManager(sarah, hankOwnsNoCards);
}
Test for Role verification.

Let's add a test that will fail at first, but will pass when we fully implement authorization.

Here, we'll assert that user "hank-owns-no-cards" shouldn't have access to a CashCard since that user is not a CARD-OWNER.

@Test
void shouldRejectUsersWhoAreNotCardOwners() {
    ResponseEntity<String> response = restTemplate
      .withBasicAuth("hank-owns-no-cards", "qrs456")
      .getForEntity("/cashcards/99", String.class);
    assertThat(response.getStatusCode()).isEqualTo(HttpStatus.FORBIDDEN);
}
But wait! CashCard with ID 99 belongs to sarah1, right? Shouldn't only sarah1 have access to that data regardless of role?

You're right! Keep that in mind for later in this lab.

Run the tests.

We see that our new test fails when we run it.

[~/exercises] $ ./gradlew test
...
CashCardApplicationTests > shouldRejectUsersWhoAreNotCardOwners() FAILED
 org.opentest4j.AssertionFailedError:
 expected: 403 FORBIDDEN
  but was: 200 OK
Why was hank-owns-no-cards able to access a CashCard as indicated by the 200 OK response?

Although we have given the test users roles, we are not enforcing role-based security.

Enable role-based security.

Edit SecurityConfig.filterChain to restrict access to only users with the CARD-OWNER role.

@Bean
SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
     http
             .authorizeHttpRequests(request -> request
                     .requestMatchers("/cashcards/**")
                     .hasRole("CARD-OWNER")) // enable RBAC: Replace the .authenticated() call with the hasRole(...) call.
             .httpBasic(Customizer.withDefaults())
             .csrf(csrf -> csrf.disable());
     return http.build();
}
Run the tests.

We see that our tests pass!

[~/exercises] $ ./gradlew test
...
CashCardApplicationTests > shouldRejectUsersWhoAreNotCardOwners() PASSED
We've now successfully enabled RBAC-based authorization!