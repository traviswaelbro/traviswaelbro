---
title: "Creating Custom pre-commit Hooks"
date: 2018-11-05
---

## Creating Your First pre-commit Hook

Firstly, you'll need a repository that pre-commit can hook into. Typically, that
means you'll go to your Github account, create a new repository, and copy the
"Clone this Repo" link. Then, locally, you'll clone the repo, set up the minimum
viable example below, and commit your changes.

**In order for pre-commit to be able to test your repo, you must at least commit
your test hook locally.** If you make changes and want to test them locally,
you'll have to commit again before running the pre-commit test again.

For more on how to test your custom hook, see further down below.

### Minimum Viable Example

#### File Structure

```
├── __init__.py
├── pre-commit-hooks.yaml
├── hooks
│   ├── __init__.py
│   └── my_hook.py
└── setup.py
```

#### __init__.py

```
from . import hooks
```

#### pre-commit-hooks.py

```
-   id: my-hook
    name: My Hook Name
    description: My hook's useful description
    entry: my_hook
    language: python
    files: ''

```

#### hooks/__init__.py

```
from . import my_hook
```

#### hooks/my_hook.py

Typically, it is most readable to keep each custom hook as its own file. I
suggest keeping all of your hooks in a single directory.

I would also recommend that you try to write your hooks in a way that is as
version independent as possible. If you're writing Python 2- or Python 3-specific
code, at least update `setup.py` to match whatever versions you are supporting.

```
import argparse, sys


def my_hook(filename):
    with open(filename, mode='rb') as file_checked:
        # For example, reject the commit if the file as a set_trace in it
        return any('set_trace' in file_checked.read())


def main(argv=None):
    # Parse arguments
    parser = argparse.ArgumentParser()
    parser.add_argument('filenames', nargs='*', help='File names to check')
    args = parser.parse_args(argv)
    filenames = args.filenames

    # Perform your custom behavior
    files_with_conflicts = [for filename in filenames if my_hook(filename)]

    # Return response
    exit_code = 0
    if files_with_conflicts:
        exit_code = 1

    return exit_code


if __name__ == '__main__':
    sys.exit(main(sys.argv))
```

#### setup.py

The setup file is going to dictate what hooks you're going to be able to run.

In my example, I'm setting up a single hook, called `my_hook`. Notice how it's
being defined in the `console_scripts` section of `entry_points`. The path on
the right side of your `=` should match the path to your file. In this case,
that means we must have a file called `my_hook.py` which resides in the `hooks`
directory of our repo.

```
from setuptools import find_packages, setup

setup(
    name='pre-commit-hooks',
    description='Hooks for pre-commit',
    url='https://github.com/yourname/your-repo',
    version='0.0.1',

    author='Your Name',
    author_email='you@example.com',

    platforms='linux',
    classifiers=[
        'License :: OSI Approved :: MIT License',
        'Programming Language :: Python :: 2',
        'Programming Language :: Python :: 2.6',
        'Programming Language :: Python :: 2.7',
        'Programming Language :: Python :: 3',
        'Programming Language :: Python :: 3.3',
        'Programming Language :: Python :: 3.4',
        'Programming Language :: Python :: Implementation :: CPython',
        'Programming Language :: Python :: Implementation :: PyPy',
    ],

    packages=find_packages('.'),
    entry_points={
        'console_scripts': [
            'my_hook = hooks.my_hook:main',
        ],
    },
)

```

