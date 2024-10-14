# RepoConfig

Please refer to https://www.chunfuchao.com/posts/unity-git-en for complete explanation and other using git with Unity information.

## Prerequisites
This config assumes:
- git
  - `git config core.ignorecase true` is set and the repo is in case-insensitive mode.  
    Recommending using `git config --global core.ignorecase true` to add to global git config.  
    Use `git config --list --show-origin --show-scope` to check your current config.  
    Allowing users push file only differ in capitalization will cause all kinds of birrard behaviors on Windows machines. 
  - git LFS installed.
- Unity
  - Force text
  - Visible meta files

## `.gitignore`
Since the repo is in case-insensitive mode, all patterns are written without matching capitalization of each character.  
A leading slash matches the beginning of the pathname like `/Library/` or `/Temp/` to only ignore `Library/` and `Temp/` at project root level.

## `.gitattribute`
Also assuming case-insensitive mode.  
Treat everything not text if not overridden by stating `* -text` at the beginning of `.gitattribute`. We do not set `* binary` since we still want to be able to diff `.prefab`, some `.asset` and `.unity` files even though we are not treating them as text files that should go through line ending conversion.  
Unity visible meta is LF regardless of created platform so we force `.meta` as LF by `*.meta eol=lf` It can fix the issue of `.meta` constantly changing its line ending on different machines.  
Set `eol=lf` to human authored code, code needs explicit EOL settings to avoid line ending flip-flopping in different editors and polluting git blame. We set EOL to LF even on Windows since we need code as part of input of our own AssetBundle source hashing and they need to have the same file content on both Windows and Mac.
Add binary files to LFS, but skip `.prefab`, `.asset` (`LightingData.asset` is certainly binary so added to LFS) and `.unity` to do text diff. Reading text diff of Unity serialized YAML assets is a very effective way to find changes in Unity assets.

## `.editorconfig`
https://editorconfig.org/  
Very basic editor config to force charset and space over tab, it will be followed by most code editors (Notable exception is Visual Studio Code which requires a plugin to support `.editorconfig`)  
Share Visual Studio or Rider config would be a more complete solution if your team can agree on which editor to use.

