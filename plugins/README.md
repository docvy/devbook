
# Plugins

The Docvy App relies on Plugins to convert files of different content-types.

All plugins are expected to expose a well-defined interface. This interface is descibed here:

* [plugin metadata](#metadata)
* [plugin API](#api)
* [plugin Run](#run)
* [licensing](#licensing)
* [conventions](#conventions)


## interface v0.0

<a name="metadata"></a>
### metadata (package.json):

Every plugin requires a **package.json**. This file should be valid for [npm](https://npmjs.org). All the following key-value pairs are required:

```json
{
  "api_version": "0.0",
  "name": "a-no-space-name",
  "version": "0.0.0",
  "main": "index.js"
}
```

The **api_version** tells the server the interface your application is built with in mind. This will help incase the plugin API is changed.

The following are **optional**. They help provide more information to the user about your plugin, especially if your plugin errors:

```json
{
  "description": "from what content to what content do I convert",
  "homepage": "https://github.com/docvy/dp-markdown",
  "icon": "icon.png"
}
```


<a name="api"></a>
### API:

Once the plugin module has been imported, the following functions should be exposed:

#### plugin.accepts()

Should return an array of all the content-types the plugin can handle.


#### plugin.produces()

Should returns an array of all the content-types the plugin can produce.


#### plugin.run(rawData, expects, callback)

This function executes your plugin:

* `rawData` (String): the data read from the file
* `expects` (Array[String]): of the content-types the plugin can produce, this array holds those the server wants.
* `callback` (Function): once processing the data is complete, the plugin should call this function
  - this function's signature is: `callback(err, contentType, processedData)`
  - `err` (Error): if an error occurs. Otherwise, `null`.
  - `contentType` (String):the content-type of the final processed data
  - `processedData` (String): the actual processed/converted data


<a name="run"></a>
## plugin run:

All plugins are given **45 seconds** to respond by calling the callback with the processed data. After this timeout, the plugin process will be shutdown immediately.


<a name="licensing"></a>
## licensing:

All plugins should be licensed under the [MIT](http://opensource.org/licenses/MIT) license. This is to ensure all our users use our application freely without restrictions while protecting the developer from any legal action pertainig to the use of their plugin.


<a name="conventions"></a>
## conventions:

These are a set of **conventions**. They are **not** rules. They are just to make our lives better when developing and using the application.

1. Plugins be named with the prefix `dp-`. For example, `dp-markdown`. The `dp` abbreviates for **docvy plugin**
2. Plugins be uploaded to [npm](https://npmjs.org) for easier distribution.
3. Plugins should be thoroughly tested. Some of the basic tests include:

  * ensure the plugins ships with the basic metadata as in section <a href="#metadata">#metadata</a>
  * `plugin.accepts` is a **Function** that returns an **Array**
  * `plugin.produces` is a **Function** that returns an **Array**
  * `plugin.run` is a **Function** that converts the various content-types you claim your plugin does.

