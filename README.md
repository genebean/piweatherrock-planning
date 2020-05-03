# PiWeatherRock Planning Document

This repository represents a blueprint for how the components of PiWeatherRock
will be structured / restructured. Each top-level folder represents a git
repository.

* `genebean-piweatherrock` represents a Puppet module that will be used for
  deploying and configuring PiWeatherRock
* The core PiWeatherRock repository will be transitioned into a Python package
  that contains the core components of the application.
* Each of the `piweatherrock-*` repositories will represent a single Python
  package that can be consumed from PyPI.

## Python Namespacing

The core components are in `piweatherrock` directly. Core plugins are bundled
into that distribution and are formatted exactly like 3rd party plugins.

## Weather providers

The code to format data from an API to work with PiWeatherRock is stored in
external packages named `piweatherrock.data_<provider name>.pwr_data`. The
package should contain a module named `pwr_data` under the sub-package
`data_<provider name>`. It can then be imported for use like so (using
ClimaCell as an example):

```python
# To use a different provider just change this line
from piweatherrock.data_climacell import pwr_data

# Usage of ^^
forecast = pwr_data.Forecast()
```

Placing the logic within a module that is always named `pwr_data` ensures that
the core application can always refer to it the same way without having to use
an alias.

Each provider is also expected to have a script file that can be used as a cli
for seeing what data is being sent to the core app.

## Plugins

Core plugins should be packaged as sub-packages within the core application.
3rd party plugins will be published as separate packages. To be used they
will need to be imported from within the core application.

Each plugin, core or otherwise, should be named `plugin_<foo>` and should be
a sub-package of `piweatherrock`. A plugin can be imported like so (using the
hourly screen as an example):

```python
from piweatherrock import plugin_hourly
```

## Web-based configuration package

The web-based configuration utility is an external package so that it can
at a different pace than the core application.
