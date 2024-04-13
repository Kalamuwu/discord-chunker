# discord-chunker

Got a large file you wanna share, but don't have Discord Nitro?
discord-chunker is a simple python script that splits uploads into 25mb chunks, and then reassembles them on the other side.

# Usage

### Encoding

Encoding a file to chunks:
`python3 chunker.py encode <infile> <outdir>`

This will instruct the script to take file `infile` (or all files in folder, if `infile` is a directory) and encode it, spitting out the header and associated chunks in directory `outdir`.

### Decoding

Decoding chunks back to the original file(s):
`python3 chunker.py decode <indir> [-o outdir]`

1. Download all the chunks from a discord channel or otherwise, as well as the `header` file, and place them all in a new, empty directory. Do not rename any of the chunks.
2. Navigate up one level, and run the `decode` command, with `indir` as the folder you just made.
3. Optionally, specify an output directory with `-o`. By default, the output(s) will be placed in the cwd.

# How?

First, the files are specified to the program. An example is shown with the files below.
```
_A__B_____________C_____D_________E________F____
|  |             |     |         |        |    |
|  |             |     |         |        |    |
|  |             |     |         |        |    |
|  |             |     |         |        |    |
|__|_____________|_____|_________|________|____|
```
These files are then glued end-to-end, with a `header` file keeping track of the original file lengths:
```
_0__________1__________2__________3__________4__
|          |          |          |          |  |
|  |       |     |    ||         |        | |  |
|  A       |     B    |C         D        E |  F
|  |       |     |    ||         |        | |  |
|__________|__________|__________|__________|__|
```
These chunks are all 25mb in length (or, if specified, some other predetermined length) for uploading to Discord.

Then, on the other side, download all the files. The program will then glue them all back together, and cut appropriately, based on the generated `header` file:
```
_A__B_____________C_____D_________E________F____
|  |             |     |         |        |    |
|  |             |     |         |        |    |
|  |             |     |         |        |    |
|  |             |     |         |        |    |
|__|_____________|_____|_________|________|____|
```
Voilà.
