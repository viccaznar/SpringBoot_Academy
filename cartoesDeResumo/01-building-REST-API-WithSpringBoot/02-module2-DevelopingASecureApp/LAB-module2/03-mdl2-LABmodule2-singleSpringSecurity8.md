Traduza integralmente o seguinte texto:

8: Cash Card ownership: Repository Updates
As mentioned in the previous exercise, we have a glaring security hole in our application.

Any authenticated user with role CARD-OWNER can view anyone else's Family Cash Cards!

To fix this, we'll update our tests, CashCardRepository, and CashCardController:

We'll add functionality to CashCardRepository to restrict or "scope" queries to the correct OWNER.
Next, we'll update our CashCardController to guarantee that only the correct OWNER is used.
Learning Moment: Best Practices
Wait! Isn't this lab all about the Spring Security project and its amazing technology? Can't Spring Security do all of this ownership validation so we don't have to modify our Repositories and Controllers?

Answer: This lab and associated lesson are about securing an HTTP API. Yes, security technologies such as Spring Security are amazing. Yes, there are features such as Spring Security Method Security that might help in this situation, but it's still your responsibility as a developer to write secure code and follow best security practices. For example, don't write code that allows users to access other users' data!

Now, let's update our tests and CashCardRepository.

Add a new CashCard for a user named kumar2.

Update src/test/resources/data.sql with a CashCard record owned by a different user:

...
INSERT INTO CASH_CARD(ID, AMOUNT, OWNER) VALUES (102, 200.00, 'kumar2');
Test that users cannot access each other's data.

Let's add a test that explicitly asserts that our API returns a 404 NOT FOUND when a user attempts to access a Cash Card they do not own.

Note: You might wonder why we want to return a 404 NOT FOUND response instead of something else, like 401 UNAUTHORIZED. One argument in favor of choosing to return NOT FOUND is that it's the same response that we'd return if the requested Cash Card doesn't exist. It's safer to err on the side of not revealing any information about data which isn't authorized for the user.

Now we'll have sarah1 attempt to access kumar2's data.

@Test
void shouldNotAllowAccessToCashCardsTheyDoNotOwn() {
    ResponseEntity<String> response = restTemplate
      .withBasicAuth("sarah1", "abc123")
      .getForEntity("/cashcards/102", String.class); // kumar2's data
    assertThat(response.getStatusCode()).isEqualTo(HttpStatus.NOT_FOUND);
}
Run the tests.

What do you think will happen when we run the tests? Let's find out.

As expected, our new test fails, along with many others.

CashCardApplicationTests > shouldNotAllowAccessToCashCardsTheyDoNotOwn() FAILED
 org.opentest4j.AssertionFailedError:
 expected: 404 NOT_FOUND
  but was: 200 OK
What's going on here?

Answer: Currently, user sarah1 is able to view kumar2's data because:

sarah1 is authenticated.
sarah1 is an authorized CARD-OWNER.
In addition, our test for fetching a list of CashCards is also failing:

CashCardApplicationTests > shouldReturnAllCashCardsWhenListIsRequested() FAILED
 org.opentest4j.AssertionFailedError:
 expected: 3
  but was: 4
Why are we returning too many Cash Cards? For the same reason as above: sarah1 has access to kumar2's data. kumar2's Cash Card is being returned to sarah1's list of Cash Cards.

Let's prevent users from accessing each other's data.

Update the CashCardRepository with new findBy... methods.

The simplest thing we can do is to always filter our data access by CashCard owner.

Let's do that in our Repository.

We'll need to filter by owner when finding both a single CashCard or a list of CashCards.

Edit CashCardRepository to add two new finder methods.

Be sure to include the new imports for Page and PageRequest.

...
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
...

interface CashCardRepository extends CrudRepository<CashCard, Long>, PagingAndSortingRepository<CashCard, Long> {
   CashCard findByIdAndOwner(Long id, String owner);
   Page<CashCard> findByOwner(String owner, PageRequest pageRequest);
}
As mentioned in the Spring Data labs and lessons, Spring Data will take care of the actual implementations (writing the SQL queries) for us.

Note: You might wonder whether Spring Data allows you to write your own SQL. After all, Spring Data can't anticipate every need, right? The answer is Yes! It's easy for you to write your own SQL code. The Spring Data Query Methods documentation describes how to do so by using the @Query annotation.

Spring Data is perfectly capable of generating the SQL for the queries we need, though. Thanks, Spring Data!

Next, let's update the Controller.