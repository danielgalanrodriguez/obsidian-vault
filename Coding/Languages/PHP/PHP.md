
A **popular general-purpose scripting language** that is especially suited to web development.  
Fast, flexible and pragmatic, PHP powers everything from your blog to the most popular websites in the world.

# Install php with [[homebrew]]

1. Download php
	- `brew install php@<version>`
2. Link php
	- `brew link --force --overwrite php@<version>`

## Example
`<version>` = 8.4

```zsh
brew install php@8.4
```

```bash
brew unlink php@8.4 # Only if you have a version already installed
brew link --force --overwrite php@8.4
```

# Version roadmap
https://www.php.net/supported-versions.php

# Change memory limits

- locate the php.ini file (execute: phpinfo();)
- change 
```php
; Maximum allowed size for uploaded files.
upload_max_filesize = 40M

; Must be greater than or equal to upload_max_filesize
post_max_size = 40M

```


# Install extensions
