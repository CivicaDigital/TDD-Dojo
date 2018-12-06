# TDD

## What is TDD?
Test driven development (TDD) is a process used in software development where developers write tests before the write code. Firstly they create a set of tests that define the requirements of a feature. Once they have the tests ready (and failing) they then write the minimal amount of code required to get a test to pass. Once the tests are passing, they can refactor the code to improve it and bring it up to acceptable standards.

At first, the concept of writing the minimal amount of code possible (and often hard code values) to make the tests work will be a strange feeling, and goes against all of your instincts as a good developer. However, it structures the code you do create, and prevents the "bloat" of unnecessary code. 

## The 3 steps to TDD

* Fail:  Write the test first so that the tests fail.
* Pass: Write the minimal amount of code possible to get the test to pass.
* Refactor: Re-write the code to meet the standards of code required ensuring that the test still passes.

## Advantages of TDD

* Reduces the chances of bugs: As you create the code from the tests, you are more likely to discover bugs as you code, because a bug that you find will make a previous test fail. You can then quickly catch the bug and fix it.

* More structured code: Writing little bits of code for each test and then refactoring the code afterwards, creates a stronger structure of code. You aren't writing code that you don't need and then forgetting about it until later, at which point you don't know if it's required for something.

* Allows you to confirm the requirements: Writing a set of tests for each requirement allows you to confirm with the product owner that you agree about what you're expected to deliver. 

* Free documentation: Your tests can act as documentation. A new developer coming onto the project will be able to clearly see how the code was developed and what each bit does.

## Fizzbuzz example

### The problem

The Fizzbuzz problem is simple. A user supplies a number, and the code will work out a response to that number, depending on a pre set of rules:
* If the number is divisible by 3, return 'Fizz'
* If the number is divisible by 5, return 'Buzz'
* If the number is divisible by both 3 and 5, return 'Fizz Buzz'
* If the number doesn't match any of the above criteria, return the original number.
 
 ### Write your first test

 Now that we have the requirements we can write the first test. The first one we will write will be to return the original number as that's the easiest to implement.

 Remember, we don't write any implementation code yet, only the test code. It must fail to start off with!!

 ```cs
 [Test]
public void ShouldReturnOriginalNumber()
{
    int input = 1;
    string expected = "1";

    FizzBuzzService fizzBuzz = new FizzBuzzService();

    string result = fizzBuzz.Print(input);
    
    Assert.AreEqual(expected, result);
}
 ```

As expected, this test code won't compile as we haven't got a FizzBuzzService class anywhere. So that's our next step.

### Write the first piece of code to get the test code to compile

All we need to get the test code to compile is a class called FizzBuzzService that has a Print method that takes an int parameter.

```cs
public class FizzBuzzService
{
    public string Print(int input)
    {
        return "";
    }
}
```

Now when you try to run the test, it will compile and then run but most importantly, the test will fail.

### Write the first piece of code to make the test pass in the easiest way possible

Remember, we want to make the code pass in the easiest way possible, which can be done by returning a hard coded value.

```cs
public class FizzBuzzService
{
    public string Print(int input)
    {
        return "1";
    }
}
```

Run the test and it should pass. But what happens if we change the test to use 2 as the input?

### Refactor the code

The service should return the input number as a string, so refactor the code to make it work.

```cs
public class FizzBuzzService
{
    public string Print(int input)
    {
        return input.ToString();
    }
}
```

Run the test and it should pass. Change the input number and the expected string and check that the test still passes.

### Next tests

The next test we want to implement is to check if the input is divisible by 3 and if it is, then return "Fizz".

 ```cs
[Test]
public void ShouldReturnFizz()
{
    int input = 3;
    string expected = "Fizz";

    FizzBuzzService fizzBuzz = new FizzBuzzService();

    string result = fizzBuzz.Print(input);
    
    Assert.AreEqual(expected, result);
}
 ```

Run the test and it should fail. Now change the service code to make the test pass in the easiest way possible.

