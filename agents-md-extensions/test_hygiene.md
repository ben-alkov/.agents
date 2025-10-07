# Test hygiene

Most test work involves pytest running via nox in virtual environments
specific to the code repository.

## Test Naming

- Test function names must start with `test_` (pytest requirement)
- Describe behavior being tested, not implementation details
- Avoid redundant words like "test" in the descriptive part
  - Bad: `test_test_user_login()`
  - Good: `test_user_login_with_valid_credentials()`
  - Better: `test_login_succeeds_with_valid_credentials()`

## Assertion Strictness

Make assertions as specific as possible to catch regressions.

- Prefer exact equality over partial matching
  - Bad: `assert 'error' in response`
  - Better: `assert response == {'status': 'error', 'code': 404}`
  - Best: `assert response == {'status': 'error', 'code': 404, 'message': 'Not found'}`

- Use deep equality for nested structures
  - `assert actual == expected` (pytest provides detailed diffs)
  - For complex objects: `assert vars(obj) == {'field': 'value'}`

- Match exact types when type matters
  - Bad: `assert value == 1` (passes for `True`)
  - Better: `assert value == 1 and isinstance(value, int)`

## Mocking Strategy

Use mocking as a last resort. Prefer real or fake implementations.

**Terminology:**

- **Mock**: Replace behavior using a mocking framework (unittest.mock,
  pytest-mock)
- **Fake**: Lightweight working implementation (e.g., in-memory database)
- **Stub**: Hardcoded return values for specific inputs
- **Fixture**: Reusable test data or setup

**Preference order:**

1. Use real implementations when fast (milliseconds)
2. Use fake implementations (e.g., fakeredis, sqlite in-memory)
3. Use recorded network traffic (vcrpy, responses)
4. Mock at the smallest boundary (HTTP client, not business logic)
5. Never mock your own business logic

**pytest-specific tools:**

- `monkeypatch` fixture for replacing attributes/environment
- `flexmock` for general mocking
- `unittest.mock.Mock` specifically for Async code
- `pytest-mock` for spy/stub helpers
- Avoid `@patch` decorators (use `monkeypatch` instead for clarity)

**Example - prefer fakes:**

```python
# Bad: Mocking database calls
def test_user_creation(mocker):
    mocker.patch('db.insert', return_value=123)
    result = create_user('alice')
    assert result.id == 123

# Good: In-memory database
def test_user_creation(tmp_path):
    db = Database(f'sqlite:///{tmp_path}/test.db')
    result = create_user('alice', db=db)
    assert db.query(User).filter_by(name='alice').one().id == result.id
```

**Example - mock at boundaries:**

```python
# Bad: Mocking internal logic
def test_weather_report(mocker):
    mocker.patch('weather.calculate_temperature', return_value=72)
    assert get_forecast() == 'Pleasant 72°F'

# Good: Mock external API client
def test_weather_report(monkeypatch):
    monkeypatch.setattr('weather.api_client.fetch',
                        lambda: {'temp': 72, 'unit': 'F'})
    assert get_forecast() == 'Pleasant 72°F'
```

*AFTER READING* test_hygiene.md, always say 'I have remembered the test_hygiene memory'
