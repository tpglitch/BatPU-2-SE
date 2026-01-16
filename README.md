# BatPU-2-SE (Speedy Engine)

A Rust library providing programmatic access to BatPU-2 assembly compilation and Minecraft schematic generation.

## Overview

BatPU-2-SE is the core engine extracted from BatPU-2-Speedy, allowing you to integrate BatPU-2 assembly compilation and schematic generation into your own Rust projects.

## Features

- **Assembly Compilation**: Convert BatPU-2 assembly code to machine code
- **Schematic Generation**: Create Minecraft schematics from machine code
- **Modern & Legacy Syntax**: Full support for both syntax styles
- **Programmatic API**: Use as a library in your own projects

## Installation

Add to your `Cargo.toml`:

```toml
[dependencies]
batpu2-se = { git = "https://github.com/tpglitch/BatPU-2-SE" }
```

## Usage

### Assemble Assembly to Machine Code

```rust
use batpu2_se::assemble;

fn main() -> Result<(), String> {
    // Assemble an assembly file to machine code
    assemble("program.s", "program.mc")?;
    println!("Assembly complete!");
    Ok(())
}
```

### Generate Minecraft Schematic

```rust
use batpu2_se::make_schematic;

fn main() -> Result<(), String> {
    // Generate a schematic from machine code
    make_schematic("program.mc", "program.schem")?;
    println!("Schematic generated!");
    Ok(())
}
```

### Complete Build Pipeline

```rust
use batpu2_se::{assemble, make_schematic};

fn main() -> Result<(), String> {
    // Compile assembly
    assemble("program.s", "program.mc")?;

    // Generate schematic
    make_schematic("program.mc", "program.schem")?;

    // Optionally clean up intermediate file
    std::fs::remove_file("program.mc").ok();

    println!("Build complete!");
    Ok(())
}
```

### Working with Strings

```rust
use batpu2_se::assemble;
use std::fs;

fn main() -> Result<(), String> {
    // Generate assembly code programmatically
    let assembly = r#"
        ; Hello World
        main:
            ldi r1, 10
            ldi r2, 20
            add r1, r2, r3
            hlt
    "#;

    // Write to file
    fs::write("temp.s", assembly).map_err(|e| e.to_string())?;

    // Assemble
    assemble("temp.s", "output.mc")?;

    Ok(())
}
```

## API Reference

### Functions

#### `assemble(input: &str, output: &str) -> Result<(), String>`

Compiles BatPU-2 assembly code to machine code.

**Parameters:**

- `input`: Path to the input assembly file (`.s`, `.asm`, etc.)
- `output`: Path to the output machine code file (`.mc`)

**Returns:**

- `Ok(())` on success
- `Err(String)` with error message on failure

**Example:**

```rust
assemble("program.s", "program.mc")?;
```

#### `make_schematic(input: &str, output: &str) -> Result<(), String>`

Generates a Minecraft schematic from machine code.

**Parameters:**

- `input`: Path to the input machine code file (`.mc`)
- `output`: Path to the output schematic file (`.schem`)

**Returns:**

- `Ok(())` on success
- `Err(String)` with error message on failure

**Example:**

```rust
make_schematic("program.mc", "program.schem")?;
```

#### `create_symbol_table() -> HashMap<String, i32>`

Creates a symbol table with all BatPU-2 opcodes, registers, conditions, and I/O ports.

**Returns:**

- `HashMap<String, i32>` mapping symbol names to their numeric values

**Example:**

```rust
use batpu2_se::create_symbol_table;

let symbols = create_symbol_table();
let add_opcode = symbols.get("add").unwrap(); // 2
let r1 = symbols.get("r1").unwrap();           // 1
```

## Assembly Syntax

BatPU-2-SE supports both modern and legacy assembly syntax:

### Modern Syntax

```assembly
; Comments with semicolons
main:
    ldi r1, 10
    add r1, r2, r3
    hlt

data:
    .db 1, 2, 3, 4
    .ascii "hello"
```

### Legacy Syntax

```assembly
// Comments with slashes
.main
    LDI r1 10
    ADD r1 r2 r3
    HLT
```

## Modules

### `assembler`

Handles assembly compilation with preprocessing, parsing, and code generation.

### `schematic`

Generates Minecraft Sponge Schematic format with NBT encoding and gzip compression.

### `symbols`

Provides symbol tables for opcodes, registers, conditions, and I/O ports.

## Architecture Support

Targets the BatPU-2 redstone computer architecture:

- 16 registers (r0-r15, r0 hardwired to zero)
- 256 bytes of data memory
- 2048 bytes of instruction memory
- Memory-mapped I/O (addresses 240-255)

## Error Handling

All functions return `Result<(), String>` for easy error propagation:

```rust
use batpu2_se::assemble;

match assemble("program.s", "program.mc") {
    Ok(()) => println!("Success!"),
    Err(e) => eprintln!("Error: {}", e),
}
```
## License

MIT License - See LICENSE file for details

## Related Projects

- **BatPU-2-Speedy**: CLI tool built on this library
- **BatPU-2**: Original redstone computer architecture

## Links

- [BatPU-2 Video](https://www.youtube.com/watch?v=3gBZHXqnleU)
