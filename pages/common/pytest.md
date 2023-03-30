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
# Mocking ..........................................................................................
- `mock.patch()` takes a string which will be resolved to an object when applying the patch,
- `mock.patch.object()` takes a direct reference to object, must be imported.
- `patch` returns `MagicMock`: has asserts for testing how it is called, return-values, side-effects
- `side_effect`: replacement function with same signature as mocked one
- limit mock of `builtins` by using smallest possible scope: `context manager`

Speccing prevents mock problems:
- `spec`: prevents getting attributes which do not exist: creates correct attributes and call signatures for functions (always use it)
- `autospec`: stricter than spec, recursively creates correct attributes
- `spec_set`: prevents setting attributes which do not exist
- `kwargs`: keep all settings/config in one place:
```python
    @patch('my_module.Foo.name', return_value='Thomas')
    @patch('my_module.Foo', name='Thomas', month='April')
```
- mock.MagicMock instance, or an auto-spec: always favor using an `auto-spec`, as it helps keep your tests sane for future changes.
  This is because mock.Mock and mock.MagicMock accept all method calls and property assignments regardless of the underlying API.

## builtins (mock 'input')
- use global path:
```python
mocker.patch('builtins.input', new=lambda _: "enter")
```

## Environment, Dict
!!! https://docs.pytest.org/en/latest/how-to/monkeypatch.html#monkeypatching-environment-variables
- `patch.dict` does not inject an argument
- to clear `os.environ` so only the given variables are set: `clear=True` (deleting keys not possible)
```python
# module-level auto-use
@pytest.fixture(autouse=True)
def mock_settings_env_vars():
    with mock.patch.dict(os.environ, {"XXX_COLOUR": "ROUGE"}, clear=True):
        yield
```

## Class
You can always provide your own mock instead of the default MagicMock instance by passing it to the mock.patch call:
- class methods, static methods and properties: patch these on the class rather than an instance.
- patch attribute: use `new=` (see example below)
```python
# replace entire objects not attributes:
mocked = mocker.patch("gldpm.routers.authentication.dependencies.config", config)
def test_something():
    with mock.patch('tested_module.Foo', TestFoo):
        assert tested_module.my_function() == 'not important'

# mocking coroutine in class
@pytest.mark.asyncio
@mock.patch('simulation.agent_control.Agent.send_charging_point', autospec=True, side_effect=xxx)

# patch attribute with new value
@patch('dayaheadforecast.tasks.calc_flex_blocks.config.flexblock', new=(datetime.time(hour=10), datetime.time(hour=15)))

# patch with return value
@mock.patch('simulation.helper.data_access_layer.get_chargepoint_by_evse_id', autospec=True, return_value=chargepoint())
def test_build_tx(chargepoint_mock):
```

## asyncio
[asyncio](asyncio#testing)


## patch
[Important: patch where the object is used/imported, in contrast to where the object is defined](https://docs.python.org/3/library/unittest.mock.html#where-to-patch)
```python
import db
@patch('db.db_write') == @patch('my_module.db.db_write')  # verbose but currect

# module level mocking
@pytest.fixture(autouse=True)
def mock_post():
    with mock.patch('simulation.agent_control.Agent.send_charging_point', autospec=True) as mocked:
        yield mocked

# order: backwards
@mock.patch('mymodule.sys')
@mock.patch('mymodule.os')
@mock.patch('mymodule.os.path')
def test_something(self, mock_os_path, mock_os, mock_sys):
    pass
```

## Entire Package/Module
```python
def test_function(mocker):
    # Create a mock object for the vim package
    mock_vim = mocker.MagicMock()

    # 1. Set the mock object as an attribute of the module
    mocker.patch.dict('sys.modules', {'vim': mock_vim})

    # 2. Create a mock object for the vim package with the same attributes as the real package
    #mock_vim = mocker.create_autospec(vim, autospec=True)

    # Import the module under test
    import module_under_test

    # Now, when the module under test imports the vim package, it will use the mock object instead
    assert module_under_test.vim == mock_vim

# easier to use since you don't have to import it again in the test function
@mocker.patch('module_under_test.vim', new=mock_vim)
def test_function(mocker):
    # Import the module under test
    import module_under_test

    # Now, when the module under test imports the vim package, it will use the mock object instead
    assert module_under_test.vim == mock_vim
```

## Mock for entire class
```python
import pytest

@pytest.fixture
def mock_database():
    # Create a mock database object
    mock_db = MockDatabase()

    # Set up the mock object
    mock_db.setup()

    yield mock_db

    # Tear down the mock object
    mock_db.teardown()

@pytest.mark.usefixtures("mock_database")
class TestClass:
    def test_method_1(self, mock_database):
        # Use the mock database object in the test
        result = mock_database.query("SELECT * FROM table")
        assert result == expected_result

    def test_method_2(self, mock_database):
        # Use the mock database object in the test
        result = mock_database.query("SELECT * FROM table")
        assert result == expected_result
```
## Mock config object
```python
    mock_sqs_client = mocker.patch("poi_fb_backend.service_layer.use_cases.config.sqs")

    assert mock_sqs_client.send_message.call_count == 1
    assert mock_sqs_client.send_message.ssert_called_once_with(
        QueueUrl=config.sqs_queue_url,
        MessageGroupId=config.sqs_message_group_id,
    )
```

## Gotcha
- location/target is a STRING
- mocked object must be passed as parameter to test
- at import-time code at the top-level of modules is executed, including class bodies.
- For classes at import-time:
    - interpreter executes the body of every class, even the body of classes nested in other classes.
    - Execution of a class body means that the attributes and methods of the class are defined, and then the class object itself is built.


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

# Other ............................................................................................
[Mutant Testing](https://github.com/boxed/mutmut)

## BDD
https://blog.testproject.io/2019/05/16/python-testing-framework-pros-cons/ -> [pytest-bdd](https://github.com/pytest-dev/pytest-bdd)


# Resource .........................................................................................
- https://github.com/pytest-dev/pytest-mock
- https://changhsinlee.com/pytest-mock/
- https://www.toptal.com/python/an-introduction-to-mocking-in-python
- https://chase-seibert.github.io/blog/2015/06/25/python-mocking-cookbook.html
- [Special Case: return value and PropertyMock](https://stackoverflow.com/questions/16867509/mock-attributes-in-python-mock/29510636#29510636)
