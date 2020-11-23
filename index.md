title: Road to efficient testing
class: animation-fade
layout: true

.bottom-bar[
  {{title}}
]

---

class: impact cover

# {{title}}
## Nov. 24, 2020

---
<div style="text-align: center">
<img src="img/me.png" style="width: 25%"/><br />
<br />
<strong>Laurent VOULLEMIER</strong>
<br />
<br />
@Sensiolabs<br /><br />
Team P50/MiddleOffice
</div>
---

class: impact subcover

# Unit tests

---

# What is an unit test ?

<blockquote>
Unit tests are typically automated tests written and run by software developers to ensure that a section of an application (known as the "unit") meets its design and behaves as intended. (Wikipedia)  
</blockquote>
--
<blockquote>
Unit tests test individual units (modules, functions, classes) in isolation from the rest of the program. (Kent beck)  
</blockquote>
--
<blockquote>
The term "unit test" is unfortunate.  A unit is a small element of structure.  The TDD discipline causes us to write tests for small elements of behavior; not small elements of structure.  So a test per class is inappropriate. (Robert C. Martin)
</blockquote>
---

# F.I.R.S.T. principles

An unit test should be...
--

- **F**ast

--
- **I**solated

--
- **R**epeatable

--
- **S**elf-validating

--
- **T**imely
  
--
  
<br />
<br />
https://howtodoinjava.com/best-practices/first-principles-for-good-tests/
---
# Test a behavior, not an implementation

<p style="text-align: center">
<img src="img/code1.png" data-src="https://carbon.now.sh/?bg=rgba%28243%2C243%2C243%2C0%29&t=a11y-dark&wt=none&l=text%2Fx-php&ds=true&dsyoff=10px&dsblur=26px&wc=false&wa=true&pv=56px&ph=56px&ln=true&fl=1&fm=Hack&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=class%2520EditArticleVoter%2520extends%2520Voter%250A%257B%250A%2520%2520protected%2520function%2520supports%28string%2520%2524attribute%252C%2520%2524subject%29%253A%2520bool%250A%2520%2520%257B%250A%2520%2520%2520%2520return%2520%27EDIT%27%2520%253D%253D%253D%2520%2524attribute%2520%2526%2526%2520%2524subject%2520instanceof%2520Article%253B%2520%2520%2520%250A%2520%2520%257D%250A%2520%2520%250A%2520%2520%252F**%250A%2520%2520%2520*%2520%2540param%2520Article%2520%2524article%250A%2520%2520%2520*%252F%250A%2520%2520protected%2520function%2520voteOnAttribute%28string%2520%2524attribute%252C%2520%2524article%252C%2520TokenInterface%2520%2524token%29%253A%2520bool%250A%2520%2520%257B%250A%2520%2520%2520%2520%2524user%2520%253D%2520%2524token-%253EgetUser%28%29%253B%250A%2520%2520%2520%2520%250A%2520%2520%2520%2520if%2520%28%21%2524user%2520instanceof%2520User%29%2520%257B%250A%2520%2520%2520%2520%2520%2520return%2520false%253B%250A%2520%2520%2520%2520%257D%250A%2520%2520%2520%2520%250A%2520%2520%2520%2520return%2520%2524article-%253EisPublic%28%29%2520%257C%257C%2520%2524article-%253EgetAuthor%28%29%2520%253D%253D%253D%2520%2524user%253B%2520%250A%2520%2520%257D%2520%2520%250A%257D" style="width: 90%; margin-top: -50px;"/>
</p>

???
Explain how voters work in Symfony
---
# Test a behavior, not an implementation

