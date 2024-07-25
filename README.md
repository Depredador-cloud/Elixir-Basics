# Elixir-Basics

## Introduction

Elixir is a dynamic, functional language designed for building scalable and maintainable applications. It leverages the Erlang VM, known for running low-latency, distributed, and fault-tolerant systems.

## Table of Contents
- [Getting Started](#getting-started)
- [Basic Syntax](#basic-syntax)
- [Data Types](#data-types)
- [Pattern Matching](#pattern-matching)
- [Control Structures](#control-structures)
- [Functions](#functions)
- [Modules](#modules)
- [Concurrency](#concurrency)
- [Mix - Build Tool](#mix---build-tool)
- [Testing](#testing)

## Getting Started

### Installation

To install Elixir, follow the official guide [here](https://elixir-lang.org/install.html). Ensure you have Erlang installed as Elixir depends on it.

```bash
$ elixir -v
Erlang/OTP 24 [erts-12.0] [source] [64-bit] [smp:4:4] [ds:4:4:10] [async-threads:1] [jit]

Elixir 1.12.0 (compiled with Erlang/OTP 24)
```

## Basic Syntax

### Interactive Shell (IEx)

Elixir's interactive shell, IEx, is a powerful tool to try out code.

```bash
$ iex
Erlang/OTP 24 [erts-12.0] [source] [64-bit] [smp:4:4] [ds:4:4:10] [async-threads:1] [jit]

Interactive Elixir (1.12.0) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)>
```

### Basic Types

Elixir has several basic types: integers, floats, booleans, atoms, strings, lists, and tuples.

```elixir
iex> 1         # integer
iex> 0x1F      # integer
iex> 1.0       # float
iex> true      # boolean
iex> :atom     # atom
iex> "hello"   # string
iex> [1, 2, 3] # list
iex> {1, 2, 3} # tuple
```

## Data Types

### Numbers

Elixir supports integers and floating-point numbers.

```elixir
iex> 1 + 1
2
iex> 10 / 2
5.0
iex> div(10, 2)
5
iex> rem(10, 3)
1
```

### Booleans

Elixir uses `true` and `false` for boolean values.

```elixir
iex> true and false
false
iex> true or false
true
iex> not true
false
```

### Atoms

Atoms are constants where their name is their value.

```elixir
iex> :hello
:hello
iex> :hello == :world
false
```

### Strings

Strings in Elixir are UTF-8 encoded binaries.

```elixir
iex> "hello"
"hello"
iex> String.length("hello")
5
iex> "hello " <> "world"
"hello world"
```

### Lists

Lists are a collection of values.

```elixir
iex> [1, 2, 3]
[1, 2, 3]
iex> length([1, 2, 3])
3
iex> [1, 2, 3] ++ [4, 5, 6]
[1, 2, 3, 4, 5, 6]
iex> [1, 2, 3] -- [2]
[1, 3]
iex> hd([1, 2, 3])
1
iex> tl([1, 2, 3])
[2, 3]
```

### Tuples

Tuples store a fixed number of elements.

```elixir
iex> {:ok, "hello"}
{:ok, "hello"}
iex> tuple_size({:ok, "hello"})
2
iex> elem({:ok, "hello"}, 1)
"hello"
iex> put_elem({:ok, "hello"}, 1, "world")
{:ok, "world"}
```

## Pattern Matching

Pattern matching is a powerful feature in Elixir.

```elixir
iex> {a, b, c} = {:hello, "world", 42}
{:hello, "world", 42}
iex> a
:hello
iex> [head | tail] = [1, 2, 3]
[1, 2, 3]
iex> head
1
iex> tail
[2, 3]
```

## Control Structures

### `if` and `unless`

```elixir
iex> if true do
...>   "This works!"
...> else
...>   "This will never be seen"
...> end
"This works!"

iex> unless false do
...>   "This works!"
...> end
"This works!"
```

### `case`

```elixir
iex> case {1, 2, 3} do
...>   {4, 5, 6} ->
...>     "This won't match"
...>   {1, x, 3} ->
...>     "This will match and bind x to 2"
...>   _ ->
...>     "This would match any value"
...> end
"This will match and bind x to 2"
```

### `cond`

```elixir
iex> cond do
...>   2 + 2 == 5 ->
...>     "This is not true"
...>   2 * 2 == 3 ->
...>     "Nor this"
...>   1 + 1 == 2 ->
...>     "But this is"
...> end
"But this is"
```

## Functions

### Anonymous Functions

```elixir
iex> add = fn a, b -> a + b end
iex> add.(1, 2)
3
```

### Named Functions

```elixir
defmodule Math do
  def sum(a, b) do
    a + b
  end
end

iex> Math.sum(1, 2)
3
```

### Function Capture

```elixir
iex> add = &(&1 + &2)
iex> add.(1, 2)
3
```

## Modules

Modules are a way to group functions.

```elixir
defmodule Math do
  def sum(a, b) do
    a + b
  end

  def zero?(0), do: true
  def zero?(x) when is_integer(x), do: false
end

iex> Math.sum(1, 2)
3
iex> Math.zero?(0)
true
iex> Math.zero?(3)
false
```

## Concurrency

Elixir provides concurrency with lightweight processes.

### Spawning Processes

```elixir
iex> spawn(fn -> IO.puts("Hello from a process!") end)
Hello from a process!
```

### Message Passing

```elixir
defmodule Greeter do
  def greet do
    receive do
      {sender, msg} ->
        send(sender, {:ok, "Hello, #{msg}"})
    end
  end
end

pid = spawn(Greeter, :greet, [])

send(pid, {self(), "World!"})

receive do
  {:ok, message} ->
    IO.puts(message)
end
```

## Mix - Build Tool

Mix is a build tool that provides tasks for creating, compiling, and testing Elixir projects.

### Creating a New Project

```bash
$ mix new my_app
$ cd my_app
```

### Running Tests

```bash
$ mix test
```

## Testing

Elixir uses ExUnit for writing tests.

### Example Test

```elixir
defmodule MathTest do
  use ExUnit.Case
  doctest Math

  test "sum of two numbers" do
    assert Math.sum(1, 2) == 3
  end
end
```

Run the tests with `mix test`.

## Conclusion

This guide covers the basics of Elixir. For more in-depth knowledge, refer to the [official documentation](https://elixir-lang.org/docs.html) and explore the rich ecosystem of libraries and tools.

```

This guide covers essential aspects of Elixir, providing a solid foundation for further exploration and development. Let me know if you need additional sections or specific examples.
