# Short Responses

For this short response assignment, aim to write a response with the following
qualities (your instructor will give you feedback on these areas):

- [ ] Addresses all parts of the prompt
- [ ] Accurately uses relevant technical terminology
- [ ] Is free of grammar and spelling mistakes (double check with Grammarly!)
- [ ] Uses markdown to enhance readability (preview in VS Code with
      Command/Control + Shift + V)
- [ ] Is easy to comprehend

Write your response in the space provided. Make sure to preview your markdown
to check how it is rendered before submitting.

## Question 1

```py
def requires_paid(method):
    def wrapper(self):
        if self.status != "paid":
            raise ValueError(f"{method.__name__} needs a paid rental.")
        return method(self)
    return wrapper


class Rental:
    @requires_paid
    def collect(self):
        self.status = "collected"
```

By the time anyone calls `rental.collect()`, `requires_paid` finished running
long ago — it ran once, when the class was being defined.

**First**, name the variable `wrapper` reads that does not belong to it. Say
which function it belongs to.

**Second**, explain why `wrapper` can still read it. `requires_paid` has
returned, so its local variables should be gone. Say what keeps that one alive.

**Third**, `rental.collect()` — there is no `method` argument anywhere in that
call, and `wrapper` never received one. Trace where the value of `method` comes
from, starting at the line that put it there.

**Fourth**, answer this. Suppose `Rental` had a second guarded method:

```py
    @requires_paid
    def refund(self):
        self.status = "refunded"
```

Say how many wrappers now exist, and for each one, what its `method` holds. The
answer is the reason this works at all.

### Your Response

[Write your response here]

## Question 2

A plain decorator is applied with no call:

```py
@requires_paid
def collect(self): ...
```

A **decorator factory** is applied with one:

```py
@requires_status("paid")
def check_in(self): ...
```

Flask, which you use in Mod 3, writes every route with the second form:
`@app.route("/")`.

**First**, name the **two steps** Python performs for `@requires_status("paid")`.
For each step, say what is called and what comes back. One of them happens
because of the parentheses and one because of the `@`.

**Second**, say how many functions deep `requires_status` has to be to work as
written, and what each level is for.

**Third**, here is the mistake. Somebody writes:

```py
    @requires_status
    def check_in(self): ...
```

No parentheses. Explain what `requires_status` receives as its `status`
argument, what gets bound to the name `check_in`, and **when** anybody finds
out. Name the specific call that finally fails, and say whether the error
message would point you at this line.

That last part is the reason this question exists. Both forms are legal Python
and only one is what you meant.

### Your Response

[Write your response here]

## Question 3

This question asks you to **write test code**, not prose. Project requirement 9
asks for exactly this, on your own system.

```py
def requires_paid(method):
    def wrapper(self):
        if self.status != "paid":
            raise ValueError(f"{method.__name__} needs a paid rental.")
        return method(self)
    return wrapper


class Rental:
    def __init__(self, bike):
        self.bike = bike
        self.status = "reserved"
        self.history = []

    @requires_paid
    def collect(self):
        self.status = "collected"
        self.history.append("collected")
```

Write **one** test proving that calling `collect()` on a reserved rental raises
`ValueError` **and leaves the object exactly as it was**. Use
`pytest.raises(ValueError)`.

Use `src/playground.py` in the code repo to run it, then paste it here.

A refusal that raises but half-ran is worse than no refusal, because the error
says it was refused while the object says otherwise. So your test checks the
status **and** the collection.

Then answer two things:

1. **Name the broken version of `requires_paid` your test would catch** that a
   test checking only `pytest.raises(ValueError)` would pass. Write the few
   lines of it.
2. The guard raises **before** calling the method. Say what would have to change
   about `wrapper` for the status to move and the error still to be raised, and
   what a caller would see afterwards.

The second part is the point. You are being asked to say why the order of two
lines inside the wrapper is the whole guarantee.

### Your Response

[Write your response here]

