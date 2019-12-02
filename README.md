# PHP Sniffer & Beautifier for VS Code

[![Current Version](https://vsmarketplacebadge.apphb.com/version/ValeryanM.vscode-phpsab.svg)](https://marketplace.visualstudio.com/items?itemName=ValeryanM.vscode-phpsab)
[![Install Count](https://vsmarketplacebadge.apphb.com/installs/ValeryanM.vscode-phpsab.svg)](https://marketplace.visualstudio.com/items?itemName=ValeryanM.vscode-phpsab)
[![Open Issues](https://vsmarketplacebadge.apphb.com/rating/ValeryanM.vscode-phpsab.svg)](https://marketplace.visualstudio.com/items?itemName=ValeryanM.vscode-phpsab)

# Special Notice 
This is very much an Alpha of combining my [vscode-phpcbf](https://github.com/valeryan/vscode-phpcbf) extension with the [Php Sniffer](https://github.com/wongjn/vscode-php-sniffer) extension created by `wongjn` and the auto config search capabilities of [vscode-phpcs](https://github.com/ikappas/vscode-phpcs) by `Ioannis Kappas`. I am currently testing this internally with my team and I am sure there are many bugs. It is currently only being tested on Linux. Other OS support will follow. 
#

This linter plugin for [Visual Studio Code](https://code.visualstudio.com/) provides an interface to [phpcs & phpcbf](http://pear.php.net/package/PHP_CodeSniffer/). It will be used with files that have the “PHP” language mode. This extension is designed to use auto configuration search mechanism to apply rulesets to files within a workspace. This is useful for developers who work with many different projects that have different coding standards.

## Installation

Visual Studio Code must be installed in order to use this plugin. If Visual Studio Code is not installed, please follow the instructions [here](https://code.visualstudio.com/Docs/editor/setup).

## Usage

<kbd>F1</kbd> -> `fixer: fix this file`

or keyboard shortcut `alt+shift+f` vs code default formatter shortcut

or right mouse context menu `Format Document`

or if format on save is enabled save document

## Linter Installation

Before using this plugin, you must ensure that `phpcs` is installed on your system. The preferred method is using [composer](https://getcomposer.org/) for both system-wide and project-wide installations.

Once phpcs is installed, you can proceed to install the vscode-phpsab plugin if it is not yet installed.

> **NOTE:** This plugin can detect whether your project has been set up to use phpcbf via composer and use the project specific `phpcs & phpcbf` over the system-wide installation of `phpcs & phpcbf` automatically. This feature requires that both composer.json and composer.lock file exist in your workspace root or the `phpsab.composerJsonPath` in order to check for the composer dependency. If you wish to bypass this feature you can set the `phpsab.executablePath` configuration setting.

> **NOTE:** `phpcbf` is installed along with `phpcs`.

### System-wide Installation

The `phpcs` linter can be installed globally using the Composer Dependency Manager for PHP.

1. Install [composer](https://getcomposer.org/doc/00-intro.md).
1. Require `phpcs` package by typing the following in a terminal:

    ```bash
    composer global require squizlabs/php_codesniffer
    ```

### Project-wide Installation

The `phpcs` linter can be installed in your project using the Composer Dependency Manager for PHP.

1. Install [composer](https://getcomposer.org/doc/00-intro.md).
1. Require `phpcs` package by typing the following at the root of your project in a terminal:

    ```bash
    composer require --dev squizlabs/php_codesniffer
    ```

### Plugin Installation

1. Open Visual Studio Code.
1. Press `Ctrl+P` on Windows or `Cmd+P` on Mac to open the Quick Open dialog.
1. Type ext install phpsab to find the extension.
1. Press Enter or click the cloud icon to install it.
1. Restart Visual Studio Code when prompted.

## Basic Configuration

There are various options that can be configured to control how the plugin operates which can be set
in your user, workspace or folder preferences.

### **phpsab.fixerEnable**

[ *Scope:* All | Optional | *Type:* boolean | *Default:* true ]

This setting controls whether `phpcbf` fixer is enabled.

### **phpsab.snifferEnable**

[ *Scope:* All | Optional | *Type:* boolean | *Default:* true ]

This setting controls whether `phpcs` sniffer is enabled.

### **phpsab.executablePathCS**

[ *Scope:* All | Optional | *Type:* string | *Default:* null ]

This setting controls the executable path for `phpcs`. You may specify the absolute path or workspace relative path to the `phpcs` executable.
If omitted, the plugin will try to locate the path parsing your composer configuration or look for an entry for 'phpcs' in your path.

### **phpsab.executablePathCBF**

[ *Scope:* All | Optional | *Type:* string | *Default:* null ]

This setting controls the executable path for the `phpcbf`. You may specify the absolute path or workspace relative path to the `phpcbf` executable.
If omitted, the plugin will try to locate the path parsing your composer configuration or look for an entry for 'phpcbf' in your path..

### **phpsab.standard**

[ *Scope:* All | Optional | *Type:* string | *Default:* null ]

This setting controls the coding standard used by `phpcbf`. You may specify the name, absolute path or workspace relative path of the coding standard to use.

> **NOTE:** While using composer dependency manager over global installation make sure you use the phpcbf commands under your project scope !

The following values are applicable:

1. This setting can be set to `null`, which is the default behavior and uses the `default_standard` when set in the `phpcs` configuration or fallback to the `Pear` coding standard.

    ```json
    {
        "phpsab.standard": null
    }
    ```

    You may set the `default_standard` used by phpcbf using the following command:

    ```bash
    phpcs --config-set default_standard <value>
    ```

    or when using composer dependency manager from the root of your project issue the following command:

    ```bash
    ./vendor/bin/phpcs --config-set default_standard <value>
    ```

1. The setting can be set to the name of a built-in coding standard ( ie. `MySource`, `PEAR`, `PHPCS`, `PSR1`, `PSR2`, `Squiz`, `Zend` ) and you are good to go.

    ```json
    {
        "phpsab.standard": "PSR2"
    }
    ```

1. The setting can be set to the name of a custom coding standard ( ie. `WordPress`, `Drupal`, etc. ). In this case you must ensure that the specified coding standard is installed and accessible by `phpcbf`.

    ```json
    {
        "phpsab.standard": "WordPress"
    }
    ```

    After you install the custom coding standard, you can make it available to phpcbf by issuing the following command:

    ```bash
    phpcs --config-set installed_paths <path/to/custom/coding/standard>
    ```

    or when using composer dependency manager from the root of your project issue the following command:

    ```bash
    ./vendor/bin/phpcs --config-set installed_paths <path/to/custom/coding/standard>
    ```

1. The setting can be set to the absolute path to a custom coding standard:

    ```json
    {
        "phpsab.standard": "/path/to/coding/standard"
    }
    ```

    or you can use the path to a custom ruleset:

    ```json
    {
        "phpsab.standard": "/path/to/project/phpcs.xml"
    }
    ```

1. The setting can be set to your workspace relative path to a custom coding standard:

    ```json
    {
        "phpsab.standard": "./vendor/path/to/coding/standard"
    }
    ```

    or you can use the path to your project's custom ruleset:

    ```json
    {
        "phpsab.standard": "./phpcs.xml"
    }
    ```

### **phpsab.autoConfigSearch**

[ *Scope:* All | Optional | *Type:* boolean | *Default:* true ]

Automatically search for any `.phpcs.xml`, `.phpcs.xml.dist`, `phpcs.xml`, `phpcs.xml.dist`, `phpcs.ruleset.xml` or `ruleset.xml` file to use as configuration. Overrides `phpsab.standard` configuration when a ruleset is found. If `phpcs` finds a configuration file through auto search this extension should similarly find that configuration file and apply fixes based on the same configuration.

> **NOTE:** This option does not apply for unsaved documents (in-memory). Also, the name of files that are searched for is configurable in this extension.

### **phpsab.allowedAutoRulesets**

[ *Scope:* All | Optional | *Type:* array | *Default:* [] ]

An array of filenames that could contain a valid phpcs ruleset.

```json
{
    "phpsab.allowedAutoRulesets": [
        "phpcs.xml",
        "special.xml",
    ]
}
```

### **phpsab.snifferMode**

[ *Scope:* All | Optional | *Type:* string | *Default:* onSave ]

Enum dropdown options to set Sniffer Mode to `onSave` or `onType`. 

1. `onSave`: The Sniffer will only update diagnostics when the document is saved.

1. `onType`: The Sniffer will update diagnostics as you type in a document.

### **phpsab.snifferTypeDelay**

[ *Scope:* All | Optional | *Type:* number | *Default:* 250 ]

When `snifferMode` is `onType` this setting controls how long to wait after typing stops to update. The number represents milliseconds. 

### **phpsab.snifferShowSources**

[ *Scope:* All | Optional | *Type:* boolean | *Default:* false ]

Determines if the Sniffer includes the source of the diagnostic data with error messages.

## Advanced Configuration

### **phpsab.composerJsonPath**

[ *Scope:* All | Optional | *Type:* string | *Default:* composer.json ]

This setting allows you to override the path to your composer.json file when it does not reside at the workspace root. You may specify the absolute path or workspace relative path to the `composer.json` file.

## Diagnosing common errors

### **phpsab.debug**

[ *Scope:* All | Optional | *Type:* boolean | Default: false ]

Write phpcbf stdout and extra debug information out to the console.

### The phpcs report contains invalid json

This error occurs when something goes wrong in phpcs execution such as PHP Notices, PHP Fatal Exceptions, Other Script Output, etc, most of which can be detected as follows:

Execute the phpcbf command in your terminal with --report=json and see whether the output contains anything other than valid json.

## Acknowledgements

This extension is based off of the `phpcs` extension created by [Ioannis Kappas](https://github.com/ikappas/vscode-phpcs/), the `PHP Sniffer` extension create by [wongjn](https://github.com/wongjn/vscode-php-sniffer) and the existing `phpcbf` extension by [Per Søderlind](https://github.com/soderlind/vscode-phpsab/). It uses some portions of these extensions to provide the `phpcs & phpcbf` functionality with auto config search.

## Contributing and Licensing

The project is hosted on [GitHub](https://github.com/valeryan/vscode-phpsab) where you can [report issues](https://github.com/valeryan/vscode-phpsab/issues), fork
the project and submit pull requests. See the [development guide](https://github.com/valeryan/vscode-phpsab/blob/master/DEVELOPMENT.md) for details.

The project is available under [MIT license](https://github.com/valeryan/vscode-phpsab/blob/master/LICENSE.md), which allows modification and redistribution for both commercial and non-commercial purposes.
