
# Link local libraries into other repositories

## How? 

1. In the library that you want to use in the other repository run  `npm link`
   This will 'install' the package globally with a symlink pointing to the source code instead of having a fixed version from [[npm]]
2. Then, go to the repository that wants to use that library and run `npm link <package-name>`
   That will tell the repository to use the global symlink instead of the real package from [[npm]] or whatever package registry.
## Where?
Run `npm prefix -g` to get the location of the node installation 

Modules are stored in: `{prefix}/lib/node_modules/<package-name>`
Binaries are store in: `{prefix}/bin/<bin-name>`

>[!warning] Watch out  the node version
>Make sure that the node version is the same in both projects!
> The package global installation lies inside a specific node version. The one that was running when he `npm link` command is executed.
> If the projects are running with 2 different node versions the package won't be found!

## Undo 
1. Run `npm unlink <package-name> -g`
   This will  remove the global symlink and the repository should use the package registry again
2. You can also go the the global *node_modules* and remove the folder yourself

## Source

https://medium.com/dailyjs/how-to-use-npm-link-7375b6219557

