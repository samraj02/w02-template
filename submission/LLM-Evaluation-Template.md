# LLM Evaluation

Produced with an LLM using the prompt in `LLM-Evaluation-prompt.md`, then reviewed and answered by
you. **Both halves are required** — an unread LLM assessment pasted in whole is worth nothing.

---

## Assessment

Paste the LLM's assessment here, in full and unedited.

Its scoring categories are the ones in `LLM-Evaluation-prompt.md`, which are the same categories and
the same weights as the rubric in `how-to-submit.md`. If the LLM invents different categories or
weights, say so below rather than silently accepting them.

**Coverage reported:** [line coverage % from `build/reports/jacoco/test/html/index.html`]

LLM RESPONSE

I scored the submission at 86/100. All 35 tests pass on a forced clean rerun and checkstyle reports no findings. The code is strong; the marks were lost in the tests, the AgeMonths.toString() structure, and an introspection that never mentions Species.

# Category Score

1 Immutability & encapsulation 25/25
2 Constructor validation 24/25
3 Correctness 15/15
4 Testing & coverage 11/15
5 Code quality & style 6/10
6 Scope discipline & code walk 5/10

1. Immutability and encapsulation: 25/25
   Fields: all four Animal fields are private final (Animal.java:28-31), and so is AgeMonths.months.
   No mutation: there are no setters and no method changes state.
   Accessors: every one returns an immutable type (String, an enum, AgeMonths, LocalDate), so no caller can change the object through a returned reference.
   Not deducted: public class Animal isn't final, so a subclass could override name() or toString(). That line shipped with the template, so I didn't take marks for it, but the student should be able to say why AgeMonths is final and Animal isn't.
2. Constructor validation: 24/25
   Each required check is present (Animal.java:56-71):

Rule Present?
null name ✅ name == null
empty / whitespace-only name ✅ name.isBlank()
null species ✅
null age ✅
null intake date ✅
stored stripped ✅ this.name = name.strip();
validates before any assignment ✅ all four ifs come before line 68
throws IntakeException, not a generic exception ✅
−1: "Name must be non-null containing at least one non-whitespace character" covers two different failures with one message. A caller who passed " " and one who passed null get the same text, so neither learns which mistake they made. The shipped AgeMonths.of sets a better example: it names the rejected value ("..., was " + months).

3. Correctness: 15/15
   Age strings: all seven required strings are correct (0, 1, 11, 12, 23, 24, 25 months). So are 13, 479 and 480.
   Plurals: singular/plural is right in both the years part and the months part.
   Whole years: these drop the months part completely (case 0 -> "").
   Animal.toString(): gives exactly Luna (Cat, 1 year, 11 months, intake 2026-09-21).
4. Testing and coverage: 11/15
   Boundaries: 0, 11/12, the maximum (480) and one past it (481) are all covered. Every one except the toString of 480 (see below) comes from the shipped tests.
   Student's additions:
   toString at 479 and 480 (theOutputOfMaxCombinedInput, theOutputOfMaxPossibleAgeInput).
   Four tests that check exception messages.
   These are useful, and they correctly assert IntakeException.class, not just Exception.
   −2, a weak assertion that would pass for wrong code: AnimalTest.java:132 uses contains("age"). The substring also appears in "message", "usage" and "page", so a message like "Invalid usage" would pass.
   −1, duplicate tests: aNullSpeciesIsRefused1, aNullAgeIsRefused1 and aNullIntakeDateIsRefused1 repeat existing tests and only add a message check. A 1 suffix tells a reader nothing; names like aNullSpeciesMessageNamesSpecies would. The method-level @Tag ("current") is also redundant, since the class is already tagged.
   −1, gaps. Three specific untested cases:
   Names with other whitespace, such as "\tLuna\n" or "\u2003Luna\u2003" (an em space). This is the test that would catch a switch from strip() to trim(): trim() leaves the em space in place.
   Animal.toString() with a whole-year age or another species, such as Bird at 24 months → "Kiwi (Bird, 2 years, intake 2026-09-21)". Only the "1 year, 11 months" and "1 month" shapes are tested.
   Message contents for a null name and for AgeMonths.of(-1). The null-name message is never checked. The -1 test checks for "negative" but not that the message contains -1, so it doesn't test the "name the offending value" rule the shipped test applies to 481.