<p style="text-align: center">
<img src="img/code2.png" data-src="https://carbon.now.sh/?bg=rgba%28243%2C243%2C243%2C0%29&t=a11y-dark&wt=none&l=text%2Fx-php&ds=true&dsyoff=10px&dsblur=26px&wc=false&wa=true&pv=56px&ph=56px&ln=true&fl=1&fm=Hack&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=class%2520EditArticleVoterTest%2520extends%2520TestCase%250A%257B%250A%2520%2520public%2520function%2520testAuthenticatedUserCanEditPublicArticle%28%29%250A%2520%2520%257B%250A%2520%2520%2520%2520%2524article%2520%253D%2520new%2520Article%28%29%253B%250A%2520%2520%2520%2520%2524article-%253EsetPublic%28true%29%253B%250A%2520%2520%2520%2520%250A%2520%2520%2520%2520%2524token%2520%253D%2520new%2520UsernamePasswordToken%28new%2520User%28%29%252C%2520%27%27%252C%2520%27%27%29%253B%250A%2520%2520%2520%2520%250A%2520%2520%2520%2520%2524voter%2520%253D%2520new%2520EditArticleVoter%28%29%253B%250A%2520%2520%2520%2520%2524vote%2520%253D%2520%2524voter-%253Evote%28%27EDIT%27%252C%2520%2524article%252C%2520%2524token%29%253B%250A%2520%2520%2520%2520%250A%2520%2520%2520%2520%2524this-%253EassertTrue%28%2524vote%29%253B%250A%2520%2520%257D%250A%2520%2520%250A%2520%2520%252F%252F...%250A%257D" style="width: 80%; margin-top: -60px;"/>
</p>

???
- No mock (don't mock what you don't own)
- Protected methods are not tested directly
---
# How to test a configuration ?

<p style="text-align: center">
<img src="img/code3.png" data-src="https://carbon.now.sh/?bg=rgba%28243%2C243%2C243%2C0%29&t=a11y-dark&wt=none&l=text%2Fx-php&ds=true&dsyoff=10px&dsblur=26px&wc=false&wa=true&pv=56px&ph=56px&ln=true&fl=1&fm=Hack&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=class%2520NotifyArticlePublished%2520implements%2520EventSubscriberInterface%250A%257B%250A%2520%2520%252F%252F...%250A%2520%2520%250A%2520%2520public%2520function%2520onArticlePublished%28ArticleEvent%2520%2524event%29%253A%2520void%250A%2520%2520%257B%250A%2520%2520%2520%2520%2524article%2520%253D%2520%2524event-%253EgetArticle%28%29%253B%250A%2520%2520%2520%2520%2524mail%2520%253D%2520%252F%252F...%253B%250A%2520%2520%2520%2520%2524this-%253Emailer-%253Esend%28%2524mail%29%253B%2520%2520%250A%2520%2520%257D%250A%2520%2520%250A%2520%2520%252F%252F%2520How%2520to%2520test%2520this%2520%253F%250A%2520%2520public%2520static%2520function%2520getSubscribedEvents%28%29%253A%2520array%250A%2520%2520%257B%250A%2520%2520%2520%2520return%2520%255BArticleEvents%253A%253APUBLISHED%2520%253D%253E%2520%27onArticlePublished%27%255D%253B%250A%2520%2520%257D%2520%2520%250A%257D" style="width: 80%; margin-top: -60px;"/>
</p>

???
getSubscribedEvents: other events can be added, priorities can be added... Test should not break
---
# Configuration may be a part of the behavior