```cs
public class FizzBuzzService
{
    public string Print(int input)
    {
        if (input % 3 == 0)
        {
            return "Fizz";
        }

        return input.ToString();
    }
}
```

Run the tests again and they should all pass. There isn't a way to refactor this, so we can skip this step for this test.

The next test we need to create is to test that if the input is divisible by 5, then the service return "Buzz".

 ```cs
[Test]
public void ShouldReturnBuzz()
{
    int input = 5;
    string expected = "Buzz";

    FizzBuzzService fizzBuzz = new FizzBuzzService();

    string result = fizzBuzz.Print(input);
    
    Assert.AreEqual(expected, result);
}
 ```

 Run the test and it will fail. Now change the service code to make it work.

```cs
public class FizzBuzzService
{
    public string Print(int input)
    {
        if (input % 3 == 0)
        {
            return "Fizz";
        }
        else if (input % 5 == 0)
        {
            return "Buzz";
        }

        return input.ToString();
    }
}
```

Run the test again and it will pass. 

### Final test

The final test to write is to return FizzBuzz if the number is divisible by 3 and 5.


 ```cs
[Test]
public void ShouldReturnFizzBuzz()
{
    int input = 15;
    string expected = "FizzBuzz";

    FizzBuzzService fizzBuzz = new FizzBuzzService();

    string result = fizzBuzz.Print(input);
    
    Assert.AreEqual(expected, result);
}
 ```
 
 Run the test and it will fail, so now write the code to make it work. 

```cs
public class FizzBuzzService
{
    public string Print(int input)
    {
        if (input % 3 == 0)
        {
            return "Fizz";
        }
        else if (input % 5 == 0)
        {
            return "Buzz";
        }
        else if (input % 3 == 0 && input %% 5 == 0)
        {
            return "FizzBuzz";
        }

        return input.ToString();
    }
}
```

Run the test and it works!!

### Refactor
Now that we have all the tests running, we can go back to the service code and refactor it as at the moment it's not very good.

What we can do is extract the division checks to their own methods to reduce the duplicate code.

```cs
public class FizzBuzzService
{
    public string Print(int input)
    {
        if (isDivisibleByThree(input))
        {
            return "Fizz";
        }
        else if (isDivisibleByFive(input))
        {
            return "Buzz";
        }
        else if (isDivisibleByThree(input) && isDivisibleByFive(input))
        {
            return "FizzBuzz";
        }

        return input.ToString();
    }

    private bool isDivisibleByThree(int n)
    {
        return n % 3 == 0;
    }

    private bool isDivisibleByFive(int n)
    {
        return n % 5 == 0;
    }
}
```

If we run the tests again, they should all still pass. We have just finished writing our first TDD code. 


## Code Katas are the best way to practice TDD

To get better at TDD, code Katas are perfect. They are small problems that contain easily definable rules that can easily be transcribed into tests. 

https://www.codewars.com/ is a fantastic site for Code Katas. 

### Who likes it
This is an example Kata from Codewars.

```
You probably know the "like" system from Facebook and other pages. People can "like" blog posts, pictures or other items. We want to create the text that should be displayed next to such an item.

Implement a function likes :: [String] -> String, which must take in input array, containing the names of people who like an item. It must return the display text as shown in the examples:

Kata.Likes(new string[0]) => "no one likes this"
Kata.Likes(new string[] {"Peter"}) => "Peter likes this"
Kata.Likes(new string[] {"Jacob", "Alex"}) => "Jacob and Alex like this"
Kata.Likes(new string[] {"Max", "John", "Mark"}) => "Max, John and Mark like this"
Kata.Likes(new string[] {"Alex", "Jacob", "Mark", "Max"}) => "Alex, Jacob and 2 others like this"
```

