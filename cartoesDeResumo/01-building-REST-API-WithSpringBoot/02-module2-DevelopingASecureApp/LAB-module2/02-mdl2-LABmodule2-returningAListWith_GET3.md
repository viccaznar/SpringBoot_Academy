Traduza integralmente o seguinte texto:

3: Test for an Additional GET Endpoint
Write a failing test for a new GET endpoint.

Let's add a new test method which expects a GET endpoint which returns multiple CashCard objects.

In CashCardApplicationTests.java, add a new test:

 @Test
 void shouldReturnAllCashCardsWhenListIsRequested() {
     ResponseEntity<String> response = restTemplate.getForEntity("/cashcards", String.class);
     assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
 }
Here we're making a request to the /cashcards endpoint. Since we're getting the entire list of cards, we don't need to specify any additional information in the request.

Run the tests and observe the failure.

The test should fail because we haven't implemented a Controller endpoint to handle this GET request.

How do you think it will fail? Perhaps a 404 NOT FOUND?

In a previous lesson we wrote a test that failed because no endpoint yet existed to match the route being requested. The result was a 404 NOT FOUND error. We might expect the same thing to happen when we run the new test, since we haven't added any code to the Controller.

Let's see what happens. Run the test and search for the following failure:

expected: 200 OK
 but was: 405 METHOD_NOT_ALLOWED
The error messages don't make it clear why we're receiving a 405 METHOD_NOT_ALLOWED error. The reason is a bit hard to discover, so we'll quickly summarize it: We've already implemented a /cashcards endpoint, but not for a GET verb.

This is Spring's process:

Spring receives a request to the /cashcards endpoint.
There's no mapping for the HTTP GET verb at that endpoint.
There is, however, a mapping to that endpoint for the HTTP POST verb. It's the endpoint for the Create operation that we implemented in a previous lesson!
Therefore, Spring reports a 405 METHOD_NOT_ALLOWED error instead of 404 NOT FOUND -- the route was indeed found, but it doesn't support the GET verb.
Implement the GET endpoint in the Controller.

To get past the 405 error, we need to implement the /cashcards endpoint in the Controller using a @GetMapping annotation:

@GetMapping()
private ResponseEntity<Iterable<CashCard>> findAll() {
   return ResponseEntity.ok(cashCardRepository.findAll());
}
Understand the handler method.

Once again we're using one of Spring Data's built-in implementations: CrudRepository.findAll(). Our implementing Repository, CashCardRepository, will automatically return all CashCard records from the database when findAll() is invoked.

Rerun the tests.

When we run the tests again we see they all pass, including the test for the GET endpoint for a CashCard list.

[~/exercises] $ ./gradlew test
...
BUILD SUCCESSFUL in 7s