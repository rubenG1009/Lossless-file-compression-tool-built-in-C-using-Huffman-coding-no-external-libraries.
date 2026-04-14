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
