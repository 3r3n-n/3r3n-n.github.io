---
layout: post
title: "How does Nuclear import a playlist ?"
---
Nuclear is a pretty cool app. It is a desktop music player focused on streaming from free sources... and it's open-sourced !

Nuclear allows you to import a playlist from a JSON file. 

We can get to the playlists view from the main menu on the left panel:

<img src="/img/nuclear_screen.jpg" width="514" height="382">

Here we see an option at the header: *import from file (JSON)*. If we click it, it shows us a file browser dialog to select our playlist JSON file.

But how does this work ?...

If we go to the source code, we will find that this button is located in the `PlaylistsHeader` component, which is located in the `Playlists` component, which is located in the `PlaylistsContainer` component. The action that is associated with the `onClick` prop for this button is `handleImportFromFile()`. This function is passed (as part of an object) as props from `PlaylistsContainer` and de-structured in `Playlists`. This de-structuring is done because `handleImportFromFile()` is defined and returned from another function `usePlaylistsProps()` so it must be separated from the rest of the returned outputs.  

Here is a small diagram in case the previous paragraph was too cluttered:

<img src="/img/nuclear_props.jpg" width="575" height=“641">

So `handleImportFromFile` is defined in this way:

```jsx
const handleImportFromFile = useCallback(async () => {
    const filePath = await openLocalFilePicker();
    dispatch(PlaylistActions.addPlaylistFromFile(filePath[0], t));
  }, [dispatch, t]);
```

We can see that `handleImportFromFile` uses two other functions: `openLocalFilePicker` and `addPlaylistFromFile`. These functions are stored in the actions folder. 

The `openLocalFilePicker` function uses `remote.dialog.showOpenDialog()`, this function is imported from electron and opens a dialog for browsing files, it returns the selected file's path. 

After the file path has been selected, the `addPlaylistFromFile` function is called. It takes the file path as input and uses the `fs.readFile` function from the node.js library. Its inputs are the file path and an anonymous function that handles the actions if there is an error while reading the file and parses the content of the file into JSON format. Then the `PlaylistHelper.formatPlaylistForStorage` function is called (this function is imported from the nuclear *core* library folder) and the returned `playlist` object is validated (it must have at least one track). If the `playlist` object passes the validation, the app state is updated so the `playlists` dataset in `store` includes the new `playlist` object (the formatted imported playlist). 

I am surprised that this explanation ended up being a short one because I spent the whole afternoon trying to understand what was going on.
