# Convert Symlink to Target File

The `cp` command on macOS does not support the `--remove-destination` flag, therefore the following command is not working:

```
# Not working on macOS
cp --remove-destination "$(readlink <symlink>)" <symlink>
```

Instead, this simple bash script can be used to replace the symlink with a copy of the target file. With this script the file will have the name of the link. When placing the script in the PATH, e.g. by placing it in the `/usr/local/bin` directory, in can be called like `unln <symlink>`

<details open>

<summary>unln</summary>

```bash
#!/usr/bin/env bash

# FILE is the symlink that is pointing to some other file
FILE=$1
BACKUP=$1".bak"
TARGET=$(readlink "$FILE")

if [ -e "$TARGET" ]
then
    mv "$FILE" "$BACKUP" && cp "$TARGET" "$FILE" || echo "ERROR: Unable to change $FILE to $TARGET"

    if [ -f "$FILE" ]
    then
        rm "$BACKUP"
        echo "Replaced symlink $FILE with copy of $TARGET"
    else
        echo "ERROR: unln failed."
    fi
else
    echo "ERROR: Broken symlink"
fi
```

</details>
