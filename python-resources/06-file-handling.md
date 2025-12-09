# File Handling

File handling is essential for reading, writing, and managing files in Python.

## Reading Files

### Basic File Reading

```python
# Read entire file at once
with open("data.txt", "r") as file:
    content = file.read()
    print(content)

# Read file line by line
with open("data.txt", "r") as file:
    for line in file:
        print(line.strip())  # strip() removes newline character

# Read specific number of lines
with open("data.txt", "r") as file:
    lines = file.readlines()  # Returns list of all lines
    print(lines)
```

### Reading File Modes

```python
# "r" - Read (default)
with open("file.txt", "r") as file:
    content = file.read()

# "rb" - Read binary
with open("image.jpg", "rb") as file:
    binary_data = file.read()

# "r+" - Read and write
with open("file.txt", "r+") as file:
    content = file.read()
    file.write("new content")
```

## Writing Files

### Basic File Writing

```python
# Write (overwrites existing content)
with open("output.txt", "w") as file:
    file.write("Hello, World!\n")
    file.write("This is line 2\n")

# Append (adds to end of file)
with open("output.txt", "a") as file:
    file.write("This is appended content\n")

# Write multiple lines
lines = ["Line 1\n", "Line 2\n", "Line 3\n"]
with open("output.txt", "w") as file:
    file.writelines(lines)
```

### Writing Binary Files

```python
# Write binary data
data = b"Binary content here"
with open("binary.bin", "wb") as file:
    file.write(data)

# Write and append binary
with open("binary.bin", "ab") as file:
    file.write(b"More binary data")
```

## File Positioning

### Seek and Tell

```python
with open("data.txt", "r") as file:
    # Tell current position
    print(file.tell())         # Output: 0
    
    # Read first 10 characters
    first_chars = file.read(10)
    print(file.tell())         # Output: 10
    
    # Seek to beginning
    file.seek(0)
    print(file.tell())         # Output: 0
    
    # Read from position
    content = file.read()
```

## Working with CSV Files

### Reading CSV Files

```python
import csv

# Read CSV as list of lists
with open("data.csv", "r") as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)  # Each row is a list

# Skip header
with open("data.csv", "r") as file:
    reader = csv.reader(file)
    next(reader)  # Skip header row
    for row in reader:
        print(row)
```

### Writing CSV Files

```python
import csv

# Write CSV
with open("output.csv", "w", newline="") as file:
    writer = csv.writer(file)
    writer.writerow(["Name", "Age", "City"])
    writer.writerow(["John", 30, "New York"])
    writer.writerow(["Alice", 25, "Los Angeles"])
    
    # Write multiple rows
    data = [
        ["Bob", 28, "Chicago"],
        ["Charlie", 32, "Houston"]
    ]
    writer.writerows(data)
```

### Reading CSV as Dictionaries

```python
import csv

# Reading CSV as dictionaries
with open("data.csv", "r") as file:
    reader = csv.DictReader(file)
    for row in reader:
        print(row)  # Each row is a dictionary

# Writing CSV as dictionaries
with open("output.csv", "w", newline="") as file:
    fieldnames = ["Name", "Age", "City"]
    writer = csv.DictWriter(file, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerow({"Name": "John", "Age": 30, "City": "New York"})
    writer.writerow({"Name": "Alice", "Age": 25, "City": "Los Angeles"})
```

## Working with JSON Files

### Reading JSON

```python
import json

# Reading JSON
with open("data.json", "r") as file:
    data = json.load(file)  # Parse JSON
    print(data)

# JSON string to Python object
json_string = '{"name": "Alice", "age": 25}'
data = json.loads(json_string)
print(data)
```

### Writing JSON

```python
import json

# Writing JSON
data = {
    "name": "John",
    "age": 30,
    "city": "New York",
    "hobbies": ["reading", "gaming", "coding"]
}

with open("output.json", "w") as file:
    json.dump(data, file, indent=2)  # indent for readability

# Python object to JSON string
json_string = json.dumps(data, indent=2)
print(json_string)
```

## File Path Handling

### Using os.path

