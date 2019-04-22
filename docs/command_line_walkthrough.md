# Command Line Walkthrough
This tutorial will show you how to perform the FizzBuzz code kata using BASH and BATS
on the command line.  This example uses MacOS in a terminal window.

## 1. Create project directory
```bash
$ mkdir -p fizzbuzz_bats_00N/lib # where 00N is a number to separate attempts
$ cd fizzbuzz_bats_00N
```

## 2. Create first test
Create fizzbuzz.bats and update it to have first test
```bash
#!/usr/bin/env bats

source lib/fizzbuzz.sh

@test "1 = 1" {
    run do_fizzbuzz 1
    [[ "${output}" = "1" ]]
}
```

## 3. Run bats - fail because lib/fizzbuzz.sh not created
```bash
$ bats --tap .
```

## 4. Create fizzbuzz library
* Create lib/fizzbuzz.sh that contains:
```bash
#!/usr/bin/env bash

function do_fizzbuzz() {
    echo ""
}
```

## 5. Run bats - fail because the number "1" step not implemented
```bash
$ bats --tap .
```

### 6. Implement the number "1" test
We want a simple test that returns the number given to it.

* Add the test for the first scenario in fizzbuzz.bats to look like:
```bash
#!/usr/bin/env bash

function do_fizzbuzz() {
    number=$1
    echo ${number}
}
```

## 7. Run bats - pass
```bash
$ bats --tap .
```

Great job!  Our first test has passed.  Now onto to the second scenario.

## 8. Add the test for the second scenario for "2"
* Add the test for the second scenario in fizzbuzz.bats to look like:
```bash
@test "2 = 2" {
    run do_fizzbuzz 2
    [[ "${output}" = "2" ]]
}
```

## 9. Run bats - pass
```bash
$ bats --tap .
```

Why did this pass without code changes?  Because the fizzbuzz function is just returning the number given
to it. 

## 10. Add the test for the third scenario for "3"
* Add the test for the third scenario in fizzbuzz.bats to look like:
```bash
@test "3 = Fizz" {
    run do_fizzbuzz 3
    [[ "${output}" = "Fizz" ]]
}
```

## 11. Run bats - fail because the multiple of 3 logic not implemented in the do_fizzbuzz function
```bash
$ bats --tap .
```

You'll notice that bats reports a failure on this test.  Let's add code to deal with this.

## 12. Implement the multiple of 3 logic in do_fizzbuzz 
* Update lib/fizzbuzz.sh to contain:
```bash
#!/usr/bin/env bash
  
function do_fizzbuzz() {
    number=$1

    if [[ $(( $number % 3 )) == 0 ]]; then
        echo "Fizz"
    else
        echo ${number}
    fi
}
```

This simple test to use the modulo operator ("%") returns a 0 if there's no remainder.  So by testing
if dividing the number by 3 has a 0 remainder, then we echo the string "Fizz".

## 13. Run bats - pass
```bash
$ bats --tap .
```

## 14. Add the test for the fourth scenario for "5"
* Add the test for the fourth scenario in fizzbuzz.bats to look like:
```bash
@test "5 = Buzz" {
    run do_fizzbuzz 5
    [[ "${output}" = "Buzz" ]]
}
```

## 15. Run bats - fail because the multiple of 5 logic not implemented in the do_fizzbuzz function
```bash
$ bats --tap .
```

You'll notice that bats reports a failure on this test.  Let's add code to deal with this.

## 16. Implement the multiple of 3 logic in do_fizzbuzz 
* Update lib/fizzbuzz.sh to contain:
```bash
#!/usr/bin/env bash
  
function do_fizzbuzz() {
    number=$1

    if [[ $(( $number % 5 )) == 0 ]]; then
        echo "Buzz"
    elif [[ $(( $number % 3 )) == 0 ]]; then
        echo "Fizz"
    else
        echo ${number}
    fi
}
```

This simple test to use the modulo operator ("%") returns a 0 if there's no remainder.  So by testing
if dividing the number by 5 has a 0 remainder, then we return the string "Buzz".

## 17. Run bats - pass
```bash
$ bats --tap .
```

## 18. Add the test for the fifth scenario for "15"
* Add the test for the fifth scenario in fizzbuzz.bats to look like:
```bash
@test "15 = FizzBuzz" {
    run do_fizzbuzz 15
    [[ "${output}" = "FizzBuzz" ]]
}
```

## 19. Run bats - fail because the multiple of 3 and 5 logic not implemented in the do_fizzbuzz function
```bash
$ bats --tap .
```

You'll notice that bats reports a failure on this test.  Let's add code to deal with this.

## 20. Implement the multiple of 15 logic in do_fizzbuzz 
* Update lib/fizzbuzz.sh to contain:
```bash
#!/usr/bin/env bash
  
function do_fizzbuzz() {
    number=$1

    if [[ $(( $number % 3 )) == 0 && $(( $number % 5 )) == 0 ]]; then
        echo "FizzBuzz"
    elif [[ $(( $number % 5 )) == 0 ]]; then
        echo "Buzz"
    elif [[ $(( $number % 3 )) == 0 ]]; then
        echo "Fizz"
    else
        echo ${number}
    fi
}
```

