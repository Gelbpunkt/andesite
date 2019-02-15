# Plugins

Plugins allow extending andesite's functionality with your own code. 

During startup, andesite will look for plugins in a folder named `plugins`,
in the current working directory, if it exists. Everything inside it will
be treated as a [plugin root](#plugin-root).

Additional plugin roots may be provided with the `extra-plugins` configuration.

Andesite will fail to start if *any* plugin fails to load.

An example plugin is available [here](https://github.com/natanbc/andesite-node/blob/master/example-plugin/src/main/java/example/ExamplePlugin.java)

The plugin API is also available as a jar in the releases.

## Plugin Root

A plugin root is either a jar file or a directory. Plugins must define a file named
`manifest.json` in the root, with the following format:

```json
{
    "classes": [
        "plugin.class.One",
        "plugin.class.Two"
    ]
}
```

If the manifest is missing, andesite will ignore the root. If present, the manifest **must** be
a valid json object and contain the `classes` key, or loading will fail. The array may be left empty,
although that would render the plugin useless, as all plugins are completely isolated from each other.

All entry points defined in `manifest.json` must implement the [`Plugin`](https://github.com/natanbc/andesite-node/blob/master/api/src/main/java/andesite/node/Plugin.java)
class.

The classes must also be contained in the plugin root, with their path being
`ROOT_PATH + "/" + CLASS_NAME.replace('.', '/') + ".class"`. Jar files built by any
build tool already follow this format, so no extra work is required.

A valid plugin root looks like the following

```
<ROOT>/
├── my/
|   └── package/
|       └── MyPlugin.class
└── manifest.json
```

## Plugin API

The plugin API is exposed with the `api` module. To install it, add [jitpack](https://jitpack.io)
as a maven repository and the API dependency:

```xml
<dependency>
    <groupId>com.github.natanbc.andesite-node</groupId>
    <artifactId>api</artifactId>
    <version>VERSION</version>
</dependency>
```

```gradle
compile 'com.github.natanbc.andesite-node:api:VERSION'
```

The `Plugin` interface defines the callbacks andesite will call after loading your plugin.