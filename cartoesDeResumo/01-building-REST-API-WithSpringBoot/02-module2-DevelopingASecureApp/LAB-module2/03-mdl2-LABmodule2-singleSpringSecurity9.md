Traduza integralmente o seguinte texto:

9: Cash Card ownership: Controller Updates
The CashCardRepository now supports filtering CashCard data by owner.

But we're not using this new functionality. Let's make those updates to the CashCardController now by introducing a concept we explained in the written lesson: the Principal.

As with other helpful objects, the Principal is available for us to use in our Controller. The Principal holds our user's authenticated, authorized information.

Update the Controller's GET by ID endpoint.

Update the CashCardController to pass the Principal's information to our Repository's new findByIdAndOwner method.

Be sure to add the new import statement.

import java.security.Principal;
...

@GetMapping("/{requestedId}")
private ResponseEntity<CashCard> findById(@PathVariable Long requestedId, Principal principal) {
    Optional<CashCard> cashCardOptional = Optional.ofNullable(cashCardRepository.findByIdAndOwner(requestedId, principal.getName()));
    if (cashCardOptional.isPresent()) {
     ...
Note that principal.getName() will return the username provided from Basic Auth.

Run the tests.

The GET is passing, but our tests for Cash Card lists are failing.

CashCardApplicationTests > shouldReturnASortedPageOfCashCards() FAILED
...
CashCardApplicationTests > shouldReturnACashCardWhenDataIsSaved() PASSED
...
CashCardApplicationTests > shouldReturnAllCashCardsWhenListIsRequested() FAILED
...
CashCardApplicationTests > shouldReturnASortedPageOfCashCardsWithNoParametersAndUseDefaultValues() FAILED
...
Update the Controller's GET for lists endpoint.

Edit CashCardController to filter lists by owner.

@GetMapping
private ResponseEntity<List<CashCard>> findAll(Pageable pageable, Principal principal) {
    Page<CashCard> page = cashCardRepository.findByOwner(principal.getName(),
            PageRequest.of(
                pageable.getPageNumber(),
                ...
Once again we get the authenticated username from the principal.getName() method.

Run the tests.

They all pass!

BUILD SUCCESSFUL in 8s
Looking good!