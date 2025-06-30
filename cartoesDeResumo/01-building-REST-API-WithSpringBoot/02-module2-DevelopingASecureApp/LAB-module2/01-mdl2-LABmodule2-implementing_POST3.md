Traduza integralmente o seguinte texto:

3: Testing based on semantic correctness
We want our Cash Card API to behave as semantically correctly as possible. Meaning, users of our API shouldn't be surprised by how it behaves.

Let's refer to the official Request for Comments for HTTP Semantics and Content (RFC 9110) for guidance as to how our API should behave.

For our POST endpoint, review this section about HTTP POST; note that we've added emphasis:

If one or more resources has been created on the origin server as a result of successfully processing a POST request, the origin server SHOULD send a 201 (Created) response containing a Location header field that provides an identifier for the primary resource created ...

We'll explain more about this specification as we write our test.

Let's start by updating the POST test.

Update the shouldCreateANewCashCard test.

Here's how we'll encode the HTTP specification as expectations in our test. Be sure to add the additional import.

import java.net.URI;
...

@Test
void shouldCreateANewCashCard() {
   CashCard newCashCard = new CashCard(null, 250.00);
   ResponseEntity<Void> createResponse = restTemplate.postForEntity("/cashcards", newCashCard, Void.class);
   assertThat(createResponse.getStatusCode()).isEqualTo(HttpStatus.CREATED);

   URI locationOfNewCashCard = createResponse.getHeaders().getLocation();
   ResponseEntity<String> getResponse = restTemplate.getForEntity(locationOfNewCashCard, String.class);
   assertThat(getResponse.getStatusCode()).isEqualTo(HttpStatus.OK);
}
Understand the test updates.

We've made quite a few changes. Let's review.

assertThat(createResponse.getStatusCode()).isEqualTo(HttpStatus.CREATED);
According to the official specification:

the origin server SHOULD send a 201 (Created) response ...

We now expect the HTTP response status code to be 201 CREATED, which is semantically correct if our API creates a new CashCard from our request.

URI locationOfNewCashCard = createResponse.getHeaders().getLocation();
The official spec continues to state the following:

send a 201 (Created) response containing a Location header field that provides an identifier for the primary resource created ...

In other words, when a POST request results in the successful creation of a resource, such as a new CashCard, the response should include information for how to retrieve that resource. We'll do this by supplying a URI in a Response Header named "Location".

Note that URI is indeed the correct entity here and not a URL; a URL is a type of URI, while a URI is more generic.

ResponseEntity<String> getResponse = restTemplate.getForEntity(locationOfNewCashCard, String.class);
assertThat(getResponse.getStatusCode()).isEqualTo(HttpStatus.OK);
Finally, we'll use the Location header's information to fetch the newly created CashCard.

Run the tests.

Unsurprisingly, they fail on the first changed assertion.

expected: 201 CREATED
  but was: 200 OK
Let's start fixing stuff!