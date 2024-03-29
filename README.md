# asyncio-task-manager

[![PyPI - Version](https://img.shields.io/pypi/v/asyncio-task-manager.svg)](https://pypi.org/project/asyncio-task-manager)
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/asyncio-task-manager.svg)](https://pypi.org/project/asyncio-task-manager)

asyncio-task-manager is a Python library that provides an easy-to-use interface for managing asyncio tasks with dependencies. It allows you to create tasks, specify their dependencies, and run them concurrently in a thread pool.

---

**Table of Contents**

- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Example](#example)
- [License](#license)

## Features

- Create tasks with unique task IDs and coroutine functions
- Specify dependencies between tasks
- Run tasks concurrently, respecting a maximum thread count
- Ensure tasks are run only after all their dependencies have finished running

## Installation

asyncio-task-manager is available on [PyPi](https://pypi.org/project/asyncio-task-manager):

```console
pip install asyncio-task-manager
```

## Usage

```python
TaskManager(max_threads: int)
```

Creates a new task manager instance with the specified maximum thread count.

```python
create_task(task_id: str, coroutine: Callable[..., Coroutine], *args, **kwargs) -> None
```

Creates a new task with the given task ID and coroutine function.

```python
add_dependency(task_id: str, dependency_id: str) -> None
```

Adds a dependency between two tasks, indicating that task_id depends on the completion of dependency_id.

```python
run_tasks() -> None
```

Runs all tasks in the dependency graph, respecting the maximum thread count.

## Example

Here's a basic example. We create three tasks (task1, task2, and task3) and specify that task2 and task3 depend on task1. We set the maximum thread count to 2. The task manager ensures that task1 is run first, and then task2 and task3 are run concurrently.:

```python
from asyncio_task_manager import TaskManager
import asyncio

async def task1():
    print("Running task 1")
    await asyncio.sleep(1)
    print("Task 1 completed")

async def task2():
    print("Running task 2")
    await asyncio.sleep(2)
    print("Task 2 completed")

async def task3():
    print("Running task 3")
    await asyncio.sleep(1)
    print("Task 3 completed")

async def main():
    task_manager = TaskManager(max_threads=2)

    task_manager.create_task("task1", task1)
    task_manager.create_task("task2", task2)
    task_manager.create_task("task3", task3)

    task_manager.add_dependency("task2", "task1")
    task_manager.add_dependency("task3", "task1")

    await task_manager.run_tasks()

asyncio.run(main())
```

## License

`asyncio-task-manager` is distributed under the terms of the [MIT](https://spdx.org/licenses/MIT.html) license.
