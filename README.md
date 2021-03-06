# AudioBook Compiler (abc)

This script will compile a directory of MP3 files into an .mb4 audiobook.

## Setup

You will start with a folder containing the separate audio files that you want to convert into an audiobook. Files should be named so that they can be sorted by title. So you would want to have your files named something like this:

```
00-Introduction.mp3
01-Chapter 1.mp3
02-Chapter 2.mp3
03-Chapter 3.mp3
99-Outro.mp3
```

Also, add a cover image into this directory.

Create a file named `meta.yaml` that will contain the metadata for the audiobook. It should look like this:

```yaml
title: The Greatest Audiobook
author: Author B. Name
cover: cover.jpg  # enter the name of your cover image
track_title_metadata: title  # Optional - Sets the chapter title as the file's metadata title
track_title_regex: ^\d{3}-(.+)$  # Optional - Regex to modify the chapter title based on the filename (see below)
```

### Track (Chapter) Titles

There are a few options for specifying track titles.

By default, the track title will be the name of the file with the extension removed.

If you specify `track_title_metadata`, the script will read the metadata on the file and use use the field you specify. Most common would be to use the `title` field, but if you have a compilation of speakers at a conference for example, you may want to specify the `artist` field.

If you specify `track_title_regex`, you will need to have a matching group. The script will use the first match group as the title. File extenions are automatically removed. For example, let's sayin my files are formatted with title's like `01 - Chapter 1.m4a` and `02 - Chapter 2.m4a`, etc. If you specified a regex value of `^\d{2} - (.+)`, the resulting titles would be `Chapter 1` and `Chapter 2`.

## Conversion

There are three parts to creating your audiobook:

1. Covert: convert audio files to MPEG-4 (`.m4a`). This script will also covert them to 24000 Hz, 64k bitrate and mono.
2. Build: generates two files name `FILES` and `METADATA`. Files is the list of files that will be combined. METADATA contains the title, author, and chapter bookmark data.
3. Compile: This is the final step that will convert use the above generated files to create your `.m4b` audiobook.

You can run each step individually or run them all at once. To run them all at once you would call:

```sh
./abc -abc "AudioBook MP3 Directory Name"
```

Or, you can run them one step at a time:

```sh
./abc -a "AudioBook MP3s"
./abc -b converted
./abc -c converted
```

The reason you might want to run each step independently is perhaps you might want to edit the titles in the `METADATA` file for example. By default, the title will be the name of the file minus the extension.

## Usage

```shell
./abc <option[s]> <directory>
```

Options | Description
--- | ---
| `-a`, `--convert` | Convert audio files into MPEG-4 (.m4a) format. A directory named `converted` will be created to store these. |
| `-b`, `--build` | Builds the `FILES` list and the `METADATA` files. These will be located in the directory you specify. |
| `-c`, `--convert` | Uses the `FILES` list and `METADATA` file to build your m4b audiobook. |

## Requirements

This script makes use of some nice open-source tools. You will need the following:

1. Python3
2. Python packages listed in `requirements.txt`
3. ffmpeg
4. [mp4v2](https://code.google.com/archive/p/mp4v2/)

## Todo:

1. Make available in brew
  - https://jimkubicek.com/creating-a-homebrew-formula-for-a-python-project.html
  - https://docs.brew.sh/Python-for-Formula-Authors