This simple test to use the modulo operator ("%") returns a 0 if there's no remainder.  So by testing
if dividing the number by 3 has a 0 remainder and dividing the number by 5 has a 0 remainder, then we
echo the string "FizzBuzz".

## 21. Run bats - pass
```bash
$ bats --tap .
```

## 22. Refactor to simplify logic
* Update lib/fizzbuzz.sh to contain:
```bash
#!/usr/bin/env bash
  
function do_fizzbuzz() {
    number=$1

    results=""
    if [[ $(( $number % 3 )) == 0 ]]; then
        results="${results}Fizz"
    fi
    if [[ $(( $number % 5 )) == 0 ]]; then
        results="${results}Buzz"
    fi
    [[ -n "${results}" ]] && echo ${results} || echo ${number}
}
```

BASH has a nice "-n VAR" test operator behavior with strings, where the expressions will give "results"
if it is not empty (an empty string returns False, a non-empty string returns True), otherwise, return
"number".

## 23. Run bats - pass
```bash
$ bats --tap .
```

## 24. Write the program to FizzBuzz the numbers 1 to 100
Now that we have the core logic in our FizzBuzz library, let's write a simple wrapper script that uses
this library to iterate through the numbers 1 to 100, running it through the do_fizzbuzz function.

* Create fizzy.sh to contain:
```bash
#!/usr/bin/env bash

source lib/fizzbuzz.sh

for i in $(seq 1 100); do
    echo $(do_fizzbuzz ${i})
done
```

Make the script executable:
```bash
$ chmod +x ./fizzy.sh
```

## 25. Run the script and see how it meets the FizzBuzz kata requirements!
```bash
$ ./fizzy.sh
```

## 26. Stage 2 - new requirements and the sixth scenario - number contains a "3"
Now let's add new requirements to the tests for fun.

* Add the following to the end of fizzbuzz.bats
```bash
# Stage 2 - new requirements

# A number is fizz if it is divisible by 3 or if it has a 3 in it
@test "3 in number = Fizz" {
    run do_fizzbuzz 13
    [[ "${output}" = "Fizz" ]]
}
```
We're using 13, because it's a prime number and it has a 3 in it.

## 27. Run bats - fail because do_fizzbuzz doesn't have contains a "3" implemented
```bash
$ bats --tap .
```

## 28. Implement the number contains a "3" logic in do_fizzbuzz 
At first, we can just use compound logic to test for both the remainder and if the number contains
the number "3".  BASH has a handy *"string"* match string feature, so we'll use that:

* Update lib/fizzbuzz.sh to contain:
```bash
#!/usr/bin/env bash
  
function do_fizzbuzz() {
    number=$1

    results=""
    if [[ $(( $number % 3 )) == 0 || ${number} == *"3"* ]]; then
        results="${results}Fizz"
    fi
    if [[ $(( $number % 5 )) == 0 ]]; then
        results="${results}Buzz"
    fi
    [[ -n "${results}" ]] && echo ${results} || echo ${number}
}
```

## 29. Run bats - pass
```bash
$ bats --tap .
```

## 30. Stage 2 - seventh scenario - number contains a "5"
Let's add a seventh require about number contains a "5"

* Add the following to the end of the Feature description in fizzbuzz.bats
```text
# A number is buzz if it is divisible by 5 or if it has a 5 in it
@test "5 in number = Buzz" {
    run do_fizzbuzz 59
    [[ "${output}" = "Buzz" ]]
}
```

We're using 59, because it's a prime number and it has a 5 in it.

## 31. Run bats - fail because do_fizzbuzz doesn't have contains a "5" implemented
```bash
$ bats --tap .
```

## 32. Implement the number contains a "5" logic in do_fizzbuzz 
At first, we can just use compound logic to test for both the remainder and if the number contains
the number "5".  BASH has a handy *"string"* match string feature, so we'll use that:

* Update lib/fizzbuzz.sh to contain:
```bash
#!/usr/bin/env bash
  
function do_fizzbuzz() {
    number=$1

    results=""
    if [[ $(( $number % 3 )) == 0 || ${number} == *"3"* ]]; then
        results="${results}Fizz"
    fi
    if [[ $(( $number % 5 )) == 0 || ${number} == *"5"* ]]; then
        results="${results}Buzz"
    fi
    [[ -n "${results}" ]] && echo ${results} || echo ${number}
}
```

## 33. Run bats - pass
```bash
$ bats --tap .
```

## 34. Run fizzy.sh and see how the output has changed for the Stage 2 requirements
```bash
$ ./fizzy.sh
```

----

Now that you've gone through this walkthrough, try it on your own from scratch!
  
See how far you can get without referencing the walkthrough!
  
Enjoy!
