# Ultra Mock

Ultra Mock is the most powerful and simple to use mocking framework. All major class-based programming languages are supported.

With small considerations in your application code, you can mock any boundary and achieve incredible performance and developer usability.

## Getting Started

Installing Ultra Mock is easy, just execute the following steps:

1. decide that you're fed up with complicated solutions that are out of your control
2. understand that this is a solved problem that many of your peers already figured out
3. be ready to receive criticism from people who have not followed step 1

## Usage

Using Ultra Mock is simple. Start by identifying side-effects boundaries.

> **What is a side-effect?** A side-effect is any interaction with something besides function or constructor arguments. (reading from clocks, writing to the database, etc)

### Example

We would like to mock the following class with Ultra Mock.

> The following example is done in PHP, but Ultra Mock supports any class-based language : [Python](PYTHON.md), Java, C#, JavaScript, etc.

```PHP
class Clock
{
  public function now(): DateTime {
    return new DateTime('now');
  }
}
```

Start by adding an interface.

```PHP
interface Clock
{
  public function now(): DateTime;
}

```

Now implement this new interface on your old class. Name your old class according to some property that is unique to its implementation. Try to be clever with the naming.

> Being clever is a sign that you're thinking about your situation in a unique way. It's fun to show your colleagues that you're not on auto-pilot! Don't worry, this gets easier with practice!

```PHP
class SystemClock implements Clock
{
  public function now(): DateTime {
    return new DateTime('now');
  }
}
```

OK, we're almost done! The final step is implementing your mock.

> Note: Don't forget to make your life easier by implementing methods that aren't on the interface! Another great opportunity to be clever!

```PHP
class FixedClock implements Clock {
  private DateTime $time;

  private function __construct(DateTime $time) {
    $this->time = $time;
  }

  public static function setTo(string $timeString) {
    return new self(new DateTime($timeString));
  }

  public function now(): DateTime {
    return $this->time;
  }
}
```

### Expectations

What's a mock without expectations?! Ultra Mock has you covered.

> Note: Ultra Mock is not only the simplest and most powerful mocking framework. But it's also completely compatible with any assertion framework you like.

```PHP
class SendEmailMock implements SendEmail
{
  private Email $receivedEmail;

  public function send(Email $email) {
    $this->receivedEmail = $email;
  }

  public function assertReceived(Email $email) {
    // your favorite assertion library here
  }
}
```

### Bringing It All Together

Now that we set up our Ultra Mocks, let's see how a test might function.

```PHP
public function testSendsEmailOnRegistration() {
  $sendEmail = new SendEmailMock;
  $handler = new SendEmailOnRegistration($sendEmail);

  $userEmail = Email::fromAddress("shawn@mccool.email");
  $handler->handle(new UserWasRegistered(new User($userEmail)));

  $sendEmail->assertReceived($userEmail);
}
```

That's it!

We hope you enjoy using Ultra Mock to prevent your tests from being full of heavily duplicated implementation details!
