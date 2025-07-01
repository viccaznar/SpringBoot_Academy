Traduza o seguinte texto de forma integral:

6: Refactor
At this point, we have an opportunity to practice the Red, Green, Refactor process. We've already done Red (the failing test), and Green (the passing test). Now we can ask ourselves, should we Refactor anything?

Here's the body of our CashCardController.deleteCashCard method:

...
if (!cashCardRepository.existsByIdAndOwner(id, principal.getName())) {
    return ResponseEntity.notFound().build();
}
cashCardRepository.deleteById(id);
return ResponseEntity.noContent().build();
You might find the following version, which is logically equivalent but slightly simpler, to be easier to read:

if (cashCardRepository.existsByIdAndOwner(id, principal.getName())) {
    cashCardRepository.deleteById(id);
    return ResponseEntity.noContent().build();
}
return ResponseEntity.notFound().build();
The differences are slight, but removing a not-operator (!) from an if statement often makes for easier-to-read code, and readability is important!

If you do find the second version easier to read and understand, then replace the existing code with the new version.

Run the tests again. They still pass!

[~/exercises] $ ./gradlew test
...
BUILD SUCCESSFUL in 8s