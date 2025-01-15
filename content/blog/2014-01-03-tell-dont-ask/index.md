+++
title = "Tell don't Ask"
+++

_Article originally posted on [Tribalingua](http://tribalingua.wordpress.com/2013/12/17/tell-dont-ask/)._

Do you take care with how you write a letter? So that it will clear and easy for your recipient  to read? Well, it should be the same feeling with the code that you write. With a letter we sometimes have to stop and think what is the best way to write down what is in our minds. Therefore, we have to be clear and easy for anyone else reading what you were thinking when you wrote that.  Just as we have rules to write a nice readable letter, also we have Patterns and Principles to write clean and understandable code.

The principle **Tell don't Ask** helps to ensure a correct division of responsibility  without causing excess coupling to an incorrect class.

> ...you should endeavor to *tell* objects what you want them to do; do not *ask* them questions about their state, make a decision, and then tell them what to do. - [The Pragmatic Bookshelf](http://pragprog.com/articles/tell-dont-ask)

[![TellDontAsk](./telldontask1.png?w=490)](./telldontask1.png)

To exemplify it I wrote a sample based in a common code present in a project that I'm working on in this sprint.

```
// Ask_Version.cs
private class Screen
{
  Form mainForm = ...
  private void Screen_Loaded(object sender, ControlLoadedEventArgs e)
  {
    if(mainForm.InputControls != null && mainForm.InputControls.Contains("Id"))
      mainForm.InputControls["Id"].Editable = isEditable;
  }
}

// Tell_Version.cs
private class Screen
{
  Form mainForm = ...
  private void Screen_Loaded(object sender, ControlLoadedEventArgs e)
  {
    mainForm.SetControlEditable("Id");
  }
}
```

In the Ask version I have too much logic outside the Form class, that should be responsible to do all that process. In the Tell version I just tell what I want and nothing else. This increases the maintainability and readability of my code because it is shorter, cleaner and I let the Form class do what it knows doing better than the Screen class.

As many others principles and patterns in software design, this is not set in stone nor is it  a silver bullet to solve all your problems, but anyway with approaches like that you will be guided towards a better code.

So do you want to be known as a good writer or as that crazy drunk friend that sends you nonsense messages on the phone at 3:00 AM?! Think about it :)

