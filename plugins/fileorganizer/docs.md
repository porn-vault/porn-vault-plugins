Templates is what you use to tell fileorganizer how you want your new filenames to look like. Only the filename can be changed. The directories and the extensions are never modified.

A template is a succession of __blocks__ that each can contain:
- a `prefix` (fixed text part)
- a `field` (data from pv's scene)
- a `suffix` (fixed text part)

Blocks are delimited with `{}`. Inside blocks, fields are delimited with `<>`. All other characters are considered fixed text. 

Example of a block: 
```html
{<videoHeight>p}
```
This group has
- no prefix
- a field that references the scene's video height
- the letter "p" as suffix

For a scene with a video height of 2160 pixels, this block would output "2160p" to the renamed file.

### Blocks: key characteristics 

- Blocks are "all or nothing". If the field has a value (it is not empty), the prefix/suffix fixed text and the field value are output in the new filename. If the field value is empty, the whole block is ignored, including the fixed text.
- Each block can contain only one field. If you need more fields, use more blocks.
- The template only process a succession of blocks. Everything that is not within a block is ignored.

### Supported fields

The following scene data can be used as fields in templates:
| Field               | Comment                |
| ------------------- | ------------------- |
| `actors`            | multi-values |
| `labels`            | multi-values |
| `movies`            | multi-values |
| `name`              | |
| `rating`            | |
| `releaseDate`       | see args below to specify the date format |
| `studio`            | |
| `videoDuration`     | HH:MM or HH:MM:SS depending on the duration |
| `videoHeight`       | |
| `videoWidth`        | |

### Fields: key characteristics 
- Field names are __not__ case sensitive.
- You can indicate you consider a field to be mandatory by adding a `'!'` after the field name like this: `<field!>`. When a mandatory field returns no data, the whole rename template is ignored. In other words a mandatory field means "I'd rather not rename this file at all if the field is not present in porn-vault's data".
- For multi-value fields, the default behavior is to output all values into the new filename. It is possible to use only one of the values, by specifying its index next to the field name. 

Examples: 
- `<movies>` will insert all movies into the file name. 
- `<movies1>` will insert only the first movie into the file name (or none if there are no movies defined at all).

### Full template example
```
{<studio!>}{ - <releaseDate>}{ - <actors>}{ - <name!>}{ - <movie1>}{ (<videoHeight>p)}
``` 

Which will generate file names like:
- `Studio - 2019-03-27 - Best scene ever (720p).mp4'
- `Studio - Eva Evil, John Doe - Best scene ever (2160p).mp4'
- `Studio - Best scene ever (1080p).mp4'
- `Studio - Scene 1 - Best movie ever (540p).mp4'

depending on what data exist for each scene.

### file name constraints

In filenames, there are some illegal and reserved characters like `"`, `/`, `*`, `<`, `?`, `>`, `:` and `|`.  By default, characters that are invalid in a filename are removed. 

Alternatively, you can use the `characterReplacement` argument to indicate a list of replacement characters you would like to substitute in the filename. 

For instance, if you want to replace all spaces by underscores, you can use (in JSON format):
```json
characterReplacement: [ { original: " ", replacement: "_" } ]
```
There is also a limitation to the filename's length (255 characters). You will get a warning that the file rename was not performed when the new name exceeds 255 characters.