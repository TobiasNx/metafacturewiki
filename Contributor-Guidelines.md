# Commits
This is about how to make good commits resp. a good Pull Request.

## Separate

Do not mix multiple changes in to a single commit: split each unrelated change, however small, in to an individual commit. This makes each specific change easier to review.

## Self-contain

When submitting a pull request, do not have commits that "fix" a mistake in a prior commit. Squash the fix commit in to the commit that it fixes.

## Write good git commit messages

We follow the well-established conventions for writing git messages. In essence, your message should follow eight rules:

1. Separate subject from body with a blank line
1. Limit the subject line to 50 characters
1. Capitalize the subject line
1. Do not end the subject line with a period
1. Use the imperative mood in the subject line. Do not use past tense
1. Wrap the body at 72 characters
1. Use the body to explain _what_ and _why_ not _how_
1. Add a reference to the issue if your commit solves a ticket

A detailed explanation of these rules and the reasoning behind them can be found in [this blog post](https://chris.beams.io/posts/git-commit/) by Chris Beam. The following example from the blog post shows how a commit message should look:

```
Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequences of this
change? Here's the place to explain them.

Further paragraphs come after blank lines.

 - Bullet points are okay, too

 - Typically a hyphen or asterisk is used for the bullet, preceded
   by a single space, with blank lines in between, but conventions
   vary here

If you use an issue tracker, put references to them at the bottom,
like this:

Resolves: #123
See also: #456, #789
```

# Code formatting

_WORK IN PROGRESS_

1. Indentation: Use for spaces for indentation