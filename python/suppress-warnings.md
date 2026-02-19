# Suppress Warnings

A selection of code snippets showing how to suppress warnings in Python's built-in logger.

## Basic Usage

```python
import warnings

warnings.filterwarnings(
    "ignore",
    category=UserWarning,
    module="transformers.models.marian.tokenization_marian"
)
```

## Exact Match

Note that the message is a regex:

```python
warnings.filterwarnings(
    "ignore",
    message=r"Recommended: pip install sacremoses\."
)
```

## Partial Match (Regex)

This means you can use partial matches:

```python
warnings.filterwarnings(
    "ignore",
    message=r".*sacremoses.*"
)
```

## Include Category (More Specific)

You can also filter by category:

```python
warnings.filterwarnings(
    "ignore",
    category=UserWarning,
    message=r".*sacremoses.*"
)
```

## Include Module (Most Specific)

```python
warnings.filterwarnings(
    "ignore",
    category=UserWarning,
    module=r"transformers\.models\.marian\.tokenization_marian",
    message=r".*sacremoses.*"
)
```

## Gotchas

Important gotchas:

* `message` is a regex, not a substring. This is why the `.` characters are escaped above.
* It must match the start unless you use `.*`
* Filters must be registered before the warning is emitted (i.e. before importing transformers)
