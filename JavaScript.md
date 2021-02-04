# JavaScript

## Path

### Importing Path

```javascript
const path = require('path')
```

### Create Path

- `__dirname` provides the path of the directory where the current file resides.
-  It provides different OS support.

```javascript
const newPath = path.resolve(__dirname, "folder", "filename.txt")
```

## File System

### Importing `fs`

```javascript
const fs = require('fs') // Simple fs
const fs = require('fs-extra') // More function fs
```

### Removing directory

- It also removes all it’s sub folders and sub files.
- It is a new function of `fs-extra`.

```javascript
fs.removeSync(newPath)
```

### Reading File

```javascript
const fileContents = fs.readFileSync(newPath, 'utf8')
```

### Ensuring Directory

- Create a directory if it not exists.

```javascript
fs.ensureDirSync(folderPath);
```

### Writing File

```javascript

```

