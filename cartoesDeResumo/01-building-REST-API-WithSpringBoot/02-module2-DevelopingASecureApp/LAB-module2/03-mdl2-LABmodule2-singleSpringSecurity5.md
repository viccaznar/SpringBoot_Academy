Traduza integralmente o seguinte texto:

5: Configure Basic Authentication
Thus far we've bootstrapped Spring Security, but not actually secured our application.

Now let's secure our application by configuring basic authentication.

Configure basic authentication.

Update SecurityConfig.filterChain with the following to enable basic authentication:

@Bean
SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
     http
             .authorizeHttpRequests(request -> request
                     .requestMatchers("/cashcards/**")
                     .authenticated())
             .httpBasic(Customizer.withDefaults())
             .csrf(csrf -> csrf.disable());
     return http.build();
}
Understand the Spring Security configuration.

That's a lot of method calls!

Here, if we explain Spring Security's builder pattern in more understandable language, we see:

All HTTP requests to cashcards/ endpoints are required to be authenticated using HTTP Basic Authentication security (username and password).

Also, do not require CSRF security.

Note: We'll talk about CSRF security later in this lab.

Run the tests.

What will happen when we run our tests?

When you run the tests, you'll notice that most tests fail with a 401 UNAUTHORIZED HTTP status code, such as the following:

[~/exercises] $ ./gradlew test
...
expected: 200 OK
  but was: 401 UNAUTHORIZED
Though it might not look like it, this is progress!

We've enabled basic authentication, requiring that requests must supply a username and password.

Our tests don't provide a username and password with our HTTP requests. So let's do that next.