<p style="text-align: center">
<img src="img/code4.png" data-src="https://carbon.now.sh/?bg=rgba%28243%2C243%2C243%2C0%29&t=a11y-dark&wt=none&l=text%2Fx-php&ds=true&dsyoff=10px&dsblur=26px&wc=false&wa=true&pv=56px&ph=56px&ln=true&fl=1&fm=Hack&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=class%2520NotifyArticlePublishedTest%2520extends%2520TestCase%250A%257B%250A%2520%2520public%2520function%2520onArticlePublished%28ArticleEvent%2520%2524event%29%253A%2520void%250A%2520%2520%257B%250A%2520%2520%2520%2520%2524expectedEmail%2520%253D%2520%252F*...*%252F%253B%250A%2520%2520%2520%2520%2524mailer%2520%253D%2520%2524this-%253EcreateMock%28MailerInterface%253A%253Aclass%29%253B%250A%2520%2520%2520%2520%2524mailer%250A%2520%2520%2520%2520%2520%2520-%253Eexpects%28%2524this-%253Eonce%28%29%29%250A%2520%2520%2520%2520%2520%2520-%253Ewith%28%2524expectedEmail%29%253B%2520%2520%2520%250A%2520%2520%2520%2520%250A%2520%2520%2520%2520%2524dispatcher%2520%253D%2520new%2520EventDispatcher%28%29%253B%250A%2520%2520%2520%2520%2524dispatcher-%253EaddSubscriber%28new%2520NotifyArticlePublished%28%29%29%253B%250A%2520%2520%2520%2520%250A%2520%2520%2520%2520%2524article%2520%253D%2520%252F*...*%252F%253B%250A%2520%2520%2520%2520%2524event%2520%253D%2520new%2520ArticleEvent%28%2524article%29%253B%250A%2520%2520%2520%2520%2524dispatcher-%253Edispatch%28ArticleEvents%253A%253APUBLISHED%252C%2520%2524event%29%253B%2520%2520%2520%250A%2520%2520%257D%250A%257D" style="width: 70%; margin-top: -60px;"/>
</p>
---
# 100% code coverage ? Unit test everything ?
<br />
- Code coverage is a tool. It should not be the goal.
- Unit test low-complexity methods may be discussed.
- @covers annotation can help to mark as covered only code that is actually tested.
<br />  
<br />  
> If your unit tests cover 100% of your code, you're doing it wrong. Just my 2 cts. (Fabien Potencier)
???
- Controllers should not be unit tested
- Static analysis now replaces some basic tests
---
# Change Risk Anti-Patterns (CRAP) index
  
<br />
<br />  
![Crap index](img/crap.png)
<br />
<br />
<br />
<br />
https://opnsrce.github.io/how-to-read-and-improve-the-c-r-a-p-index-of-your-code
---
class: impact subcover

# Integration tests
---
# Unit tests are green

<img src="img/tests.gif" style="width: 50%"/>

???
But e2e and integration tests are not
---
# Test types
<br /><br />
<img src="img/test-types.png" style="width: 50%"/>

???
Service tests = Integration tests
---
# From here, external actors are allowed
<br />
<br />  

- Database
- Filesystem
- ...
  
<br />
But fake objects can also be used (fake api client, fake mail server...)
---
# Fixtures

⚠️ Avoid to share fixtures between tests
  
## Tooling
- Faker / Alice fixtures (rely on Faker) / Doctrine fixtures  

## Speed up database fixtures
- In memory database (sqlite)/In memory filesystem (tmpfs)
- Transactions (Doctrine test bundle)
- Database dumps / Read only fixtures (but involves to have shared fixtures)  
  
⚠️ don't rely on auto-generated identifiers

???
- Generating and loading fixtures are often the bottleneck
- Avoid to share fixtures: a common base is possible but the base should be stable
- Don't rely on auto-generated identifiers: generators are incremented even when the transaction is cancelled (mysql, postgres...).
---
class: impact subcover

# End to end tests
---
# Rules of thumb
<br />
<br />
--

- Think like an end-user, user stories and acceptance tests are a good points to start
<br />
<br />
--

- Keep in mind critical paths to test (risk-based testing)
<br />
<br />
--

- Test UI against stable elements (find by aria roles, labels) 
      
---
# Gerkhin
<br />
<br />
--

- **Given**: set the application state, magic only happens here
<br />
<br />
--

- **When**: the user action
<br />
<br />
--

- **Then**: inspect the output of the system (UI) not something deeply buried inside (database state...) 

???
Common sense: don't test with a real browser if it's not needed
---
# Some other test types
<br />
<br />
- Mutation testing (Infection...)
  
- Performance tests (Blackfire...)
  
- Stress tests (Gatling...)
  
- Visual tests (Phantomcss...)
---
class: impact subcover

# Thank you for your attention.



