Traduza integralmente o seguinte texto:

2: Add the POST endpoint
The POST endpoint is similar to the GET endpoint in our CashCardController, but uses the @PostMapping annotation from Spring Web.

The POST endpoint must accept the data we are submitting for our new CashCard, specifically the amount.

But what happens if we don't accept the CashCard?

Add the POST endpoint without accepting CashCard data.

Edit src/main/java/example/cashcard/CashCardController.java and add the following method.

Don't forget to add the import for PostMapping.

import org.springframework.web.bind.annotation.PostMapping;
...

@PostMapping
private ResponseEntity<Void> createCashCard() {
   return null;
}
Note that by returning nothing at all, Spring Web will automatically generate an HTTP Response Status code of 200 OK.

Run the tests.

When we rerun the tests, they pass.

BUILD SUCCESSFUL in 7s
But, this isn't very satisfying -- our POST endpoint does nothing!

So let's make our tests better.