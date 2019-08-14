---
title: ":sunrise: GSoC 2019 with Zulip"
layout: post
date: 2019-08-12 22:10
image: https://cdn-images-1.medium.com/max/800/1*g5RBYeGe0VLB6t_ZsvO_wQ.png
headerImage: true
tag: 
- Software Engineering
- Google
- python
- Django
- javascript
- Team development
- Remote work
- git
projects: true
hidden: true
category: project
author: whoodes
description: A summary of my accomplishments during my summer with Zulip!
externalLink: false
---

*This post contains a summary of my accomplishments during my time as a Google Summer of Code 2019 participant with the Zulip Open Source Project.*

I would like to thank my official and un-official mentors who helped me to become a better software developer.
With their help, I've had an incredibly fun, productive, and enriching summer!

**Tim Abbott, Aditya Bansal, and Anders Kaseorg:** Thank you for your patience and insight, and for imparting excellent advice and expertise when reviewing my code.

My work for the summer, based off my <a href="https://summerofcode.withgoogle.com/projects/#5885104910499840" target="_blank">proposal</a>, improved upon Zulip's already awesome development productivity tooling.
Alongside this project, I also completed a project that implemented a full web application feature from scratch.
This feature brought a "public only" data export capability to the application with a simple click of a button.  An
event that previously took a few back and forth emails, and effort from a Zulip administrator to manually conduct
the export.

## Work completed

### Development productivity tooling
**This project consisted of many sub-projects to improve the quality of life for developers.**

* <a href="https://github.com/zulip/zulip/pull/12458" target="_blank">Hashing optimization</a>: Here I added
a few hundred millisecond optimization to our `test-backed` start-up time.  This was accomplished by hashing and
comparing our migration files.  Thus we would only do the costly work of parsing the files if our hashes differed.
This project required studying sha hash protocols in order to implement the correct approach using our built-in
hash functions.

* <a href="https://github.com/zulip/zulip/pull/12486" target="_blank">Fix test database memory leak</a>:  After a
fix to running multiple instances of `test-backend` in parallel, we faced a memory leak issue regarding an
accumulation of our template databases.  This project required a bit of exploring the code to figure out that
uniquely naming our databases meant they were not being "recycled", so we had to manually throw out the garbage.

* <a href="https://github.com/zulip/zulip/pull/12533" target="_blank">Design database leak management algorithm</a>:
After fixing the memory leak issue, we needed to design an algorithm to deal with potential leaks that still arose
from certain race conditions, such as an ill-timed `SIGINT`.  In this project I learned about correctly implementing
the design of an algorithm, how to optimally shell-out postgres commands from python, and when set arithmetic can
simplify complicated list comprehension logic.  With the help of my mentor, Aditya Bansal, we also discovered the
cause of an interesting bug that double appended the database id when attempting to destroy the database when running
in serial mode.

* <a href="https://github.com/zulip/zulip/pull/12425" target="_blank">Fix Ubuntu Bionic log file generation</a>:
After upgrading Bionic, Vagrant was generating a log file that was spamming the project's root directory.  This
took a bit of investigation in upstream's issue tracker.  This was a small, quick project that was an excellent
research exercise.

* <a href="https://github.com/zulip/zulip/pull/12272">Fix spurious high-parallel test failure</a>: After we made the change
to base our `test-backend` parallel process count off of `os.cpu_count`, we had a sprint of fixing new and
interesting errors that were arising from the high parallelism.  This fix was simply to reset our default
language.  However, I gained a great appreciation for patience when tracing hard-to-reproduce errors
of this from.

* <a href="https://github.com/zulip/zulip/pull/12602" target="_blank">Improve settings.py debuggability</a>:
The change I made here helps to improve the experience when setting up a Zulip production system, as syntax
errors in the `settings.py` file means django doesn't start up.  This in turn means that no error output
is written to the log file.  To fix that, we simply catch and log any errors when starting up django.

#### Clean up var/ filesystem access
This project consisted of two-parts: A general initial clean up and re-structure, followed by the long-term
implementation for the filesystem.

* <a href="https://github.com/zulip/zulip/pull/12540" target="_blank">Initial var/ clean up</a>: Here I tackled
the first stages of cleaning up the filesystem, which consisted of getting our tests to all write to a specified
`UUID` directory, along with having our provision and test suite processes remove all of the transferred files.

* <a href="https://github.com/zulip/zulip/pull/12698" target="_blank">Follow up to restructure work</a>:
This pull request consisted of the new implementation for how file generation in our backend tests is
handled. I created a unique test run directory for each run of the suite, with an algorithm to handle
removing directories that were created outside of an allotted time delta.  To handle parallelism, every
worker that is spawned during a run of the test suite writes to its own directory within the unique parent.

A couple bug fixes as additional follow ups:
* <a href="https://github.com/zulip/zulip/pull/12786" target="_blank">Fix 1</a>: Appropriate error handling.
* <a href="https://github.com/zulip/zulip/pull/12794" target="_blank">Fix 2</a>: Apply correct values when setting
up Django.

This project helped me to see the importance of planning, drafting, and editing when it comes to implementing
ideas in code.  It's one thing to have the right idea, but another thing entirely to have the best approach.

#### Moving forward

