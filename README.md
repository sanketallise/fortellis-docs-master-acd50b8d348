# Getting started writing documentation for Fortellis docs

On Fortellis documenation there are three main concepts: pages, sections, and documents. Pages are top level folders like *api-publisher* or *deal-creation-quotes* these folders contain all of the necessary files for the site to show content about that topic. Inside of each page there are sections which are the folders nested directly underneath the page folder. These sections have no content themselves but allow us to 'section' the content displayed to the user in the table of contents. Lastly there are the documents, documents are [markdown](https://en.wikipedia.org/wiki/Markdown) files (.md extension) and are dynamically rendered into HTML on the documentation site.

## Getting started

This project uses two technologies **git** and **node** to make the workflow easy.

### Using Node

If you are new to Node check out their [website](https://nodejs.org) for some information. However you won't need to code anything for this repository it is simply used for running scripts.

#### Node quickstart

- Download Node from their [website](https://nodejs.org/en/) (Get the *LTS* version not the *Current*)
- Go to your command line and run `npm install` in the fortellis-docs directory
- You are all set the scripts will run automatically!

### Using Git

If you are new to using git Atlassian has some great documentation to get you started [here](https://www.atlassian.com/git)

#### Git quickstart

Here are the commands you need to run to get your changes to work (you can also use a git GUI instead of the command line)

- `git clone ssh://git@stash.cdk.com/cexchng/fortellis-docs.git` or `git clone http://<YOUR_USERNAME>@stash.cdk.com/scm/cexchng/fortellis-docs.git` if you are not using SSH
- `git checkout -b feature/<JIRA_STORY_ID>` : create a branch based on your story so it will be tracked by Jira
- Make your changes to the documentation based on the docs below
- `git add <FILES_OR_DIRECTORIES_CHANGED>` : add your changes to the git history
- `git commit -m "<TALK_ABOUT_CHANGES_MADE>"` : commit your changes to the git history
- `git push origin feature/<JIRA_STORY_ID>` : push your changes up to Bitbucket, you can now see them online!
- click the 'Pull Request' tab on this [page](http://stash.cdk.com/projects/CEXCHNG/repos/fortellis-docs/browse)
- 'Create a pull request' using the branch you created, people will be able to review and approve your changes

## Writing Markdown

Markdown allows us to create rich text documents with a simple syntax rather than a complex GUI like a word processor. Markdown was designed for the web so it easily translates into HTML. A sample markdown file will look something like this:

```md
# Heading 1

Hello world! This is a paragraph and will be displayed as body text to the user.

## Heading 2 - A Smaller Heading

- List items
- More List items

### Heading 3 - An Even Smaller Heading

This is how you [Link](https://google.com) content
```

While we try to adhere to the latest advancements in Markdown there are a couple of rules that you need to keep in mind for the site to display your documentation properly.

- Currently our doc framework does not support the reading of HTML elements, so you can only use markdown specific syntax.
- We also ask that you never use **h1** (#) title tags and always start the heirarchy of your headings with h2. This is due to the fact that the title of your document is rendered as an h1 and should be the largest on the page.
- Our docs use a feature called 'frontmatter' which allows us to inject metadata into the document, it looks like this:

    ```yaml
    ---
    title: Doc Title
    author: Author
    last updated: 10/31/2018
    ---
    ```

    > Currently our docs *require* that frontmatter is present or it will fail to correctly read the doc. We only support the three keys listed in the example above but as the site gets more features other data may be added.

To help you and prevent unrecognizable syntax from creating visual bugs on the site this repository uses `markdownlint` to check your files for errors. This linter can be run manually to check your work with `npm run lint`, but will always be run when you commit to git. This will prevent you from commiting unless you have conformed to the linting standards of the project.

Markdown is typically written in a text editor since it is like coding, VS Code is a great editor that will allow you to get live feedback on the linting if you download a plugin called `markdownlint` and can give you a preview pane to view your markdown style. However there are also some good tools out there that will give you a more traditional word processing experience like [Typora](https://typora.io/) that is avaliable for both Windows and MacOS. If you want to work in the browser there is also a great website [Dillinger](https://dillinger.io/) where you can write markdown and save it to a file on your machine.

> If you need help with the syntax used to style markdown documents check out this handy resource: [markdown cheat sheet](https://www.markdownguide.org/cheat-sheet/)

### Custom syntax

We also support a custom syntax for referencing environment specific variables defined in the `variables.json` file:

```md
Hi, my name is $[envVariable].
```

This will replace the `$[envVariable]` with the correct dev/prod value defined with the name `envVariable`. To define an environment variable you must supply both a dev and prod value like so in `variables.json`:

```json
{
    "envVariable": {
        "dev": "Steve",
        "prod": "Samantha"
    }
}
```

## Content Structure

Documentation structure is dynamically created by the Fortellis documentation site so there are a couple of things we need to do to enable that. Each folder should have a file called `index.json`. These files will allow the scripts that build the website to enforce ordering on your sections and documents as well as give them human readable titles. The site will only publish changes that hase been defined in an index.json file so if you don't add your new content to the correct ones it won't show up.

Pages, sections, and documents `index.json` will each look a bit different so here is a cheat sheet on each:

### Pages

There is only one `index.json` for the pages and it is at the root level of this repository. Here you can add meta-data like 'label' to the pages which will give them human readable names.

```js
{
    "api-publisher": {
        "label": "API Publisher"
    },
    "deal-creation-quotes": {
        "label": "Deal Creation Quotes"
    },
    "some-folder-name": { // MUST COORESPOND TO THE FOLDER NAME
        "label": "Human Readable Name"
    }
}
```

Think of it as a dictionary where the name in "quotes" is a term and everything inside of the { curly braces } tells you about that term

### Sections

In the sections the order matters because they will be displayed in the table of contents so we will define the order we want for the documents. If we don't define this order sections will appear in alphabetical order.

```js
{
    "order": [
        {
            "route": "overview", // MUST COORESPOND TO A SECTION FOLDER NAME
            "label": "Overview"
        },
        {
            "route": "concepts",
            "label": "Core Concepts"
        }
    ]
}
```

Everything inside of the [ brackets ] is considered part of the "order" list so they must be separated by commas to be read correctly. The terms inside of the curly braces are read by the scripts and coorespond to a folder so make sure they are correct!

### Documents

Like the sections the order of the documents on the page is very important so we will define the order we want. In this case though we only need to put the name of the document because we can define things like title, author, and lastupdated by inside the file itself.

```js
{
    "order": [
        "getting-started.md", // MUST COORESPOND TO A DOCUMENT FILE NAME
        "some-other-doc.md"
    ]
}
```

If you are ever worried that your `index.json` might not be correct you can validate it using this [json validator](https://jsonlint.com/). However this will not ensure that you have all of the right fields, only that it is legal JSON. Always make sure your changes go through review before pushing them to the master branch.

## Deploying your Documentation

You should always make changes on a personal branch rather than on master. Once you are finished making changes you will need to merge your branch into master in order for the changes to appear on the site. To do this open a *'Pull Request'* on [the repository](http://stash.cdk.com/projects/CEXCHNG/repos/fortellis-docs/pull-requests). Here others can review the changes you have made and approve them before they get pushed to development. Once it has been approved you can merge your changes and manually kick-off your deployment in production.

> **NOTE**: the build processes are not finalized so currently to test the site you will need to manually run `npm run build` before you launch the site locally.