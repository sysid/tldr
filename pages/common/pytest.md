# pytest

> Run Python tests.
> More information: <https://docs.pytest.org/>.

- Run tests from specific files:

`pytest {{path/to/test_file1.py path/to/test_file2.py ...}}`

- Run tests with names matching a specific [k]eyword expression:

`pytest -k {{expression}}`

- Exit as soon as a test fails or encounters an error:

`pytest --exitfirst`

- Run tests matching or excluding markers:

`pytest -m {{marker_name1 and not marker_name2}}`

- Run until a test failure, continuing from the last failing test:

`pytest --stepwise`

- Run tests without capturing output:

`pytest --capture=no`


# Operations  ......................................................................................
```bash
# search debug messages of dedicated test
LOG_LEVEL=DEBUG python -m pytest -s tests/service_layer/test_use_cases.py::TestSpamFilter::test_add_charging_feedback_spam_too_many_per_hour  2>&1 | grep Found
```

# Fixtures .........................................................................................
- pytest's tmpdir fixture should keep 3 versions of dirs around (no matter if the test failed or not) and delete older ones

## BeforAll, Teardown
```python
@pytest.fixture(scope="module", autouse=True)
def my_fixture():
    print ('INITIALIZATION')
    yield param
    print ('TEAR DOWN')
```
## autouse scoped
- [scoped autouse](https://docs.pytest.org/en/latest/how-to/unittest.html#using-autouse-fixtures-and-accessing-other-fixtures)
- flag fixture functions with @pytest.fixture(autouse=True) and define the fixture function in the context where you want it used
```python
# https://stackoverflow.com/a/50135020
@pytest.fixture()
def google():
    return requests.get("https://www.google.com")


class TestGoogle:

    @pytest.fixture(autouse=True)
    def _request_google_page(self, google):
        self._response = google

    def test_alive(self):
        assert self._response.status_code == 200

    def test_html_title(self):
        soup = BeautifulSoup(self._response.content, "html.parser")
        assert soup.title.text.upper() == "GOOGLE"
```

## Inheritance overwrite
```python
@pytest.fixture(scope='session')
def django_db_setup(
    request,
    django_db_setup,  # ensures the original fixture is called before the custom one.
    django_test_environment,
):
    # do custom stuff here
    print('my custom django_db_setup executing')
```

## "Redeclaration" overwrite
```python
@pytest.fixture(scope='session')
def django_db_setup(
    request,
    django_test_environment,
):
    print('original django_db_setup will not run at all')
```


# Marking tests ....................................................................................
https://docs.pytest.org/en/6.2.x/example/markers.html#marking-test-functions-and-selecting-them-for-a-run
- [register](https://docs.pytest.org/en/stable/mark.html#registering-marks) custom marks in `pytest.ini, pyproject.toml`
- mark tests with metadata and (inverse) select:
```python
    @pytest.mark.skipif(True, reason="Method is too slow")
    @pytest.mark.mark1

    # run tests with mark1 but not mark2:
    pytest -m 'mark1 and not mark2'

    # show all markers
    pytest --markers
```
## xfail vs skip
https://docs.pytest.org/en/6.2.x/skipping.html#xfail-mark-test-functions-as-expected-to-fail
- xfail means that you expect a test to fail for some reason,e.g feature not yet implemented, or a bug not yet fixed.
- When a test passes despite being expected to fail (marked with pytest.mark.xfail), it's an xpass and will be reported in the test summary.
- They serve as a sentinel for when you accidentally fix a bug
```python
# setup.cfg
[tool:pytest]
xfail_strict = true

# make it fail if test passes
@pytest.mark.xfail(strict=True)
def test_function():
    ...
```


# logging ..........................................................................................
- configure root logger in `conftest.py` OR use  live logging](https://docs.pytest.org/en/latest/logging.html#live-logs)
- run-config: `-p no:logging -s -o log_cli=True`
```ini
[pytest]
log_cli = 1
log_cli_level = INFO
log_cli_format = %(asctime)s [%(levelname)8s] %(message)s (%(filename)s:%(lineno)s)
log_cli_date_format=%Y-%m-%d %H:%M:%S

log_file = pytest.log
log_file_level = DEBUG
log_file_format = %(asctime)s [%(levelname)8s] %(message)s (%(filename)s:%(lineno)s)
log_file_date_format=%Y-%m-%d %H:%M:%S
```
