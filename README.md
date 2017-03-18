# Versionary

Add versioning to your Elixir Plug and Phoenix built API's

## Installation

The package can be installed by adding `versionary` to your list of dependencies
in `mix.exs`:

```elixir
def deps do
  [{:versionary, "~> 0.1.0"}]
end
```

## Plug API

Versionary is a set of plugs which can be used to verify the requested version
of your API.

### Versionary.Plug.VerifyHeader

Looks for a version in the `Accept` header.

```elixir
defmodule MyAPI.MyController do
  use MyAPI.Web, :controller

  plug Versionary.Plug.VerifyHeader, versions: ["application/vnd.app.v1+json"]
end
```

It's possible to expect another header to contain the versioning information.
You can specify this using the `header` option.

```elixir
defmodule MyAPI.MyController do
  use MyAPI.Web, :controller

  plug Versionary.Plug.VerifyHeader, header: "x-version",
                                     versions: ["application/vnd.app.v1+json"]
end
```

### Versionary.Plug.EnsureVersion

Looks for a verified version. If one is found, it continues, otherwise the
request is halted and a `call/1` is made to a supplied handler.

```elixir
defmodule MyAPI.MyController do
  use MyAPI.Web, :controller

  plug Versionary.Plug.EnsureVersion, handler: MyAPI.MyErrorHandler
end
```

If a handler is not supplied a default handler will be used.

The failure handler must receive the connection.
