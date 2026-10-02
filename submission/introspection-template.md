# Design Introspection

Your own reflection on the design decisions you made this week. Written in your own words — this is
distinct from the LLM's assessment of your code, and distinct from your code-walk video.

Aim for a page. Cite your actual code: name the class and method you are talking about.

## 1. What you built

In two or three sentences: which types you wrote this week, and what each one is responsible for.

This week we built the AgeMonths and Animal objects. AgeMonths being a way for us to track the age of the animals we are representing in the Animal class. We built the toString methods in each of these to cleanly print the details stored as well as basic constructors and getters.

## 2. Design decisions

Pick the two or three decisions you actually had to think about, and for each one:

- **What was the choice?** What were the alternatives you considered?
- **What did you pick, and why?** What would have gone wrong with the other option?
- **What did it cost?** Every real design decision costs something.

Good candidates: where you put a piece of behaviour and why it belongs there rather than somewhere
else; how you represented something so that an invalid version could not be built; where you chose to
delegate to existing code rather than re-deriving an answer.

1 was the to string for AgeMonths in which i ended up going with a switch implementation, I started first doing one with if statements but it seemed messier so I moved it to the switches. It cost a little more time, I had to refresh on how the syntax was for switches but i feel the solution is cleaner.

2 i guess the test that i added, i had to think a lot about which cases were not covered and went through the edge cases as well as the error handling more in depth and chose which gaps i saw were missing. I picked the ones i did because these were the gaps i saw in the current testing framework. it cost time again.

## 3. Invariants

What does your code guarantee about itself, and where is each guarantee enforced?

For each type that validates its input: what must always be true of an instance once it exists, and
which line makes that true? If a guarantee is enforced in more than one place, say why — and whether
that is deliberate or duplication.

Each of the inputs is the correct type and for certain inputs like Age fit into the constraints we had planned.

```java
  public static AgeMonths of(int months) {
    if (months < 0) {
      throw new IntakeException("age in months cannot be negative, was " + months);
    }
    if (months > MAX_MONTHS) {
      throw new IntakeException("age in months cannot exceed " + MAX_MONTHS + ", was " + months);
    }
    return new AgeMonths(months);
  }
```

here it is for the AgeMonths

The rest of them are mosly just not null checks or type checks

## 4. Testing

- Which cases did you add beyond the provided tests, and what made you think of them?
- Which test was hardest to write, and what did writing it teach you about your own design?
- What is still untested, and how would you test it if you had another hour?

I added edge cases, basically the border cases in each direction, and error handling mostly.
None were really that difficult.
Maybe i would try more output combinations of the Animal to String method.

## 5. What you would change

Given another day, what would you do differently — and what stopped you this week? Be specific;
"write more tests" is not an answer.

Maybe the toString for the Agemonths, i feel as though there is still some repetition and it could be cleaner.

## 6. What you found hard

The honest one. What took the longest, what did you get wrong first, and what finally made it click?
This is not marked on whether you struggled — everyone does — but on whether you can say clearly
where and why.
I guess figureing out which more tests i could add, this took a while to brainstorm.
