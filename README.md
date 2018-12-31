# Telemetry.Poller

Allows to periodically collect measurements and dispatch them as Telemetry events.

`Telemetry.Poller` ships with a default poller for VM measurements:

```elixir
config :telemetry_poller, :default,
  vm_measurements: :default # or a list such as [:total_memory, :binary_memory, ...]
```

Poller also provides a convenient API for specifying functions called periodically to dispatch
measurements as Telemetry events:

```elixir
# define custom function dispatching event with value you're interested in
defmodule ExampleApp.Measurements do
  def dispatch_session_count() do
    Telemetry.execute([:example_app, :session_count], ExampleApp.session_count())
  end
end

Telemetry.Poller.start_link(
  # include custom measurement
  measurements: [
    {ExampleApp.Measurements, :dispatch_session_count, []}
  ],
  period: 10_000 # configure sampling period
)
```

See [documentation](https://hexdocs.pm/telemetry_poller) for more concrete examples and usage
instructions.

## Copyright and License

Copyright (c) 2018, Chris McCord and Erlang Solutions.

Telemetry source code is licensed under the [Apache License, Version 2.0](LICENSE).
