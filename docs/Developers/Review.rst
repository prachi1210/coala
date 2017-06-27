Reviewing
=========

This document is a guide to coala's review process.

Am I Good Enough to Do Code Review?
-----------------------------------

**Yes,** if you already fixed a newcomer issue.

Reviewing can help you understand the other side of the table and learn more
about coala and python. When reviewing you will get to know new people, more
code and it ultimately helps you to become a better coder than you could do
on your own.

You can totally help us review source code. Especially try to review source
code of others and share what you have learnt with them. You can use acks and
unacks like everyone else and ``cobot`` even allows you to set PRs to WIP. Check
the section below for more information.

Generally follow this process:

1. Check if the change is helping us. Sometimes people propose changes that are
   only helpful for specific usecases but may break others. The concept has to
   be good. Consider engaging in a discussion on gitter if you are unsure!
2. Check for automatic builds. Give the contributor hints on how he can resolve
   them.
3. Review the actual code. Suggest improvements and simplifications.

**Be sure to not value quantity over quality!** Be transparent and polite:
Explain why something has to be changed and don't just "command" the coder to
obey guidelines for no reason. Reviewing always involves saying someone that
his code isn't right, be very careful not to appear rude even if
you don't mean it! Bad reviews will scare away other contributors.

.. note::

    Commits should have a good size, every logical change should be one commit.
    If there are multiple changes, those make multiple commits. Don't propose
    to squash commits without a reason!

When reviewing, try to be thorough: if you don't find any issues with a pull
request, you likely missed something.

If you don't find any issues with a Pull Request and acknowledge it, a senior
member will look over it and perform the merge if everything is good.

Manual Review Process
---------------------

The review process for coala is as follows:

1. Anyone can submit commits for review. These are submitted via Pull Requests
   on Github.
2. The Pull Request will be labeled with a ``process`` label:

    - ``pending review`` the commit has just been pushed and is awaiting review
    - ``wip`` the Pull Request has been marked as a ``Work in Progress`` by the
      reviewers and has comments on it regarding how the commits shall be
      changed
    - ``approved`` the commits have been reviewed by the developers and they
      are ready to be merged into the master branch

   If you don't have write access to coala, you can change the labels using
   ``cobot mark wip <URL>`` or ``cobot mark pending <URL>``.
3. The developers will acknowledge the commits by writing

    - ``ack commit_SHA`` or ``commit_SHA is ready``, in case the commit is
      ready, or
    - ``unack commit_SHA`` or ``commit_SHA needs work`` in case it is not ready
      yet and needs some more work or
    - ``reack commit_SHA`` in case the commit was acknowledged before, was
      rebased without conflicts and the rebase did not introduce logical
      problems.

    .. note::

        Only one acknowledgment is needed per commit i.e ``ack commit_SHA``.

4. If the commits are not linearly mergeable into master, rebase and go
   to step one.
5. All commits are acknowledged and fit linearly onto master. All
   continuous integration services (as described below) pass. A maintainer
   may leave the ``@rultor merge`` command to get the PR merged automatically.

Automated Review Process
------------------------

It is only allowed to merge a pull request into master if all ``required``
GitHub states are green. This includes the GitMate review as well as the
Continuous Integration systems.

Continuous integration is always done for the last commit on a pull
request but should ideally pass for every commit.

For the Reviewers
-----------------

-  Generated code is not intended to be reviewed. Instead rather try to
   verify that the generation was done right. The commit message should
   expose that.
-  Every commit is reviewed independently from the other commits.
-  Tests should pass for each commit. If you suspect that tests might
   not pass and a commit is not checked by continuous integration, try
   running the tests locally.
-  Check the surroundings. In many cases people forget to remove the
   import when removing the use of something or similar things. It is
   usually good to take a look at the whole file to see if it's still
   consistent.
-  Check the commit message.
-  Take a look at continuous integration results in the end even if they
   pass.
-  Coverage must not fall.
-  Be sure to assure that the tests cover all corner cases and validate the
   behaviour well. E.g. for bear tests just testing for a good and bad file
   is **not** sufficient.

