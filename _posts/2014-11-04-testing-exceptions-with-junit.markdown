---
layout: post
title:  "Testing exceptions with JUnit"
date:   2014-11-04 23:00:00
categories: java junit testing unittesting test
comments: yes
description: "There are many ways to unit test code for exceptions, both with and without the exception message. In this post I'll cover 3 ways to use JUnit to test for exceptions what we expect to be thrown. First we look at only checking the type of extension, then we use try and catch to check for message in exception and in the last example we see JUnits ExpectedException Rule in action."
---

When writing your unit tests you often need to test for expected exception. Using JUnit you can test for expected exception in many ways. In this post I'll cover three ways. In the following text we assume that we have basic class called **PhoneNumber**

{% highlight java %}
package net.joningi.example;

public class PhoneNumber {

    private String number;
    
    public PhoneNumber(final String number) {
        this.setNumber(number);
    }

    /**
     * A phone number in this example consists either out of 7 numbers in a row or out of 3 number,
     * a (white)space or a dash and then 4 numbers.
     * @param number The phone number
     */
    public void setNumber(final String number) {
        if(number.matches("\\d\\d\\d([,\\s])?\\d\\d\\d\\d")) {
            this.number = number;
        } else {
            throw new IllegalArgumentException("Illegal phone number");
        }
    }
}
{% endhighlight %}

The main thing to note is that phone number in this class consists either out of 7 numbers in a row or 3 numbers, a whitespace or a dash and then 4 numbers. If the phone number is not in the correct format then an exception in thrown.

Now the most basic way to write unit test that checks if an exception is thrown when we have illegal phone number is the following

{% highlight java %}
package net.joningi.example;

import org.junit.Test;

public class PhoneNumberTest {

    @Test(expected = IllegalArgumentException.class)
    public void testInvalidPhoneNumber() {
        new PhoneNumber("123");
    } 
}
{% endhighlight %}

Here we say that we expect to get IllegalArgumentException if the phone number is not in the correct format. For many cases this can be just what we want, but what if we also want to test the message in the exception. This could be the case if the message is created on the fly or if we have more then one exception of the same type thrown from a single code block, this could for example be SQLException from java code used to do sql queries.
The "old way" to do this is something like the following

{% highlight java %}
package net.joningi.example;

import static junit.framework.TestCase.fail;
import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.core.Is.is;

import org.junit.Test;

public class PhoneNumberTest {

    @Test
    public void testInvalidPhoneNumberException() {
        try {
            new PhoneNumber("123");
            fail("Should have thrown an IllegalArgumentException because number is illegal");
        } catch (IllegalArgumentException e) {
            assertThat(e.getMessage(), is("Illegal phone number"));
        }
    }
}
{% endhighlight %}

Here we just use try and catch with fail from JUnit. If our code does not throw exception the test fails because of the **fail(..)**. If an exception is thrown we catch it and validate the message from the exception.

The "new way" to test for message from expected exception using JUnit is the following

{% highlight java %}
package net.joningi.example;

import org.junit.Rule;
import org.junit.Test;
import org.junit.rules.ExpectedException;

public class PhoneNumberTest {

    @Rule
    public ExpectedException expectedException = ExpectedException.none();

    @Test
    public void testInvalidPhoneNumberExceptionRuleBasedTest() {
        expectedException.expect(IllegalArgumentException.class);
        expectedException.expectMessage("Illegal phone number");
        new PhoneNumber("123");
    }
}
{% endhighlight %}

Here we use JUnit ExpectedException rule to set what type of exception we are expecting and what the message from the exception should be. This makes for a much cleaner and readable code.












