# Refactoring: Improving the Design of Existing Code
> Second edition

By Martin Fowler.

## Chapter 1. Refactoring: a first example
### Comments on the starting program
* When you have to add a feature to a program but the code is not structured in a convenient way, first refactor the program to make it easy to add the feature, then add the feature.

### The first step in refactoring
* Before you start refactoring, make sure you have a solid suite of tests.
* These tests must be self-checking.
* Testing after each change means that when I make a mistake, I only have a small change to consider in order to spot the error, which makes it far easier to find and fix.
* Commit after each successful refactoring, to easily get back to a working state where you mess up later. Then, squash changes into more significant commits before pushing the changes to a shared repository.
* Any fool can write code that a computer can understand. Good programmers write code that humans can understand.

### Removing the play variable
* Benefit of removing local variables is that it makes it much easier to do extractions, since there is less local scope to deal with.

### Removing total volume credits
* If your refactoring introduces performance slow-downs, finish refactoring first and do performance tuning afterwards.

### Status: separated into two files (and phases)
* When programming, follow the camping rule: Always leave the code base healthier than when you found it.

### Final thoughts
* The true test of good code is how easy it is to change it.
* Code should be obvious.
    * When someone needs to make a change, they should be able to find the code to be changed easily and to make the change quickly without introducing any errors.
* The key to effective refactoring is recognizing that you go faster when you take tiny steps, the code is never broken, and you can compose those small steps into substantial changes.

## Chapter 2. Principles in refactoring
TODO

## Chapter 3. Bad smells in code
TODO

## Chapter 4. Building tests
TODO