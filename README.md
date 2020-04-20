# Coding Standards and Style Guides

## Overview

The aim of this document is to encourage a holistic and hopefully uniform approach to programming in at least `Python` and `C` for the purposes of the `Pipebots` project.

This involves all custom tools, utilities, firmware, and other software. Additionally, it can serve as a starting point to considering different hardware licensing options.

The rest of the document consists mostly of links to various frameworks, standards, and other authoritative documents, with some personal thoughts and suggestions from me. For the two last ones, I will put some emphasis on embedded programming safety and security.

This document is by no means exhaustive and final, and should be thought more as a living and breathing one.

## General concepts

This is a very brief and very high-level introduction to certain aspects that I feel are important when discussing software development, particularly when there is a possibility for us using one another's code and attempting to integrate everything into one well-functioning system.

### Version control

The cornerstone of modern software development. Its virtues are numerous and entire books have been written on the subject. [This](http://swcarpentry.github.io/git-novice/) course by Software carpentry is a good introduction. A very short and very quick description is that it is a much better way to collaboratively and iteratively develop and share software than sending `.zip` files as e-mail attachments or having filenames such as `Programme_V1.py`, `Programme_V2.py`, and so on.

Important to note that version control like `git` works for everything that is text-based. This includes things like `EAGLE` schematics and board layouts, which can be helpful when we get to the stage of designing and integrating custom hardware components.

### Code style, documentation, and comments

These relate to how we write code that is easy to read and therefore understand, how we document the code so that other people can use it, modify it, analyse it, and ultimately decide whether or not they can trust it. All of these also help **us** when we want to improve something, such as add a new functionality or rework and old one.

A somewhat recent example of how not commenting and documenting your code can cause mistrust was the reaction to [this tweet](https://twitter.com/neil_ferguson/status/1241835454707699713).

Writing documentation and comments is often seen as boring and tedious, however it is always an effort worth expending to ensure the software we develop is of high quality.

### Testing and mocking

This refers to writing very specific tests that ensure our software behaves as expected in a wide range of cases, including unexpected input, random errors happening, and so on. This is an important step in verifying and validating the software in a systematic way. Another benefit is that by running new versions of the software against old tests we can check if we have introduced new bugs.

A closely related concept is test coverage, i.e. how much of our code is covered by our tests? Have we made sure to check every possible branch, every potential situation our code might find itself in?

Finally, mocking in this case refers to creating something that behaves like the real thing, but is not. This helps with testing parts of our code that rely on external resources, without actually using said resources. For example, instead of querying a real database every time we run a test, we can have a simple mock function that returns an example of what the real database's response would be.

### Licensing

This is something that was briefly touched upon during the January Researchers' Meeting, and is something we will need to decide on as a team sooner rather than later. It referes to the terms and conditions under which we distribute and share the software we have written with one another. We are all part of the same team, but as we are employed by different organisations, there might be issues with that.

## Python-specific hints and tips

This section covers some general suggestions and advice on `Python` and `MicroPython` software development, including best practices, useful frameworks, and recommended workflows. Both languages are good for quickly prototyping solutions before embarking on implementing those in a more efficient manner. They are also quite useful for data post-processing, "glue" code, and general scripting and automation. For a lot of scientific and numeric computing `Python` can be a worthy alternative to `Matlab`.

The use of `Python 2` is heavily discouraged, as that version has already been [sunset](https://www.python.org/doc/sunset-python-2/). Most third-party libraries and frameworks should have been migrated to `Python 3` by now. In short, dunsetting `Python 2` means that no further development, updates, and patches will be released for it. This has security implications amongst other things.

The recommended minimum version of `Python` is `3.6`. This version includes several great features, such as type annotation for variables and the newest form of string formatting, called `f-strings`, which are much more succint and easy to use than `str.format()`.

List of recommended `Python` software development practices and associated frameworks and tools:

- Document your code. This means using comments throughout it to explain *why* and *what* is supposed to happen, not *how* it is happening. A nice tutorial is available [here](https://realpython.com/documenting-python-code/). Contact me if it is behind a paywall.
  - Reason: Enables maintenance, updating, and auditing the code. It also helps **you** when you come back to your code a few months later.
  - Associated tool and/or framework: [Google's docstring style](https://github.com/google/styleguide/blob/gh-pages/pyguide.md#38-comments-and-docstrings). Once we start having larger pieces of custom software, and we need to integrate different strands, using a documentation generation system such as [Sphinx](http://www.sphinx-doc.org/en/master/index.html) will also come in handy.
- Format your code consistently. Ideally this means using a linter such as [pylint](https://www.pylint.org/) or [flake8](https://flake8.pycqa.org/en/latest/) while writing your code, and being familiar and compliant with [PEP8](https://www.python.org/dev/peps/pep-0008/).
  - Reason: Makes the code look and feel consistent, makes the code self-documenting, helps **you** and other people quickly read and understand your code.
  - Associated tool and/or framework: [Black](https://black.readthedocs.io/en/stable/), in addition to the ones mentioned above. Just do not forget to pass the `--line-length 79` option to it.
- Use type hinting and type annotation when developing `Python` modules and packages
  - Reason: Improves overall code quality, static code analysis can catch some bugs, helps other people use and integrate your software. Read [this tutorial](https://realpython.com/python-type-checking/) for more info and how to get started. Contact me if it is behind a paywall.
  - Associated tool and/or framework: [MyPy](https://mypy.readthedocs.io/en/stable/)
- Check your code for common security issues
  - Reason: Should go without saying, but we want to produce good quality, safe, and secure code. Even though `Python` is rarely the language of choice for developing mission critical software, keeping to a good standard throughout the project should be encouraged. It also helps with building trust and conclusively demonstrating the safety and security of our solution.
  - Associated tool and/or framework: [Bandit](https://bandit.readthedocs.io/en/latest/) and this [blog post](http://stupidpythonideas.blogspot.com/2013/06/misra-c-and-python.html). Note that while there isn't anything like `MISRA` for `Python`, certain principles can be readily applied.
- Write unit and integration tests for your software
  - Reason: These can be an invaluable help with verification and validation of what the software you have developed does. It also helps with continuous development, giving you peace of mind that you have not introduced bugs. Have a look at [this tutorial](https://realpython.com/pytest-python-testing/) for more info, and again, contact me if it is behind a paywall.
  - Associated tool and/or framework: [pytest](https://docs.pytest.org/en/latest/contents.html) with the [pytest-cov](https://pytest-cov.readthedocs.io/en/latest/) plugin


## C-specific hints and tips

Most of the more general advice and reasoning given in the `Python` section applies to `C` as well. Generall, `C` is recommended and preferred over `C++` when discussing embedded software development, as there are fewer things that we can do wrongly when developing in `C`.

Furthermore, unlike `Python`, there are industry standards and guides on writing safe and secure code for mission-critical applications, such as automotive, aviation, and healthcare ones. There even are certification programmes which mark a particular codebase as compliant with the standard, but those can be expensive. The most prominent standard, [MISRA-C](https://www.misra.org.uk/) is not free. Tools that integrate with code editors and check for compliance with it are also not free.

There is an alternative standard, which is not as extensive, but is free and for the most part can be good enough - [Barr-C](https://barrgroup.com/embedded-systems/books/embedded-c-coding-standard). It is also included in this repo as a PDF. To my knowledge, there aren't any tools that automatically lint for compliance with its directives.

In contrast to `Python`, here using an older version of `C` is often encouraged, e.g. the most recent one recommended for use in embedded systems by `MISRA` is `C99`. The motivation behind this is that compilers and tools for that standard are mature with known and deterministic behaviour.

List of recommended `C` software development practices and associated frameworks and tools:

- Document your code
  - Reason: Same as with `Python`. A well-documented code is mandatory in the modern software development world.
  - Associated tool and/or framework: I would recommend [Doxygen](http://www.doxygen.nl/), which comes with its own commenting style. It is a bit verbose, but that is not necessarily a bad thing.
- Format your code consistently
  - Reason: Again, same as with `Python`. Well-formatted code is easy on the eyes, and is easy to maintain, update, and spot errors.
  - Associated tool and/or framework: The [Linux Kernel coding style](https://www.kernel.org/doc/html/v4.10/process/coding-style.html) seems like a good start. Unfortunately I am not aware of any linters or autoformatters for `C` that are compliant with it. Suggestions welcome.
- Writing safe and secure code
  - Reason: The `Pipebots` solution is trusted and performs well, amongst other things.
  - Associated tool and/or framework: Use of `MISRA-C` or `Barr-C` with code reviews to ensure compliance. Both standards forbid the use of `malloc()` which is one of the main sources of runtime issues with `C` code, but usting a tool like [Valgrind](https://valgrind.org/docs/manual/quick-start.html) can certainly help.
- Writing unit and integration tests for your software
  - Reason: Make sure the code you have written does what you expect it to do under a wide range of situations.
  - Associated tool and/or framework: [Ceedling](http://www.throwtheswitch.org/tools
), which has a focus on embedded systems. The main difficulty here is mocking hardware interfaces, but that is not an insurmountable obstacle. There are interesting blog posts [here](https://www.embedded.com/modern-unit-testing-in-c-with-tdd-and-ceedling/), [here](http://www.electronvector.com/blog/try-embedded-test-driven-development-right-now-with-ceedling), [here](http://www.electronvector.com/blog/mocking-embedded-hardware-interfaces-with-ceedling-and-cmock) and [here](https://interrupt.memfault.com/blog/unit-testing-basics). `PlatformIO` also seems to have a unit testing capability/framework, which looks to be more hardware-based. Still worth looking into.


## Licensing

Any code that we intend to share and/or publish will need an explicit license of some sort. For a good description of what happens when we do not include a license have a look [here](https://choosealicense.com/no-permission/).

My personal preference will be for adopting a permissive Open Source License, such as the [BSD](https://choosealicense.com/licenses/bsd-3-clause/) or the [MIT](https://choosealicense.com/licenses/mit/) one. There are several benefits of publishing code we create as Open Source, such as it being auditable by third parties, upholding the spirit of academic freedom, and complying with EPSRC [requirements](https://epsrc.ukri.org/newsevents/news/resoutputpolicy/) for publicly-funded research to be freely available. Furthermore, with this can also help with building trust, as anyone will be able to see *exactly* what we have programmed our robots to do. A good starting point if you want to learn more is the Open Source Initiative [website](https://opensource.org/faq).

Do note that Open Source does not preclude commercialisation of any works. The only thing that we need to be aware of is the [GNU General Public License](https://www.gnu.org/licenses/gpl-faq.en.html), as it is a "viral" license. That means that if we use any library or framework that is licensed under the GPL as part of a software we write, that software will also have to be GPL licensed. There are some notable exceptions to this, such as the ones for the GNU Compiler Collection. Most frameworks these days are licensed under the Lesser GPL (LGPL) which lacks the "infectiousness" of regular GPL.

In any case, there are plenty of options for an Open Source license, virtually all of which are summarised [here](https://choosealicense.com/appendix/). A custom proprietary license is also always an option, however that one will require extensive legal support.

The *recommended* way of specifying a license for code uploaded to an online version control system, such as GitHub, is to include a file name "LICENSE" at the root folder of the repository, containing the text of the chosen license.

## Contributing

Contributions are most welcome! Please let me know your thoughts and opinions on this, either via opening an issue here, creating a pull request, or via e-mail to [Viktor](mailto:eenvdo@leeds.ac.uk).

Particularly welcome will be any discussions on how to approach the licensing of software and hardware created as part of the `Pipebots` project, suggestions for `C` header file templates, or examples from your own practice on using any of the packages and frameworks listed above.

## Credits

Andrew Pickering from Theme 3 for originally mentioning the idea of having common header files to a consistent look and feel across the `Pipebots` project. It's what got me thinking about the various topics dicusses in this document.