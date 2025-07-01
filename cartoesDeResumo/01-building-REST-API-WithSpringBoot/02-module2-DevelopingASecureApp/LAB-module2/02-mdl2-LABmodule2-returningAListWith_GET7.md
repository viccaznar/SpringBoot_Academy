Traduza integralmente o seguinte texto:

7: Sorting
We'd like the Cash Cards to come back in an order that makes sense to humans. So let's order them by amount in a descending order with the highest amounts first.

Write a test (which we expect to fail).

Add the following test to CashCardApplicationTests:

@Test
void shouldReturnASortedPageOfCashCards() {
    ResponseEntity<String> response = restTemplate.getForEntity("/cashcards?page=0&size=1&sort=amount,desc", String.class);
    assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);

    DocumentContext documentContext = JsonPath.parse(response.getBody());
    JSONArray read = documentContext.read("$[*]");
    assertThat(read.size()).isEqualTo(1);

    double amount = documentContext.read("$[0].amount");
    assertThat(amount).isEqualTo(150.00);
}
Understand the test.

The URI we're requesting contains both pagination and sorting information: /cashcards?page=0&size=1&sort=amount,desc

page=0: Get the first page. Page indexes start at 0.
size=1: Each page has size 1.
sort=amount,desc
The extraction of data (using more JSONPath!) and accompanying assertions expect that the returned Cash Card is the $150.00 one.

Do you think the test will pass? Before running it, try to figure out whether it will or not. If you think it won't pass, where do you think the failure will be?

Run the test.

[~/exercises] $ ./gradlew test
...
org.opentest4j.AssertionFailedError:
 expected: 150.0
  but was: 123.45
The test expected to get the $150.00 Cash Card, but it got the $123.45 one. Why?

The reason is that since we didn't specify a sort order, the cards are returned in the order they are returned from the database. And this happens to be the same as the order in which they were inserted.

An important observation: Not all databases will act the same way. Now, it should make even more sense why we specify a sort order (instead of relying on the database's default order).

Implement sorting in the Controller.

Adding sorting to the Controller code is a super simple single line addition. In the CashCardController class, add an additional parameter to the PageRequest.of() call:

PageRequest.of(
     pageable.getPageNumber(),
     pageable.getPageSize(),
     pageable.getSort()
));
The getSort() method extracts the sort query parameter from the request URI.

Run the tests again. They pass!

CashCardApplicationTests > shouldReturnAllCashCardsWhenListIsRequested() PASSED
Learn by breaking things.

To get a little more confidence in the test, let's do an experiment.

In the test, change the sort order from descending to ascending:

ResponseEntity<String> response = restTemplate.getForEntity("/cashcards?page=0&size=1&sort=amount,asc", String.class);
This should cause the test to fail because the first Cash Card in ascending order should be the $1.00 card. Run the tests and observe the failure:

CashCardApplicationTests > shouldReturnASortedPageOfCashCards() FAILED
org.opentest4j.AssertionFailedError:
 expected: 150.0
  but was: 1.0
Correct! This result reinforces our confidence in the test. Instead of writing a whole new test, we used an existing one to run a little experiment.

Now let's change the test back to request descending sort order so that it passes again.