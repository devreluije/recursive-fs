# Recursive-fs

Easily access your files in any folder or sub-folder.

## Objective

It is often difficult to retrieve and use files stored locally at your application, `fs-recursive` allows you to retrieve any file saved in a folder or sub-folder at a defined location.

## How to use

Install the module in your project via YARN

```bash
yarn add fs-recursive
```

Or NPM

```bash
npm install fs-recursive
```

The `fs-recursive` is very simple to use, you need to use the followed function :

```ts
fetch(
  directory: string,
  extentions: Array<string>,
  encode?: Buffer | string | undefined,
  callback?: (file: File) => void
): Promise<Map<string, File>>

fetchExpression (
	path: string,
	filenamePattern: RegExp,
	encode: Encode,
	excludes?: Array<string>,
	callback?: (file: File) => void
): Promise<Map<string, File>>

fetchSortedExpression (
	path: string,
	filenamePattern: RegExp,
	sortedExtensions: Array<string>,
	encode: Encode,
	excludes?: Array<string>,
	callback?: (file: File) => void
): Promise<Array<File>>
```

### An example is always more telling

```ts
import { fetch } from 'fs-recursive';

const directory = path.join(process.cwd(), 'folder');
const extensions = ['ts', 'json'];

const files = await fetch(directory, extensions, 'utf-8'); // return Map<string, File>
console.log(files.entries().next().value[1]);

/**
 * Log the followed result
 *
 * File {
 *   path: 'E:\\WindowsData\\Desktop\\folder`\\File.ts',
 *   filename: 'File',
 *   extension: 'ts',
 *   size: 1513   👈 expredded in bytes
 * }
 */
```

You can get the same result using `RegExp`

```ts
import { fetchExpression } from 'fs-recursive';

const directory = path.join(process.cwd(), 'folder');
const expression = /.*\.ts$/;

const files = await fetchExpression(directory, expression, 'utf-8'); // return Map<string, File>
console.log(files.entries().next().value[1]);

/**
 * Log the followed result
 *
 * File {
 *   path: 'E:\\WindowsData\\Desktop\\folder`\\File.ts',
 *   filename: 'File',
 *   extension: 'ts',
 *   size: 1513   👈 expredded in bytes
 * }
 */
```

Then you can access to other data like this

```ts
const files = await fetch(directory, extensions, 'utf-8'); // return Map<string, File>
const file = files.entries().next().value[1];
const stats = await file.getStats();

console.log(stats);
/**
 * Stats {
 *   dev: 310369320,
 *   mode: 33206,
 *   nlink: 1,
 *   uid: 0,
 *   gid: 0,
 *   rdev: 0,
 *   blksize: 4096,
 *   ino: 562949956735823,
 *   size: 72,
 *   blocks: 0,
 *   atimeMs: 1618523067313.032,
 *   mtimeMs: 1618521064609.6714,
 *   ctimeMs: 1618521064609.6714,
 *   birthtimeMs: 1618521033264.188,
 *   atime: 2021-04-15T21:44:27.313Z,
 *   mtime: 2021-04-15T21:11:04.610Z,
 *   ctime: 2021-04-15T21:11:04.610Z,
 *   birthtime: 2021-04-15T21:10:33.264Z
 * }
 */
```

You can sort results by their extension

```ts
import { fetchSortedExpression } from 'fs-recursive';

const extensions = ['ts', 'js'];
const expression = /.*\.(ts|js)$/;
const files = await fetchSortedExpression(
	process.cwd(),
	expression,
	extensions,
	'utf-8'
);

console.log(files);

/**
 * Log the followed result
 *
 * [
 * 	File {
 *   path: 'E:\\WindowsData\\Desktop\\folder`\\File.ts',
 *   filename: 'File',
 *   extension: 'ts',
 *   size: 1513   👈 expredded in bytes
 *	},
 * 	File {
 *   path: 'E:\\WindowsData\\Desktop\\folder`\\File.js',
 *   filename: 'File',
 *   extension: 'js',
 *   size: 1513   👈 expredded in bytes
 * 	}
 * ]
 */
```