As you perform your review of each commit, please make comments on the
relevant lines of code in the GitHub pull request. After performing your
review, please comment on the pull request directly as follows:

-  If any commit passed review, make a comment that begins with "ack",
   "reack", or "ready" (all case-insensitive) and contains at least the
   first 6 characters of each passing commit hash delimited by spaces,
   commas, or forward slashes (the commit URLs from GitHub satisfy the
   commit hash requirements).

-  If any commit failed to pass review, make a comment that begins with
   "unack" or "needs work" (all case-insensitive) and contains at least
   the first 6 characters of each passing commit hash delimited by
   spaces, commas, or forward slashes (the commit URLs from GitHub
   satisfy the commit hash requirements).

.. note::

    GitMate only separates by spaces and commas. If you copy and paste the SHAs
    they sometimes contain tabs or other whitespace, be sure to remove those!

Example:

.. code-block:: none

   unack 14e3ae1 823e363 342700d

If you have a large number of commits to ack, you can easily generate a
list with ``git log --oneline master..`` and write a message like this
example:

.. code-block:: none

   reack
   a8cde5b  Docs: Clarify that users may have pyvenv
   5a05253  Docs: Change Developer Tutorials -> Resources
   c3acb62  Docs: Create a set of notes for development setup

   Rebased on top of changes that are not affected by documentation
   changes.

Guide to Writing Good Reviews
------------------------

**Before you start writing a review**

-  Familiarize yourself with the context.  Read up on the issue 
   that is relevant to the proposed PR.
-  Examine if the code does what it proposes to solves. 
   For example: If it is a bug, it may be useful to run the code locally 
   and check if the same exception is thrown up.
-  Understand the importance of the code and the consequences of failure. 
   Code reviews must always be done with utmost care.

**While writing a review**

It is important to ask the following questions:
-  Is the code modular and clean? 
   Is there any reduntant code?
-  Would I have done this differently? 
   If yes, which approach is better? 
-  Are comments comprehensible? Do they add something to the maintainability of the code?
-  Have exceptions been used appropriately?
-  Have common errors been checked and accounted for?
-  Does the functionality fit the current design?
-  Is the code unit testable?
-  Does the code and the commit message comply to coding standards? 
-  Does the commit message give complete and useful information about the changes under it?

**Things to look out for**

-  Typographic errors: Check for spelling mistakes, typographic errors. Have a look at the code below:

``def foo(self):
   print("br") #  <---- prnts bar
``
This is a classic example of a typographic error.

-  Naming Conventions: Ensure that the code has variable names consistent with code previously written. 
For example: this_is_example_variable is in line with the naming convention whereas thisIsExampleVariable is not.

-  Docstrings and comments: 
``Docstrings = How to use code``
``Comments = Why (rationale) & how code works``
Docstrings explain how to use code, and are for the users of code. 
Comments explain why, and are for the maintainers of code. 
**Make sure comments are not too numerous or verbose.**

- Styling Issues: While a lot of code styling issues are picked out by gitmate,
keep an eye out for the following:

   1. One statement per line

   ``if(foo): print('bar')`` 

   should be changed to

   ``if(foo):
         print('bar')
   ``

   2. Complex loop and conditional constructs

   ``if((complex cond1) and (complex cond2)):
        foo()
    `` 
    should be changed to

   ``t1 = complex cond 1
     t2 = complex cond 2
     if(t1 and t2):
        foo()
   ``
-  Return Values: In order to keep a clear intent and sustainable readibility,
it is preferable to avoid returning values from many output points in the function body. 
Proper exceptions must be raised when the functio is unable to complete its computation or task.

- Idioms: Pythonic [idioms](http://wiki.c2.com/?ProgrammingIdiom) are simply a way to write python code. 
Writing Idiomatic code is an acquired programming skill. For more on, pythonic idioms [read this](http://python.net/~goodger/projects/pycon/2007/idiomatic/handout.html)

- Code Modularity: An important result of robust code reviews is modular code.
For eg:
```
  if(foo1):
      if(foo2):
        bar2()
  else:
      if(foo3):
        bar3()
      else:
        bar4()
```
can be written in a much simpler way