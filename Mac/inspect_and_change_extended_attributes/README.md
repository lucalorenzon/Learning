# Inspect and change extended attributes to a file

- use `xattr` command:
  - `xattr -l <file>` to list extended attributes
  - `xattr -w <attribute> <value> <file>` to write an extended attribute
  - `xattr -d <attribute> <file>` to delete an extended attribute
