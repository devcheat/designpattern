**Guard Clause**
===

A guard clause is simply a check that immediately exits the function, either with a return statement or an exception. If you're used to writing functions that check to ensure everything is valid for the function to run, then you write the main function code, and then you write else statements to deal with error cases, this involves inverting your current workflow. The benefit is that your code will tend to be shorter and simpler, and less deeply indented.

An example of a function that does not use guard clauses:

```cs
public void Subscribe(User user, Subscription subscription, Term term)
{
    if (user != null)
    {
        if (subscription != null)
        {
            if (term == Term.Annually)
            {
                // subscribe annually
            }
            else if (term == Term.Monthly)
            {
                // subscribe monthly
            }
            else
            {
                throw new InvalidEnumArgumentException(nameof(term));
            }
        }
        else
        {
            throw new ArgumentNullException(nameof(subscription));
        }
    }
    else
    {
        throw new ArgumentNullException(nameof(user));
    }
}
```

This code can be refactored to eliminate the need for the else clauses. This is accomplished by simply inverting the logic of the if statements and putting the exception throwing statements inside of these if statements. The result looks like this:

```cs
public void Subscribe2(User user, Subscription subscription, Term term)
{
    if (user == null)
    {
        throw new ArgumentNullException(nameof(user));
    }
    if (subscription == null)
    {
        throw new ArgumentNullException(nameof(subscription));
    }
    if (term == Term.Annually)
    {
        // subscribe annually
    }
    else if (term == Term.Monthly)
    {
        // subscribe monthly
    }
    else
    {
        throw new InvalidEnumArgumentException(nameof(term));
    }
}
```

The checks for null and common behavior of throwing a particular type of exception is clearly a violation of the [DRY principle](https://deviq.com/principles/dont-repeat-yourself). This code can be pulled out into a helper method:

```cs
public static class Guard
{
    public static void AgainstNull(object argument, string argumentName)
    {
        if (argument == null)
        {
            throw new ArgumentNullException(argumentName);
        }
    }
    public static void AgainstInvalidTerms(Term term, string argumentName)
    {
        // note: currently there are only two enum options
        if (term != Term.Annually &&
            term != Term.Monthly)
        {
            throw new InvalidEnumArgumentException(argumentName);
        }
    }
}
```

These helper guard methods can then be called without the need to even include any if statements in the calling function, since if an exception occurs it will bubble up and out of the original function. Now the original function can be updated to look like this:

```cs
public void Subscribe3(User user, Subscription subscription, Term term)
{
    Guard.AgainstNull(user, nameof(user));
    Guard.AgainstNull(subscription, nameof(subscription));
    Guard.AgainstInvalidTerms(term, nameof(term));
    if (term == Term.Annually)
    {
        // subscribe annually
        return;
    }
    // subscribe monthly
}
```

Over time you can continue to add additional Guard helper methods for any other common cases you need to check for, such as empty strings, negative numbers, invalid enum values, etc.
