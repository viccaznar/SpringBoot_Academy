Traduza integralmente o seguinte texto:

1: Test the HTTP POST Endpoint
As we've done in previous labs, we'll begin by writing a test of what we expect success to look like.

Add a test for the POST endpoint.

The simplest example of success is a non-failing HTTP POST request to our Family Cash Card API. We'll test for a 200 OK response instead of a 201 CREATED for now. Don't worry, we'll change this soon.

Edit src/test/java/example/cashcard/CashCardApplicationTests.java and add the following test method.

@Test
void shouldCreateANewCashCard() {
   CashCard newCashCard = new CashCard(null, 250.00);
   ResponseEntity<Void> createResponse = restTemplate.postForEntity("/cashcards", newCashCard, Void.class);
   assertThat(createResponse.getStatusCode()).isEqualTo(HttpStatus.OK);
}
Understand the test.

CashCard newCashCard = new CashCard(null, 250.00);
The database will create and manage all unique CashCard.id values for us. We shouldn't provide one.

restTemplate.postForEntity("/cashcards", newCashCard, Void.class);
This is very similar to restTemplate.getForEntity, but we must also provide newCashCard data for the new CashCard.

In addition, and unlike restTemplate.getForEntity, we don't expect a CashCard to be returned to us, so we expect a Void response body.

Run the tests.

We'll always use ./gradlew test to run our tests.

[~/exercises] $ ./gradlew test
What do you expect will happen?

CashCardApplicationTests > shouldCreateANewCashCard() FAILED
   org.opentest4j.AssertionFailedError:
   expected: 200 OK
   but was: 404 NOT_FOUND
We shouldn't be surprised by the 404 NOT_FOUND error. We haven't added the POST endpoint yet!

Let's do that next.