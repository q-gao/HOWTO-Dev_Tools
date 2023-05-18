# Process Files



`-n 1` option to xargs https://unix.stackexchange.com/questions/304692/piping-list-of-files-and-directories-to-du-only-shows-sizes-of-directories

## `find`
Get the total size of a list of files: 'c' passed to du = accumulation?

```sh
# +: all files in the same command execution
find ./photos/john_doe -type f -name '*.jpg' -exec du -ch {} + | grep total$
# \; one file per command execution
find ./photos/john_doe -type f -name '*.jpg' -exec du -ch {} \; | grep total$
find . -name '*_lq.jpg' -exec mv {} ../Full_LQ \;
```

## Get File Info
Get the sizes of a list of files:
```sh
# -c : print the total size (cumulative)
du -ch *.jpg
```

## Zip files
`pip install py7zr`

```python
import os
import py7zr

def find_and_zip_files(path, suffixes, archive_name, password):
    files_to_zip = []

    # Walk through the directory
    for root, dirs, files in os.walk(path):
        for file in files:
            # If the file suffix is in the suffixes list, add it to the files_to_zip list
            if any(file.endswith(suffix) for suffix in suffixes):
                files_to_zip.append(os.path.join(root, file))
    
    # Create a 7z archive with password protection
    with py7zr.SevenZipFile(archive_name, 'w', password=password) as z:
        for file in files_to_zip:
            z.write(file)

    print(f"Created a password protected 7z archive '{archive_name}' containing {len(files_to_zip)} files.")

# Example usage
find_and_zip_files('/path/to/your/directory', ['.txt', '.docx'], 'archive.7z', 'yourpassword')
```

```python
import py7zr

archive_path = '/path/to/archive.7z'  # Replace with the path to your 7z archive
extract_dir = '/path/to/extract'     # Replace with the path where you want to extract the archive

with py7zr.SevenZipFile(archive_path, mode='r') as z:
    z.extractall(path=extract_dir)
```
