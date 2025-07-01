Traduza integralmente o seguinte texto:

11: About CSRF
As we learned in the accompanying lesson, protection against Cross-Site Request Forgery (CSRF, or "sea-surf") is an important aspect of HTTP-based APIs used by web-based applications.

Yet, we've disabled CSRF via the csrf.disable() code in SecurityConfig.filterChain.

Why have we disabled CSRF?

For the purposes of our Family Cash Card API, we're going to follow the guidance from the Spring Security team regarding non-browser clients:

When should you use CSRF protection? Our recommendation is to use CSRF protection for any request that could be processed by a browser by normal users. If you are only creating a service that is used by non-browser clients, you will likely want to disable CSRF protection.

If you'd like to add CSRF security to our application, please review the testing support options below.

MockMVC CSRF testing examples
WebTestClient CSRF testing examples.
A description of the Double-Submit Cookie Pattern.