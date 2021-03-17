# npm workspaces: auto switch context based on cwd

## Summary

Running **workspace-aware** commands from the context of a **workspace** folder should work as expected.

## Motivation

Most users expect to be able to navigate to a **workspace** directory and use npm commands in that context.

Unfortunately that might not work as intended depending on the command, for example a user trying to add a new dependency `<pkg>` would expect to `cd ./<workspace-folder>` and `npm install <pkg>` to produce a valid install tree adding the new dependency to that workspace but instead that creates a separate install within that directory, failing to hoist/dedupe dependencies to the top-level and generating an extra and unintended package-lock file.

## Detailed Explanation

Now that we are landing support to **workspace-aware** commands (see: [#117](https://github.com/npm/rfcs/pull/117)) a potential fix to that mismatch between the expected user experience and the current behavior of the npm cli is to automatically switch the internal context to ensure that if the user is trying to run a command that already support the **workspaces configurations**.

## Rationale and Alternatives

- TBD

## Implementation

Make a recursive check that walks up the file system structure and validates if the current `prefix` is defined as the **"workspaces"** property of a `package.json` file of a parent directory.

In case that is so, we can then tweak the internal contexts/variables in such a way that runs the command in the context of the **top-level workspace** while defining the current working directory as the **workspace configuration** to be used. Making it so that these two lines are both functional equivalent:

```
npm install <pkg> -w <workspace-name>
cd ./<workspace-path> && npm install <pkg>
```

## Prior Art

- TBD

## Unresolved Questions and Bikeshedding

- TBD
