---
title: "Creating your first integration"
---

Alright, so it's time to write your first code for your integration. AWESOME. Don't worry, we've tried hard to keep it as easy as possible. For this first integration, we are going to create a Pseudo Random Number Generator (PRNG) integration as an example.

From a Home Assistant development environment, type the following and follow the instructions:

```shell
python3 -m script.scaffold integration
```

```shell
vscode ➜ /workspaces/ha-core (dev) $ python3 -m script.scaffold integration

What is the domain?
> prng

What is the name of your integration?
> sample_prng

What is your GitHub handle?
> rotinom

Error: GitHub handles need to start with an "@"

What is your GitHub handle?
> @rotinom

What PyPI package and version do you depend on? Leave blank for none.
> 

How will your integration gather data?

Valid values are assumed_state, calculated, cloud_polling, cloud_push, local_polling, local_push

More info @ https://developers.home-assistant.io/docs/creating_integration_manifest#iot-class

> local_push

Does Home Assistant need the user to authenticate to control the device/service? (yes/no) [yes]
> no

Is the device/service discoverable on the local network? (yes/no) [no]
> no

Is this a helper integration? (yes/no) [no]
> no

Can the user authenticate the device using OAuth2? (yes/no) [no]
> no

<command line output...>

The next step is to look at the files and deal with all areas marked as TODO.
```

You will now be able to see a newly-created dirctory under `<container>/homeassistant/components/prng`.  We will refer to this directory as `<home>`


### Update Manifest
First things first, we need to make sure that we setup the manifest.  This is a data file that tells HA about your integration. 
 You can read about more details later, but for now we're going to make a quick change.  Let's explicitly set the integration type to be a `hub`.  Wile not strictly required, this is a best practices.  Your file should look something like:

```
Edit <home>/manifest.json

{
  "domain": "prng",
  "name": "sample_prng",
  "codeowners": [
    "@rotinom"
  ],
  "config_flow": true,
  "dependencies": [],
  "documentation": "https://www.home-assistant.io/integrations/prng",
  "homekit": {},
  "iot_class": "local_push",
  "requirements": [],
  "ssdp": [],
  "zeroconf": [],

# Add this line
  "integration_type": "hub"
}
```








### Configure Home Assistant
We now need to let Home Assistant know that there's a new component available.  To do this, we need to edit `<container>/config/configuration.yaml` and add a configuration line at the end of the file for the new integration:

```
# Add to <container>/config/configuration.yaml

prng:
```

### Validate integration:
```
python3 -m script.hassfest
```



This will set you up with everything that you need to build an integration that is able to be set up via the user interface. More extensive examples of integrations are available from [our example repository](https://github.com/home-assistant/example-custom-config/tree/master/custom_components/).

## The minimum

The scaffold integration contains a bit more than just the bare minimum. The minimum is that you define a `DOMAIN` constant that contains the domain of the integration. The second part is that it needs to define a setup method that returns a boolean if the set up was successful.

```python
DOMAIN = "hello_state"


def setup(hass, config):
    hass.states.set("hello_state.world", "Paulus")

    # Return boolean to indicate that initialization was successful.
    return True
```

And if you prefer an async component:

```python
DOMAIN = "hello_state"


async def async_setup(hass, config):
    hass.states.async_set("hello_state.world", "Paulus")

    # Return boolean to indicate that initialization was successful.
    return True
```
Create a file `<config_dir>/custom_components/hello_state/__init__.py` with one of the two codeblocks.
In addition a manifest file is required with below keys as the bare minimum. Create `<config_dir>/custom_components/hello_state/manifest.json`.

```json
{
  "domain": "hello_state",
  "name": "Hello, state!",
  "version": "0.1.0"
}
```

To load this, add `hello_state:` to your `configuration.yaml` file. 

## What the scaffold offers

When using the scaffold script, it will go past the bare minimum of an integration. It will include a config flow, tests for the config flow and basic translation infrastructure to provide internationalization for your config flow.
