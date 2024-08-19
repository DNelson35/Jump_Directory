# JumpDir CLI Tool

`JumpDir` is a command-line tool written in Go that helps you find directories by name. It performs a depth-first search starting from a specified directory and returns the full path to the target directory.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Zsh Integration](#zsh-integration)
- [Help and Flags](#help-and-flags)
- [Examples](#examples)
- [Troubleshooting](#troubleshooting)
- [Alpha Release Information](#alpha-release-information)

## Installation

You can install `JumpDir` either by downloading the latest release or by building it from source.

### Option 1: Install from Release

1. **Download the Latest Release**

   Download the `install.sh` script from the [releases page](https://github.com/DNelson35/JumpDir/releases) of this repository.

2. **Make the Installation Script Executable**

   ```sh
   chmod +x install.sh
   ```

3. **Run the Installation Script**

   ```sh
   ./install.sh
   ```

   The script will download the latest `jumpdir` binary, make it executable, and configure your shell to use the `jd` function.

4. **Restart Your Terminal**

   Restart your terminal or run the following command to apply the changes:

   ```sh
   source ~/.zshrc
   ```

### Option 2: Build from Source

1. **Clone the Repository**

   ```sh
   git clone https://github.com/DNelson35/JumpDir.git
   cd JumpDir
   ```

2. **Ensure Go Version 1.22.4 or Later**

   Verify that you have Go version 1.22.4 or later installed:

   ```sh
   go version
   ```

3. **Build the Binary**

   Build the `jumpdir` binary by running:

   ```sh
   go build -o jumpdir-bin/jumpdir
   ```

4. **Set Up the Zsh Shell**

   Add the following function to your `.zshrc` file:

   ```sh
   function jd() {
     local target_dir="$1"
     local start_dir="$2"

     if [[ "$target_dir" == "-help" || "$target_dir" == "--help" ]]; then
       ./jumpdir-bin/jumpdir -help
       return 0
     fi
     if [[ -z "$target_dir" ]]; then
       echo "Error: target_directory is required."
       return 1
     fi
     
     if [[ -z "$start_dir" ]]; then
       start_dir="/"
     elif [[ "$start_dir" == "." ]]; then
       start_dir="$(pwd)"
     fi

     local dir=$(./jumpdir-bin/jumpdir $target_dir $start_dir)
     
     if [[ -z "$dir" ]]; then
       echo "Directory not found."
       return 1
     fi

     cd "$dir" || return 1
   }
   ```

5. **Reload Your Zsh Configuration**

   ```sh
   source ~/.zshrc
   ```

## Usage

The `JumpDir` tool requires one argument and optionally a second argument:

- `<target_directory>`: The name of the directory you want to find (required).
- `<starting_point>`: The directory where the search should start (optional). If omitted, the search will start from the root directory. If you pass `.` as the `<starting_point>`, the search will start from your current directory.

### Command Syntax

```sh
jd <target_directory> [<starting_point>]
```

### Example

```sh
jd "targetDirName" /path/to/start
```

This command will search for "targetDirName" starting from `/path/to/start`.

If you want to search starting from your current directory, use:

```sh
jd "targetDirName" .
```

This command will search for "targetDirName" starting from your current location in the file system.

## Zsh Integration

The `install.sh` script configures the `jd` function for you. If building from source, ensure the function is added to your `.zshrc` file as described above.

### Function Overview

The `jd` function:

- Searches for the specified directory.
- Changes to the found directory if it exists.
- Handles missing arguments gracefully and provides help if requested.

## Help and Flags

To display help information for the `JumpDir` tool, use the `-help` or `-h` flags:

```sh
jd -help
```

### Output

```
Usage: go run main.go <target_directory> [<starting_point>]

Arguments:
  <target_directory>    Target directory (required)
  [<starting_point>]    Starting point (optional)

Flags:
  -h, -help   Show help information
```

## Examples

1. **Find a Directory Starting from the Root Directory**

   ```sh
   jd Documents
   ```

   This searches for a directory named `Documents` starting from the root directory.

2. **Find a Directory Starting from a Specific Directory**

   ```sh
   jd myProject workspace
   ```

   This searches for "myProject" starting from `~/workspace`.

3. **Find a Directory Starting from the Current Directory**

   ```sh
   jd myProject .
   ```

   This searches for "myProject" starting from your current location in the file system.

## Troubleshooting

- **Error: target_directory is required**: Ensure that the `target_directory` argument is provided. The `starting_point` argument is optional.
- **Permission Denied**: Verify that `JumpDir` is executable and properly installed.
- **Function Not Found**: Ensure that the `.zshrc` file has been updated and reloaded.
- **Directory Not Found**: Confirm that the directory you are searching for exists.

## Alpha Release Information

Please note that `JumpDir` is currently in alpha release and is still being tested. Your feedback is highly appreciated as it will help improve the tool. 

### Known Limitations

- **Ignored Files**: The current implementation includes a list of specific files and directories to ignore (e.g., `node_modules`) to speed up the search. This list is located in `search.go`.
- **Permission Issues**: The tool does not yet handle directories that require special permissions to read. This may be addressed in future updates as it’s not considered critical for the tool's intended purpose.

Thank you for your understanding and support!