5. Code quality and style: 6/10
   Delegation ✅: Animal.toString() delegates through age().toString() and species().label(), and the words "year" and "month" never appear in it. If the age format changes, Animal needs no edit. (The explicit .toString() is redundant with %s, but harmless.)
   Javadoc: every public member has a purpose statement, but they all shipped with the template. The student added no public members, so there was nothing new to document. No deduction.
   −3, AgeMonths.toString() (AgeMonths.java:117-150):
   The inner switches for case 1 (lines 133-138) and default (lines 141-146) are identical apart from what they yield.
   Assigning monthsString as a side effect inside a switch expression that yields yearsString is hard to follow.
   In the years == 0 branch the whole answer goes into monthsString while yearsString is "", so the variable names mislead.
   case 0 -> "0 months" is a special case the default arm already produces.
   The block is indented 6 spaces instead of 4.
   The student noticed the repetition themselves in §5 of the introspection, which counts for something. The fix is to build each part with one plural helper and join them, not a 3×3 grid of switches.
   −1, formatting:
   Animal.java:57 is 106 characters, and AnimalTest.java:131 is 117. The rest of the files wrap at 100.
   There is trailing whitespace on Animal.java:70.
   In AnimalTest, the student moved import java.time.LocalDate above the static imports, reversing the template's order.
6. Scope discipline and code walk: 5/10
   Scope (5/5): no inheritance, collections, equals or hashCode were added. twoIdenticalAnimalsAreDifferentObjects was left alone.
   Code-walk evidence (0/5):
   Missing file: submission/introspection.md doesn't exist. The work is in submission/introspection-template.md.
   Two copies that disagree: the student also edited the root introspection-template.md, and §6 differs between the two ("nothign really." vs. "figuring out which more tests i could add").
   No Species discussion: neither copy mentions Species at all, so nothing shows they understand why it's an enum with a label field and a label() method rather than three bare constants (display text bound to each value, a fixed closed set, type-safe use in a parameter).
   §3 Invariants is thin: it says "The rest of them are mosly just not null checks or type checks". It never states Animal's actual invariant (name is non-null, non-blank, already stripped) or which line enforces it.
   Most important change for next time: put the reasoning in writing, not just the working code. The code shows they can enforce invariants, but the introspection can't say what those invariants are, and it says nothing about Species, which the rubric explicitly checks. Next time, name each invariant, cite the line that enforces it, and explain every shipped type they rely on.

Done genuinely well: the Animal constructor is textbook. It checks every argument before assigning a field, gives each failure a message naming the argument, and pairs isBlank() with strip(). Both of those treat Unicode whitespace the same way. With trim(), a name made only of em spaces would pass isBlank(), then come out unchanged and look blank. That pairing is subtle and it's correct.

---

## Your response

The part that is actually marked. For each point below, a few sentences.

### Where it is right

Which criticisms do you accept? For each, say what you would change and why you agree.

The critisicm on the ageMonths toString method i agree with. This is the change i was thinking of myself in teh design introspective as well, and the LLM seems to agree with my thoughts.
Test Naming i also agree with but i think is made more clear because of the existing tests i did not want to delete, i woudl have just edited those in the real case.

### Where it is wrong

Which criticisms do you reject, and on what grounds? LLMs confidently misread code, invent
requirements that are not in the specification, and flag correct code as broken. Disagreeing with a
specific reason is worth more marks here than agreeing with everything.

I wanted to check the error message on some of the tests and it is saying that it is not needed, not needed maybe but i wanted to check that so i think the LLM is wrong in this case.

### What it missed

What do you know is weak in your submission that the assessment did not mention? Volunteering this
costs you nothing and demonstrates you understand your own code.

this one im not sure, the things i know were weak the llm agrees with. maybe if we had implemented more methods with more functionality i would have had somethign i think the llm missed.

### What you changed

If you changed anything as a result, say what and why. If you changed nothing, say that and defend
it.

nothign i didnt find its feedback to be anythign that needed to be changed.

---

## Declaration

- Which LLM and version you used: Claude Opus 5.5
- Confirm you understand every line you submitted, regardless of who or what wrote it: yes
