Traduza o seguinte texto de forma integral:

3: Implement the DELETE Endpoint
Now we need to write a Controller method which will be called when we send a DELETE request with the proper URI.

Add code to the Controller to delete the record.

Change the CashCardController.deleteCashCard() method:

@DeleteMapping("/{id}")
private ResponseEntity<Void> deleteCashCard(@PathVariable Long id) {
    cashCardRepository.deleteById(id); // Add this line
    return ResponseEntity.noContent().build();
}
The change is straightforward:

We use the @DeleteMapping with the "{id}" parameter, which Spring Web matches to the id method parameter.
CashCardRepository already has the method we need: deleteById() (it's inherited from CrudRepository).
Run the tests, and watch them pass!

[~/exercises] $ ./gradlew test
...
BUILD SUCCESSFUL in 8s
Great, what to do next?