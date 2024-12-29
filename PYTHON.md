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

> The following example is done in Python, but Ultra Mock supports any class-based language : [PHP](README.md), Java, C#, JavaScript, etc.

```python
from datetime import datetime

class Clock:
    def now(self) -> datetime:
        return datetime.now()
```

Start by adding a Protocol.

```python
from datetime import datetime
from typing import Protocol

class Clock(Protocol):
    def now(self) -> datetime:
        ...
```

Now implement this new Protocol on your old class. Name your old class according to some property that is unique to its implementation. Try to be clever with the naming.

> Being clever is a sign that you're thinking about your situation in a unique way. It's fun to show your colleagues that you're not on auto-pilot! Don't worry, this gets easier with practice!

```python
from datetime import datetime

class SystemClock(Clock):
    def now(self) -> datetime:
        return datetime.now()
```

OK, we're almost done! The final step is implementing your mock.

> Note: Don't forget to make your life easier by implementing methods that aren't on the interface! Another great opportunity to be clever!

```python
from datetime import datetime

class FixedClock(Clock):
    def __init__(self, time: datetime):
        self.time = time

        return self

    @staticmethod
    def set_to(time_string: str) -> self:
        return FixedClock(datetime.strptime(time_string, '%Y-%m-%d %H:%M:%S'))

    def now(self) -> datetime:
        return self.time
```

### Expectations

What's a mock without expectations?! Ultra Mock has you covered.

> Note: Ultra Mock is not only the simplest and most powerful mocking framework. But it's also completely compatible with Python's built-in assert statement.

```python
class SendEmailMock(SendEmail):
    def __init__(self):
        self.received_email = None

    def send(self, email: Email):
        self.received_email = email

    def assert_received(self, email: Email):
        assert self.received_email == email
```

### Bringing It All Together

Now that we set up our Ultra Mocks, let's see how a test might function.

```python
from datetime import datetime


def test_sends_email_on_registration():
    send_email = SendEmailMock()
    handler = SendEmailOnRegistration(send_email)

    user_email = Email.from_address('shawn@mccool.email')
    handler.handle(UserWasRegistered(User(user_email)))

    send_email.assert_received(user_email)
```

That's it!

We hope you enjoy using Ultra Mock to prevent your tests from being full of heavily duplicated implementation details!