#### Write the basic tests

 ```cs
[Test]
public void NoOneLikes()
{
    string[] input = new string[0];
    string expected = "No one likes this";

    LikeItService likeIt = new LikeItService();

    string result = likeIt.Print(input);
    
    Assert.AreEqual(expected, result);
}

[Test]
public void OneLike()
{
    string[] input = new string[] {"Peter"};
    string expected = "Peter likes this";

    LikeItService likeIt = new LikeItService();

    string result = likeIt.Print(input);
    
    Assert.AreEqual(expected, result);
}

[Test]
public void TwoLikes()
{
    string[] input = new string[] {"Jacob", "Alex"};
    string expected = "Jacob and Alex like this";

    LikeItService likeIt = new LikeItService();

    string result = likeIt.Print(input);
    
    Assert.AreEqual(expected, result);
}

[Test]
public void ThreeLikes()
{
    string[] input = new string[] {"Max", "John", "Mark"};
    string expected = "Max, John and Mark like this";

    LikeItService likeIt = new LikeItService();

    string result = likeIt.Print(input);
    
    Assert.AreEqual(expected, result);
}

[Test]
public void MoreThanThreeLikes()
{
    string[] input = new string[] {"Alex", "Jacob", "Mark", "Max"};
    string expected = "Alex, Jacob and 2 others like this";

    LikeItService likeIt = new LikeItService();

    string result = likeIt.Print(input);
    
    Assert.AreEqual(expected, result);
}
 ```

 And here's the basic service so that the test compiles.
```cs
public class LikeItService
{
    public string Print(string[] input)
    {
        return "";
    }
}
```

If we run these tests they will all fail, which is what we want.

#### Make one test pass

Lets make the first test pass in the easiest implementation possible.

```cs
public class LikeItService
{
    public string Print(string[] input)
    {
        return "no one likes this";
    }
}
```

#### Refactor first test to make it better

Now refactor the code to make it better. 

```cs
public class LikeItService
{
    public string Print(string[] input)
    {
        if (input.Length == 0)
        {
            return "no one likes this";
        }

        return "";
    }
}
```
Run the tests again and the first one should pass, but the rest should still fail.

#### Make the second test work

Lets make the second test work with the easiest implementation possible.

```cs
public class LikeItService
{
    public string Print(string[] input)
    {
        if (input.Length == 0)
        {
            return "no one likes this";
        }

        return "Peter likes this";
    }
}
```

Run the test and it will pass.

#### Refactor the code for the second test

Now refactor the code to make it better.

```cs
public class LikeItService
{
    public string Print(string[] input)
    {
        if (input.Length == 0)
        {
            return "no one likes this";
        }
        else if (input.Length == 1)
        {
            return $"{input[0]} likes this";
        }

        return "";
    }
}
```

#### Make the rest of the tests pass
Now work through each remaining test, one at a time, making them pass and then refactoring the code after.

#### Possible bug
Once you have all the tests passing and the code is as good as you can make it, see if you can fix this bug.
 
```
What if the input list of names is "Alex", "Alex", "Jacob", "Mark", "Max"?
```
It would give an output of "Alex, Alex and 3 others like this". Which doesn't look right. There may be 2 Alex's that like it, but it would read better as "Alex, Jacob and 3 others like this".

Add in the new test for this bug and use your TDD knowledge to make all the passes test.

#### Another possible bug

What if all the names are the same? 

## The self checkout

Using TDD, create a replica system of a shops self service checkout. You don't need to create a UI, just the components and services that are used in such a system.

* Each item has an estimated weight, so when items are scanned and placed in the packing area, make sure the total weight in the packing area is correct. Allow a 5% discrepancy. (Don't forget the lovely 'Unexpected item in packing area' message that we all hate.)
* Calculate the total of a list of items that the user scanned in
* Calculate the amount of change that should be returned to the customer and the denomination of each coin / note that should be given to the customer
* Display a receipt to show the customer what they purchased, how much each item was, the total the items costs and their payment
* Calculate the total if the user uses a 15% staff discount but only on non alcoholic items

From the list of requirements above, create some tests for each requirement, make the tests fail, then make the tests pass with the easiest route possible and then finally refactor your code to make it better.

