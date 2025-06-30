Traduza integralmente o seguinte texto:

4: Implement the POST Endpoint
Our POST endpoint in the CashCardController is currently empty. Let's implement the correct logic.

Return a 201 CREATED status.

As we incrementally make our test pass, we can start by returning 201 CREATED.

As we learned earlier, we must provide a Location header with the URI for where to find the newly created CashCard. We're not quite there yet, so we'll use a placeholder URI for now.

Be sure to add the two new import statements.

import java.net.URI;
import org.springframework.web.bind.annotation.RequestBody;
...

 @PostMapping
 private ResponseEntity<Void> createCashCard(@RequestBody CashCard newCashCardRequest) {
     return ResponseEntity.created(URI.create("/what/should/go/here?")).build();
 }
Run the tests.

expected: 200 OK
 but was: 404 NOT_FOUND
Remarkably, our new test passes until the last line of the new test.

...
assertThat(getResponse.getStatusCode()).isEqualTo(HttpStatus.OK);
Here we expect to have retrieved our newly created CashCard, which we haven't created or returned from our CashCardController. Thus, our expectation fails with a result of NOT_FOUND.

Save the new CashCard and return its location.

Let's add the rest of the POST implementation, which we will describe in detail.

Be sure to add the new import.

import org.springframework.web.util.UriComponentsBuilder;
...

@PostMapping
private ResponseEntity<Void> createCashCard(@RequestBody CashCard newCashCardRequest, UriComponentsBuilder ucb) {
   CashCard savedCashCard = cashCardRepository.save(newCashCardRequest);
   URI locationOfNewCashCard = ucb
            .path("cashcards/{id}")
            .buildAndExpand(savedCashCard.id())
            .toUri();
   return ResponseEntity.created(locationOfNewCashCard).build();
}
Next we'll go over these changes in detail.