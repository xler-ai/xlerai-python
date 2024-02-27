# XlerAI Python API library

[![PyPI version](https://img.shields.io/pypi/v/xlerai.svg)](https://pypi.org/project/xlerai/)

The XlerAI Python library provides convenient access to the XlerAI REST API from any Python 3.7+
application. The library wraps the [openai python library](https://github.com/openai/openai-python),
points it at `api.xler.ai/v1` for you, and extends their library further with additional functionality.


## Documentation

Documentation can be found [on docs.xler.ai](https://docs.xler.ai/sdks/python).

## Installation

```sh
pip install xlerai
```

## Usage

The full API of this library can be found in [api.md](api.md).

```python
import os
from xlerai import XlerAI

client = XlerAI(
    # This is the default and can be omitted
    api_key=os.environ.get("OPENAI_API_KEY"),
)

chat_completion = client.chat.completions.create(
    messages=[
        {
            "role": "user",
            "content": "Say this is a test",
        }
    ],
    model="gpt-3.5-turbo",
)
```

While you can provide an `api_key` keyword argument,
we recommend using [python-dotenv](https://pypi.org/project/python-dotenv/)
to add `OPENAI_API_KEY="My API Key"` to your `.env` file
so that your API Key is not stored in source control.

## Async usage

Simply import `AsyncXlerAI` instead of `XlerAI` and use `await` with each API call:

```python
import os
import asyncio
from xlerai import AsyncXlerAI

client = AsyncXlerAI(
    # This is the default and can be omitted
    api_key=os.environ.get("OPENAI_API_KEY"),
)


async def main() -> None:
    chat_completion = await client.chat.completions.create(
        messages=[
            {
                "role": "user",
                "content": "Say this is a test",
            }
        ],
        model="gpt-3.5-turbo",
    )


asyncio.run(main())
```

Functionality between the synchronous and asynchronous clients is otherwise identical.

## Streaming Responses

We provide support for streaming responses using Server Side Events (SSE).

```python
from xlerai import XlerAI

client = XlerAI()

stream = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Say this is a test"}],
    stream=True,
)
for chunk in stream:
    print(chunk.choices[0].delta.content or "", end="")
```

The async client uses the exact same interface.

```python
from xlerai import AsyncXlerAI

client = AsyncXlerAI()


async def main():
    stream = await client.chat.completions.create(
        model="gpt-4",
        messages=[{"role": "user", "content": "Say this is a test"}],
        stream=True,
    )
    async for chunk in stream:
        print(chunk.choices[0].delta.content or "", end="")


asyncio.run(main())
```

## Requirements

Python 3.7 or higher.
