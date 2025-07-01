Traduza integralmente o seguinte texto:

8: Paging and Sorting defaults
We now have an endpoint which requires the client to send four pieces of information: The page index and size, the sort order, and direction. This is a lot to ask, so let's make it easier on them.

Write a new test which doesn't send any pagination or sorting parameters.

We'll add a new test that expects reasonable defaults for the parameters.

The defaults will be:

Sort by amount ascending.
A page size of something larger than 3, so that all of our fixtures will be returned.
@Test
void shouldReturnASortedPageOfCashCardsWithNoParametersAndUseDefaultValues() {
    ResponseEntity<String> response = restTemplate.getForEntity("/cashcards", String.class);
    assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);

    DocumentContext documentContext = JsonPath.parse(response.getBody());
    JSONArray page = documentContext.read("$[*]");
    assertThat(page.size()).isEqualTo(3);

    JSONArray amounts = documentContext.read("$..amount");
    assertThat(amounts).containsExactly(1.00, 123.45, 150.00);
}
Run the tests.

[~/exercises] $ ./gradlew test
...
Actual and expected have the same elements but not in the same order, at index 0 actual element was:
  123.45
whereas expected element was:
  1.0
The test failure shows:

All the Cash Cards are being returned, since the (page.size()).isEqualTo(3) assertion succeeded.
BUT: They are not sorted since the (amounts).containsExactly(1.00, 123.45, 150.00) assertion fails:
Make the test pass.

Change the implementation by adding a single line to the Controller method:

...
PageRequest.of(
        pageable.getPageNumber(),
        pageable.getPageSize(),
        pageable.getSortOr(Sort.by(Sort.Direction.ASC, "amount"))
));
...
Understand the implementation.

So, what just happened?

The answer is that the getSortOr() method provides default values for the page, size, and sort parameters. The default values come from two different sources:

Spring provides the default page and size values (they are 0 and 20, respectively). A default of 20 for page size explains why all three of our Cash Cards were returned. Again: we didn't need to explicitly define these defaults. Spring provides them "out of the box".

We defined the default sort parameter in our own code, by passing a Sort object to getSortOr():

Sort.by(Sort.Direction.ASC, "amount")
The net result is that if any of the three required parameters are not passed to the application, then reasonable defaults will be provided.

Run the tests... again!

Congratulations!

Everything's passing now.

[~/exercises] $ ./gradlew test
...
BUILD SUCCESSFUL in 7s