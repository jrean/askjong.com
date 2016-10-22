title: Ignore and Untrack Files with Git
tags: [git, command-line]
---

## Ignoring Files and Directories

Inside your project root directory, where your repository has been initialized,
create a file called `.gitignore` or edit a pre-existing `.gitignore` file.
Within this file you can add file paths or directory paths that you don't want git
to track.

If you are lazy and don't want to run `vim` to edit the file you can run the
following command:

```
echo ":path_to_ignore" >> .gitignore
```

It will prepend `:path_to_ignore` to the existing `.gitignore` content.

## Untrack Files and Directories

Sometimes you may want to stop tracking.

- Edit your `.gitignore` file as mentioned above.
- Untrack

```
git rm --cached :path_to_untrack
```

Where `:path_to_untrack` is the path of the file or directory you want to
untrack.

- Commit

Once you have told git to stop tracking the file, you must commit.

```
git add .gitignore
git commit -m "Ignoring..."
```