```python
import os

# Join paths
file_path = os.path.join("folder", "subfolder", "file.txt")
print(file_path)               # folder/subfolder/file.txt

# Check if file exists
if os.path.exists(file_path):
    print("File exists")

# Get file info
print(os.path.getsize(file_path))      # File size in bytes
print(os.path.isfile(file_path))       # Is it a file?
print(os.path.isdir(file_path))        # Is it a directory?

# Get absolute path
abs_path = os.path.abspath(file_path)
print(abs_path)

# Get directory and filename
directory = os.path.dirname(file_path)
filename = os.path.basename(file_path)
```

### Using pathlib (Modern Approach)

```python
from pathlib import Path

# Create path
file_path = Path("folder") / "subfolder" / "file.txt"
print(file_path)

# Check if file exists
if file_path.exists():
    print("File exists")

# Get file info
print(file_path.stat().st_size)        # File size
print(file_path.is_file())             # Is it a file?
print(file_path.is_dir())              # Is it a directory?

# Get absolute path
abs_path = file_path.absolute()
print(abs_path)

# Get directory and filename
directory = file_path.parent
filename = file_path.name
print(f"Directory: {directory}, Filename: {filename}")

# Create directories
file_path.parent.mkdir(parents=True, exist_ok=True)

# Read/write with pathlib
content = file_path.read_text()
file_path.write_text("New content")
```

## Error Handling with Files

### Try-Except for File Operations

```python
# Handle file not found
try:
    with open("nonexistent.txt", "r") as file:
        content = file.read()
except FileNotFoundError:
    print("File not found!")
except IOError as e:
    print(f"IO Error: {e}")
except Exception as e:
    print(f"Unexpected error: {e}")
```

### Context Manager for Error Handling

```python
import os

file_path = "data.txt"

# Check if file exists before reading
if not os.path.exists(file_path):
    print(f"Creating {file_path}")
    with open(file_path, "w") as file:
        file.write("Initial content")

# Now read the file
with open(file_path, "r") as file:
    content = file.read()
    print(content)
```

## Reading and Processing Large Files

### Reading File in Chunks

```python
# Read file in chunks
def read_large_file(filepath, chunk_size=1024):
    with open(filepath, "rb") as file:
        while True:
            chunk = file.read(chunk_size)
            if not chunk:
                break
            yield chunk

# Process chunks
for chunk in read_large_file("large_file.bin"):
    process_chunk(chunk)  # Your processing function
```

### Processing Lines Efficiently

```python
# Process lines one at a time (memory efficient)
def process_large_file(filepath):
    with open(filepath, "r") as file:
        for line in file:
            # Process line one at a time
            yield line.strip()

# Use it
for line in process_large_file("large_file.txt"):
    print(line)
```

### Working with Large CSV Files

```python
import csv

# Process large CSV efficiently
def process_csv(filepath):
    with open(filepath, "r") as file:
        reader = csv.DictReader(file)
        for row in reader:
            # Process row one at a time
            yield row

# Use it
for row in process_csv("large_data.csv"):
    print(row)
    # Do something with each row
```

## File Operations

### Creating and Managing Directories

```python
import os
from pathlib import Path

# Create directory
os.makedirs("folder/subfolder", exist_ok=True)

# Or using pathlib
Path("folder/subfolder").mkdir(parents=True, exist_ok=True)

# List files in directory
files = os.listdir("folder")
print(files)

# Or using pathlib
files = list(Path("folder").iterdir())
print(files)

# Remove file
os.remove("file.txt")

# Or using pathlib
Path("file.txt").unlink()

# Remove directory
os.rmdir("folder")  # Only works if directory is empty

# Or using pathlib
Path("folder").rmdir()

# Remove directory and contents
import shutil
shutil.rmtree("folder")
```

### Copying and Moving Files

```python
import shutil

# Copy file
shutil.copy("source.txt", "destination.txt")

# Copy file with metadata
shutil.copy2("source.txt", "destination.txt")

# Move/rename file
shutil.move("old_name.txt", "new_name.txt")

# Copy directory
shutil.copytree("source_dir", "dest_dir")
```
