---
title: Visual Studio Code Extension
author: Nathaniel Sandberg
last updated: 4/1/2022
---

You can dynamically develop specs with the Visual Studio Code extension from Fortellis.
The Visual Studio Code extension does the following:

* Provides a live preview of what your spec is going to look like on the API Directory page.
* Provides live feedback on any errors that you may have in your spec.
* Enforces Fortellis specific rules that other editors may miss.
* Shows the error in the document on the line where the mistake is.
* Offers options to update on save or update on change.

With the Visual Studio Code extension, you can quickly create specs in a live editor that give you immediate feedback, helping you to publish specs faster.

> The Fortellis extension currently only supports Swagger specs.
> You must include your `securityDefinitions` or `security` field to pass the Visual Studio Code linter.

## Downloading the VS Code Extension

> You must use Visual Studio Code when writing your specs to install the extension and validate the spec. If you do not have Visual Studio Code, you can download it from the [Microsoft website](https://code.visualstudio.com/download).

You can go to the Microsoft Marketplace to download the extension.

1. Go to [Fortellis Spec Tools](https://marketplace.visualstudio.com/items?itemName=Fortellis.fortellis-spec-tools&ssr=false#overview).
1. Click install.
1. Follow the prompts on your computer.

You can also go to the extensions in Visual Studio Code.

1. Click the extensions icon on the left side of the editor in Visual Studio Code.
1. Click **Search Extensions in Marketplace**.
1. Type **Fortellis Spec Tools**.
1. Click the extension.
1. Click **install**.

## Configuring Visual Studio Code

You can configure Visual Studio Code to validate the specs when:

* You change spec files.
* You save spec files.

You can disable either of these.

### Disabling Either Validation

**Note:** With this extension, you validate the spec on change and on save by default.

1. In Visual Studio Code, click the extensions.
1. Search for **Fortellis Spec Tools**.
1. Click the gear icon on the Visual Studio Code extension.
1. Click **Extension Settings**.
1. Clear the option that you would like to disable.

## Using Fortellis Spec Tools

### Using the Spec Validator

When you install the extension, it automatically validates any specs that you have open. The validator does the following:

* Checks that the specs are valid Open API specs.
* Checks that the specs follow the Fortellis spec rules.
* Provides a red line under any lines that violate a rule.
* Provides information on how to possibly fix the spec error.
    * To see more information on how to fix the error, hover over the text with the red underline.

![Preview with Rule Hint]($[docsUrl]/static/images/specErrors.PNG)

### Using the Spec Viewer

1. In Visual Studio Code, type **Ctrl**+**Shift**+**P**.
1. Type the command **Preview**.
1. Click **Fortellis Spec: Preview** that autopopulates as you type.

You can now view the spec as it will appear when you upload the spec to Fortellis and publish it to the [API Directory]($[apiReferenceUrl]).

![Preview with Passing Spec]($[docsUrl]/static/images/specPreviewer.PNG)
