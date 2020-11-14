# "Commits" semanticos

Good commits can maje the difference between a well-maintained product, and a terrible product. Well-written commits following a standardized format will enable viewers of your codebase to easily understand the type of change, what modules it affected, and why it occurred.

Many people dont put importance on commit messages when they are working on individual projects. But commits become **especially** important when you're part of a team.

When you enter a new repository, you want to be able to see how the project has developed over time. If you were to see commits like this:

```
Fix the thing

Update styles

Add the thing
```

You likely won't be able to understand what happened, without reading the code changes in a commit itself.

Here in AmicasaIO we make semantic commits. Compare in contrast the following:

```
fix(blog): Imported posts formatted correctly with new styles.

feat(blog): Update styles to reflect new design.

feat(blog): Add blog feed to site.
```

This examples are better understood.

The semantic commit contains 5 parts:

- The type of commit.
- The Scope of the commit.
- The actual content of the commit aka body.
- An optional body, for more description. Good for larger commits.
- A footer for referencing closed issues or breaking changes.

Format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

Here is how it works:

## Type

Here are a list of accepted types for amicasa:

- **feat**: a new feature, or change to existing feature.
- **fix**: Fixing a bug or known issue in code.
- **test**: Adding additional tests for existing features.
- **chore**: Updating build tools, like webpack, gulp, ascripts, etc.
- **docs**: Updating documentation like README.
- **refactor**: Refactoring production code.
- **update**: Update of a package.
- **other**: For all the other categories. If this is done, we motivate the programmer to propone if they think it is necessary to have other category.

## Scope

Your scope can be as granular as you'd like and will likely change based on the complexity of the project. If you're just starting off a project, you could omit the scope, but I highly recommend you consider it because it forces you to think very clearly about what you're changing.

## Message body

You want to summarize your commit with a single line. If you cant't, then you probably want to consider breaking the commit down into smaller pieces to make the work logical and self-contained.

- uses the imperative, present tense: "change" not "changed" nor "changes"
- includes motivation for the change and contrasts with previous behavior.

## The (optional) body

A single line is good for the summary, but sometimes you want to add additionsl detail so that readers can see more about what you changed now that they know why you changed it.

## Message footer (optional)

This optional part serves for 2 main things:

- Referencing issues

  - Closed issues should be listed on a separate line in the footer prefiked with "closes" keyword like this:
    `Closes #234`
  - Or in the case of multiple issues:
    `Closes #123 #245 #99`

- Breaking changes

  - All breaking changes have to be mentioned in footer with the description of the hcange, justification and migration notes.

  ```
  `port-runner` command line option has changet to `runner-port`, so that it is consistent with the configuration file syntax

  To migrate your project, change all the commands, where you use `--port-runner` to `--runner-port`
  ```
