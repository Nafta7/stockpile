# Git commit messages - Best practices

A collection of best practices in commit messages

## A model git commit message

```
Capitalized, short (50 chars or less) summary

More detailed explanatory text, if necessary.  Wrap it to about 72
characters or so.  In some contexts, the first line is treated as the
subject of an email and the rest of the text as the body.  The blank
line separating the summary from the body is critical (unless you omit
the body entirely); tools like rebase can get confused if you run the
two together.

Write your commit message in the imperative: "Fix bug" and not "Fixed bug"
or "Fixes bug."  This convention matches up with commit messages generated
by commands like git merge and git revert.

Further paragraphs come after blank lines.

- Bullet points are okay, too

- Typically a hyphen or asterisk is used for the bullet, followed by a
  single space, with blank lines in between, but conventions vary here

- Use a hanging indent
```

## What should be included in a commit message

- A summary or title
- A detailed description
- A tracking or ticket number, if it's applicable.

Here is a sample git commit with all that information:

```
Fixed bug where user can't signup.

Users were unable to register if they hadn't visited the plans
and pricing page because we expected that tracking
information to exist for the logs we create after a user
signs up.  I fixed this by adding a check to the logger
to ensure that if that information was not available
we weren't trying to write it.

[Fixes #2873942]
```

## Writing good commit messages

### Do

 - Write the summary line and description of what you have done in the imperative mode, that is as if you were commanding someone. Write "fix", "add", "change" instead of "fixed", "added", "changed".
 - Always leave the second line blank.
 - Line break the commit message (to make the commit message readable without having to scroll horizontally in gitk).

### Don't

 - Don't end the summary line with a period - it's a title and titles don't end with a period.

### Tips

If it seems difficult to summarize what your commit does, it may be because it includes several logical changes or bug fixes, and are better split up into several commits using git add -p.


## The seven rules of a great git commit message

 - Separate subject from body with a blank line
 - Limit the subject line to 50 characters
 - Capitalize the subject line
 - Do not end the subject line with a period
 - Use the imperative mood in the subject line
 - Wrap the body at 72 characters
 - Use the body to explain what and why vs. how



## References

`How to Write a Git Commit Message`: http://chris.beams.io/posts/git-commit/

`A note about git commit messages`: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html

`Proper Git Commit Messages and an Elegant Git History`: http://ablogaboutcode.com/2011/03/23/proper-git-commit-messages-and-an-elegant-git-history/

`Commit Often, Perfect Later, Publish Once: Git Best Practices`: https://sethrobertson.github.io/GitBestPractices/

`5 Useful Tips For A Better Commit Message`: https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message

`Writing good commit messages`: https://github.com/erlang/otp/wiki/Writing-good-commit-messages
