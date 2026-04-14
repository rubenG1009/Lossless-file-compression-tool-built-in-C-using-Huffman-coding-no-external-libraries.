# huffman — Lossless Text Compression in C
 
A command-line tool that compresses and decompresses any text file using **Huffman coding** — a classic lossless compression algorithm built entirely from scratch in pure C with no external libraries.
 
Built as a portfolio project to demonstrate data structures, bitwise I/O, and systems programming in C.
 
---
 
## How it works
 
Huffman coding assigns shorter bit-sequences to frequent characters and longer ones to rare characters. The result is a compact binary file that can be decoded back to the exact original.
 
```
"hello world"
  l → 10          (appears 3×, gets the shortest code)
  o → 110         (appears 2×)
  h → 0110        (appears 1×, gets a longer code)
  ...
```
 
**Pipeline:**
 
```
Input file
    │
    ▼
1. Count character frequencies   →  freq[256] array
    │
    ▼
2. Build Huffman tree            →  min-heap merging
    │
    ▼
3. Generate prefix codes         →  code table (e.g. 'e' = "10")
    │
    ├──▶  4. Encode  →  pack bits into bytes  →  .huff file
    │
    └──▶  5. Decode  →  walk tree per bit     →  original file
```
 
---
 
## Usage
 
**Compile**
```bash
gcc huffman.c -o huffman
```
 
**Compress**
```bash
./huffman compress input.txt output.huff
```
 
**Decompress**
```bash
./huffman decompress output.huff decoded.txt
```
 
**Example output**
```
  Char       ASCII    Huffman code
  ---------- -------  --------------------
  ' '        32       000
  'd'        100      001
  'e'        101      010
  'h'        104      0110
  'l'        108      10
  'o'        111      110
  'r'        114      0111
  'w'        119      1110
 
  Input  : input.txt   (1 048 576 bytes)
  Output : output.huff (  601 423 bytes)
  Ratio  : 42.7%
```
 
---
 
## File format
 
The `.huff` file has a simple three-part layout:
 
| Section | Size | Description |
|---------|------|-------------|
| Header | 1 024 bytes | 256 × 4-byte frequency table to rebuild the tree |
| Data | variable | Huffman-encoded bits packed into bytes |
| Padding byte | 1 byte | Number of zero-pad bits added to the last byte |
 
---
 
## Implementation details
 
| Component | Description |
|-----------|-------------|
| `MinHeap` | Custom min-heap (priority queue) — always pops the lowest-frequency node |
| `Node` | Binary tree node — leaves hold characters, internals hold merged frequencies |
| `Code` | Bit-string representation of each character's Huffman code |
| `write_header` / `read_header` | Serialize and restore the frequency table for the decoder |
| `encode_file` | Packs variable-length bit codes into bytes, tracks padding |
| `decode_file` | Reads bits one by one, walks the tree, emits characters at each leaf |
 
---
 
## Concepts demonstrated
 
- **Data structures:** binary trees, min-heap / priority queue
- **Algorithms:** greedy Huffman coding, tree traversal (DFS)
- **Systems programming:** binary file I/O (`fread`/`fwrite`), bitwise operations
- **Memory management:** manual `malloc` / `free`, recursive tree cleanup
- **C fundamentals:** pointers, structs, function pointers, `typedef`
 
---
 
## Limitations
 
- Designed for text files. Works on any binary file but compression ratio varies.
- The 1 024-byte header means very small files may end up slightly larger after compression — this is expected and normal for any Huffman implementation.
- No multi-threading or streaming support (intentional — kept simple for clarity).
 
---
 
## License
 
MIT — free to use, modify, and distribute.
