# Auth-py

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?label=license)](https://opensource.org/licenses/MIT)
[![CI](https://github.com/supabase-community/gotrue-py/actions/workflows/ci.yml/badge.svg)](https://github.com/supabase-community/gotrue-py/actions/workflows/ci.yml)
[![Python](https://img.shields.io/pypi/pyversions/gotrue)](https://pypi.org/project/gotrue)
[![Version](https://img.shields.io/pypi/v/gotrue?color=%2334D058)](https://pypi.org/project/gotrue)
[![Codecov](https://codecov.io/gh/supabase-community/gotrue-py/branch/main/graph/badge.svg)](https://codecov.io/gh/supabase-community/gotrue-py)
[![Last commit](https://img.shields.io/github/last-commit/supabase-community/gotrue-py.svg?style=flat)](https://github.com/supabase-community/gotrue-py/commits)
[![GitHub commit activity](https://img.shields.io/github/commit-activity/m/supabase-community/gotrue-py)](https://github.com/supabase-community/gotrue-py/commits)
[![Github Stars](https://img.shields.io/github/stars/supabase-community/gotrue-py?style=flat&logo=github)](https://github.com/supabase-community/gotrue-py/stargazers)
[![Github Forks](https://img.shields.io/github/forks/supabase-community/gotrue-py?style=flat&logo=github)](https://github.com/supabase-community/gotrue-py/network/members)
[![Github Watchers](https://img.shields.io/github/watchers/supabase-community/gotrue-py?style=flat&logo=github)](https://github.com/supabase-community/gotrue-py)
[![GitHub contributors](https://img.shields.io/github/contributors/supabase-community/gotrue-py)](https://github.com/supabase-community/gotrue-py/graphs/contributors)

This is a Python port of the [supabase js gotrue client](https://github.com/supabase/gotrue-js). The current state is that there is a features parity but with small differences that are mentioned in the section **Differences to the JS client**. As of December 14th, we renamed to repo to `auth-py` to mirror the changes in the JavaScript library.

## Installation

We are still working on making the `gotrue` python library more user-friendly. For now here are some sparse notes on how to install the module.

### Poetry

```bash
poetry add gotrue
```

### Pip

```bash
pip install gotrue
```

## Differences to the JS client

It should be noted there are differences to the [JS client](https://github.com/supabase/gotrue-js). If you feel particulaly strongly about them and want to motivate a change, feel free to make a GitHub issue and we can discuss it there.

Firstly, feature pairity is not 100% with the [JS client](https://github.com/supabase/gotrue-js). In most cases we match the methods and attributes of the [JS client](https://github.com/supabase/gotrue-js) and api classes, but is some places (e.g for browser specific code) it didn't make sense to port the code line for line.

There is also a divergence in terms of how errors are raised. In the [JS client](https://github.com/supabase/gotrue-js), the errors are returned as part of the object, which the user can choose to process in whatever way they see fit. In this Python client, we raise the errors directly where they originate, as it was felt this was more Pythonic and adhered to the idioms of the language more directly.

In JS we return the error, but in Python we just raise it.

```js
const { data, error } = client.sign_up(...)
```

The other key difference is we do not use pascalCase to encode variable and method names. Instead we use the snake_case convention adopted in the Python language.

Also, the `gotrue` library for Python parses the date-time string into `datetime` Python objects. The [JS client](https://github.com/supabase/gotrue-js) keeps the date-time as strings.

## Usage (outdated)

**Important:** This section is outdated, you can be guided by the [JS client documentation](https://supabase.github.io/gotrue-js) because this Python client has a lot of parity with the JS client.

To instantiate the client, you'll need the URL and any request headers at a minimum.

```python
from gotrue import SyncGoTrueClient

headers = {
    "apiKey": "my-mega-awesome-api-key",
    # ... any other headers you might need.
}
client: SyncGoTrueClient = SyncGoTrueClient(url="www.genericauthwebsite.com", headers=headers)
```

To send a magic email link to the user, just provide the email kwarg to the `sign_in` method:

```python
user: Dict[str, Any] = client.sign_up(email="example@gmail.com")
```

To login with email and password, provide both to the `sign_in` method:

```python
user: Dict[str, Any] = client.sign_up(email="example@gmail.com", password="*********")
```

To sign out of the logged in user, call the `sign_out` method. We can then assert that the session and user are null values.

```python
client.sign_out()
assert client.user() is None
assert client.session() is None
```

We can refesh a users session.

```python
# The user should already be signed in at this stage.
user = client.refresh_session()
assert client.user() is not None
assert client.session() is not None
```

## Contributions

We would be immensely grateful for any contributions to this project. 
