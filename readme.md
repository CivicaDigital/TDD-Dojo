# TDD

## Benefits of TDD

## The 3 steps to TDD

## Fizzbuzz example

### The problem

The Fizzbuzz problem is simple. A user supplies a number, and the code will work out a response to that number, depending on a pre set of rules:
* If the number is divisible by 3, return 'Fizz'
* If the number is divisible by 5, return 'Buzz'
* If the number is divisible by both 3 and 5, return 'Fizz Buzz'
* If the number doesn't match any of the above criteria, return the original number.
 
 ### Write your first test

 Now that we have the requirments we can write the first test. The first one we will write will be to return the original number as that's the easiest to implement.

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

Remember, we want to make the code pass in the easiest way possible, which can be done by returning a hardcoded value.

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

Run the test and it should pass. Change the input number and the expexted string and check that the test still passes.

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

The next test we need to create is to test that if the input is divisble by 5, then the service return "Buzz".

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

If we run the tests again, they should all still pass. We have just finished writting our first TDD code. 


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
    string expected = "no one likes this";

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
        if (input.Count() == 0)
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
        if (input.Count() == 0)
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
        if (input.Count() == 0)
        {
            return "no one likes this";
        }
        else if (input.Count() == 1)
        {
            return $"{input[0]} likes this";
        }

        return "";
    }
}
```

#### Make the rest of the tests passe
Now work through each remaining test, one at a time, making them pass and then refactoring the code after.

#### Possible bug
 Once you have all the tests passing and the code is as good as you can make it, see if you can fix this bug.
 
```
What if the input list of names is "Alex", "Alex", "Jacob", "Mark", "Max"?
```
It would give an output of "Alex, Alex and 3 others like this". Which doesn't look right. There may be 2 Alexs that like it, but it would read better as "Alex, Jacob and 3 others like this".

Add in the new test for this bug and use your TDD knowledge to make all the passes test.