* <a href="https://github.com/zulip/zulip/pull/12682" target="_blank">Upload Casper images as CI artifacts</a>:
I simply added the images generated by a Casper test failure as a CircleCI artifact.This was a simple quality
of life improvement for folks running tests remotely.  <a href="https://github.com/zulip/zulip/pull/12700" target="_blank">Error output</a> was also added.

* <a href="https://github.com/zulip/zulip/pull/12688" target="_blank">Improve Ubuntu Trusty error messaging</a>:
We dropped support of Ubuntu Trusty due to its imminent EOL, folks still using it for development had the potential
to run into confusing errors.  This added output helps to mitigate confusion.

* <a href="https://github.com/zulip/zulip/pull/12727" target="_blank">Store Casper results as XML file</a>:
Casper tests have a tendency to be spurious regarding a consistent set of tests.  By adding this XUnit XML
file, it helps to make it immediately apparent if the failure is a typical Casper flake.

* <a href="https://github.com/zulip/zulip/pull/12699" target="_blank">Improve provision error messaging</a>: We see
quite a few folks have provision fail on `yarn install`, this is almost always due to network connectivity/proxy issues.
The updates I made here helps clarify this by de-cluttering the provision failure, as well as color-highlighting advice
to check the network.

* <a href="https://github.com/zulip/zulip/pull/12806" target="_blank">Add fix options to template lint</a>: This was 
a fun little project that added the `--fix` option to our handlebar template linter.  A relatively simple fix, that
has a big impact on improving developer quality of life.

* <a href="https://github.com/zulip/zulip/pull/12787" target="_blank">Migrate open statements to use context managers</a>:
This rather mechanical migration actually had a very organic lesson to teach me: It is important to slow down and ensure
that every change being made is understood, and verifiable as correct.

* <a href="https://github.com/zulip/zulip/pull/12836" target="_blank">Update syntax and comment in codecov.yml</a>:
A very small self-explanatory update.

* <a href="https://github.com/zulip/zulip/pull/12833" target="_blank">Remove remnants of new-user-bot</a>:  There
were a handful of occurrences where we still referenced the now non-existent new user bot.  I removed all such
occurrences in this pull request, which also aided work being done for a fellow GSoCer.

#### Mypy upgrades

Later in the summer I began work on upgrading mypy, as well as adding the new `--warn-unreachable` option,
which warns of redundant or unreachable lines of code.  Adding this option resulted in a large blob of files that needed
to be looked at as preparatory work in order to add the option by default.

* <a href="https://github.com/zulip/zulip/pull/12850" target="_blank">Upgrade to mypy 0.711</a>:
This upgrade came with big performance improvement, which led to mypy now being one of our fastest linters to check
the entire codebase.  This project was a great introduction to working with type annotations and hints in Python,
and paved the way for the larger `--warn-unreachable` work as part of the mypy 0.720 upgrade.

* <a href="https://github.com/zulip/zulip/pull/12893" target="_blank">Upgrade to mypy 0.720</a>:
This pull request was separated out from the `warn-unreachable` blob, so as to be merged while work continued on
addressing the discrepancies from the `--warn-unreachable` option.

* <a href="https://github.com/zulip/zulip/pull/12894" target="_blank">Fix warn-unreachable errors</a>:
  * This project was an awesome exercise in typing with Python.  The `warn-unreachable` option flagged many interesting
  errors in our codebase, as well as some more mundane errors that resulted from approaches being somewhat out of date
  (e.g., using `if False` when importing instead of `if TYPE_CHECKING`, which evaluates to `False` at runtime
and `True` when type checking.)  
  * Adding this option helped to clean up a lot of dead code that had been left
  over as a result of prior refactorings.   Working on this project helped improve my code reading skills,
  as well as building up a foundation of consistently referencing documentation when looking for standard
  approaches to issues.
  
### Export feature for the webapp
 
 This project has been on-going since April 2019.  It was an awesome feeling to see this pull request closed,
 and the "public export" functionality of this feature complete.  The journey here was an incredible learning
 experience in code-reviews, full stack development, and software engineering in general!
 
 * <a href="https://github.com/zulip/zulip/pull/11996" target="_blank">Export feature</a>
 
After working on this project, I can't imagine a more efficient way to get well acquainted with the Django framework.
Tim Abbott is an incredible software engineer, and a kind and patient teacher.  This project wouldn't have been possible
for me without his help.  With his guidance and feedback, I have come a long way from the software developer I was when
this project began.

## Work on the TODO list

I still have a few pull requests in progress that are both pending review, and in need of a new perspective
to achieve an optimal approach.

* <a href="https://github.com/zulip/zulip/pull/12271" taget="_blank">Add a replicate option to test-backend</a>:
This was a very fun and interesting project that replicates all of the tests that were ran in a failed process
during a run of the `test-backend` test suite.  The relevance of this option is that it give free reproducibility
when faced with a spurious failure to running the set of tests with high parallelism.

* <a href="https://github.com/zulip/zulip/pull/12575" target="_blank">Parallelize test-js-with-node</a>:
This is a somewhat tricky issue.  Attempts to simply borrow the approach from how we run our linters in
parallel (divide-and-conquer the set of tests--forking them off into separate processes) doesn't produce
an adequate boost in performance.
 
 
*My work during the summer can be found at my GitHub <a href="https://github.com/whoodes/zulip/projects/2" target="_blank">project</a>.*

*All of my merged commits can be found <a href="https://github.com/zulip/zulip/commits?author=whoodes" target="_blank">here</a>.*
