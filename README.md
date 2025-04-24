# Supra Conversion Utils Hook Documentation

This README explains the `useConversionUtils` hook, which provides utility functions for handling data type conversions needed when interacting with Supra blockchain (MoveVM).

## Installation

To use this hook, you need to install the Supra L1 SDK:

```bash
npm install supra-l1-sdk
```

## Usage in a Client Component

```jsx
'use client';

import React from 'react';
import useConversionUtils from './path/to/useConversionUtils';

function MyComponent() {
  const {
    stringToUint8Array,
    serializeString,
    addressToUint8Array,
    serializeUint64,
    serializeVector,
    hexToString
  } = useConversionUtils();

  const handleTransaction = () => {
    // Example: Sending a transaction with different parameter types
    const serializedString = stringToUint8Array("example_data");
    const serializedAddress = addressToUint8Array("0x123abc...");
    const serializedAmount = serializeUint64(BigInt(1000));
    const serializedBoolArray = serializeVector([true, false, true], 'bool');
    
    // Use these serialized values in your transaction...
  };

  return (
    <div>
      {/* Your component content */}
    </div>
  );
}

export default MyComponent;
```

## Function Reference

### String Serialization

#### `stringToUint8Array(humanReadableStr: string)`
- Converts a human-readable string to a Uint8Array using BCS.
- **Use when**: On-chain function accepts a `vector<u8>` parameter but you have a string.

#### `serializeString(humanReadableStr: string)`
- Serializes a string for on-chain transaction.
- **Use when**: On-chain function accepts a `String` parameter.

### Address Handling

#### `addressToUint8Array(cryptoAddress: string)`
- Converts a cryptocurrency address (hex format) to Uint8Array.
- **Use when**: On-chain function accepts an `address` parameter.
- Will be renamed to `serializeAddress` in future updates.

### Numeric Value Serialization

#### `serializeUint64(value: bigint)`
- Serializes a BigInt value to the BCS format expected for uint64.
- **Use when**: On-chain function accepts a `u64` parameter.

#### `serializeU256(value: bigint)`
- Serializes a BigInt value to the BCS format expected for u256.
- **Use when**: On-chain function accepts a `u256` parameter.

### Boolean Serialization

#### `serializeBool(value: boolean)`
- Serializes a boolean value.
- **Use when**: On-chain function accepts a `bool` parameter.

### Collection/Vector Serialization

#### `serializeVector(values: any[], type: 'u8' | 'u64' | 'bool' | 'string' | 'address')`
- Serializes an array of values of specified type into a BCS vector.
- **Use when**: On-chain function accepts a vector type like `vector<bool>`, `vector<address>`, etc.
- **Example**: `serializeVector([true, false, true], 'bool')` to serialize a `vector<bool>` parameter.

### Deserialization

#### `deserializeString(uint8Array: string)`
- Deserializes a string from serialized format.

#### `deserializeVector(uint8Array: Uint8Array)`
- Deserializes a vector from serialized format.

### Hex Conversions

#### `hexToString(hex: string, type: string)`
- Converts a hex string back to human-readable format.
- For numeric types, converts to decimal.
- For string types, decodes the hex to string.
- **Use when**: Reading values from the blockchain that are in hex format.

#### `stringToHex(str: string)`
- Converts a string to hex format.
- **Use when**: You need to represent a string as hex.

## Important Notes

1. All parameters sent to Supra blockchain (MoveVM) need to be properly serialized.
2. Different data types require different serialization methods.
3. This hook simplifies the complex task of determining the correct serialization method for each type.
4. Always match the serialization function to the expected parameter type in your Move code.

## Example Scenarios

- If a Move function expects `vector<u8>` for a string parameter: Use `stringToUint8Array`
- If a Move function expects `String`: Use `serializeString`
- If a Move function expects `address`: Use `addressToUint8Array`
- If a Move function expects `vector<bool>`: Use `serializeVector(values, 'bool')`
- If reading hex-encoded data from chain: Use `hexToString` with appropriate type
