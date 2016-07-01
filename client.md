# OK Client Setup

Examples of ok assignments are available to download.

[Download Lab >](http://su16.cs61a.org/lab/lab01/lab01.zip)
[Download Project >](http://su16.cs61a.org/proj/hog/hog.zip)


[Download Homework >](http://su16.cs61a.org/hw/hw01/hw01.zip)

## OK Configuration

Every OK assignment must have an OK configuration file. Here's an example from
Lab 1 (`lab01.ok`).

```json
{
    "name": "Lab 1",
    "endpoint": "cal/cs61a/fa15/lab01",
    "src": [
        "lab01.py",
        "lab01_extra.py"
    ],
    "tests": {
        "tests/truthiness.py": "ok_test",
        "tests/short_circuiting.py": "ok_test",
        "lab01.py:both_positive": "doctest",
        "tests/what_if.py": "ok_test",
        "tests/loops.py": "ok_test",
        "lab01_extra.py:factors": "doctest",
        "lab01_extra.py:falling": "doctest"
    },
    "protocols": [
        "restore",
        "export",
        "file_contents",
        "analytics",
        "unlock",
        "grading",
        "backup"
    ]
}
```

The `endpoint` field must be updated every semester. The `src` field must list
all student files that should be uploaded to the OK server.

## OK Tests

The `tests` field is a mapping from the path to the test source to the type of
test. There are two main types of OK tests: either `ok_test` or `doctest`.
Doctests are pulled straight from the student's Python source file. OK tests are
used both for unlocking tests, as well as tests that may require more setup
code. To add an `ok_test`, make sure there is a `tests` directory with an
`__init__.py` file so that `tests` is a module that can be imported.

Here's an example *WWPP* unlocking test from Lab 1 (`tests/truthiness.py`).

```python
test = {
  'name': 'Truthiness',
  'points': 0,
  'suites': [
    {
      'type': 'wwpp',
      'cases': [
        {
          'code': """
          >>> 0 or True
          True
          >>> not '' or not 0 and False
          True
          >>> 13 and False
          False
          """,
        },
      ]
    }
  ]
}
```

You can also have multiple choice *concept* unlocking tests that test more
conceptual questions. Here's an example snippet from Project 1 (`tests/05.py`).

```python
test = {
  ...
  'suites': [
    {
      'type': 'concept',
      'cases': [
        {
          'question': r"""
          The variables score0 and score1 are the scores for both
          players. Under what conditions should the game continue?
          """,
          'choices': [
            'While score0 and score1 are both less than goal',
            'While at least one of score0 or score1 is less than goal',
            'While score0 is less than goal',
            'While score1 is less than goal',
          ],
          'answer': 'While score0 and score1 are both less than goal',
        },
      ],
    },
  ],
  ...
}
```

The `ok-lock` command will lock the tests so that the answers are not viewable to students.