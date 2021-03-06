---
advisory: experimental
description: the plugin disable command description and usage
keywords:
- plugin, disable
menu:
  main:
    parent: smn_cli
title: plugin disable
---

# plugin disable (experimental)

```markdown
Usage:  docker plugin disable PLUGIN

Disable a plugin

Options:
      --help   Print usage
```

Disables a plugin. The plugin must be installed before it can be disabled,
see [`docker plugin install`](plugin_install.md).


The following example shows that the `no-remove` plugin is installed
and active:

```bash
$ docker plugin ls

NAME                  TAG                 DESCRIPTION                ACTIVE
tiborvass/no-remove   latest              A test plugin for Docker   true
```

To disable the plugin, use the following command:

```bash
$ docker plugin disable tiborvass/no-remove

tiborvass/no-remove
```

After the plugin is disabled, it appears as "inactive" in the list of plugins:

```bash
$ docker plugin ls

NAME                  TAG                 DESCRIPTION                ACTIVE
tiborvass/no-remove   latest              A test plugin for Docker   false
```

## Related information

* [plugin ls](plugin_ls.md)
* [plugin enable](plugin_enable.md)
* [plugin inspect](plugin_inspect.md)
* [plugin install](plugin_install.md)
* [plugin rm](plugin_rm.md)
