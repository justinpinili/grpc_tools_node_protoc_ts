grpc_tools_node_protoc_ts
=========================

## Aim
Generate corresponding TypeScript d.ts codes according to js codes generated by [grpc_tools_node_protoc](https://www.npmjs.com/package/grpc-tools).

More information about grpc_tools_node_protoc:

* [npm](https://www.npmjs.com/package/grpc-tools)
* [source code](https://github.com/grpc/grpc-node/tree/master/packages/grpc-tools)
* [doc how to use](https://github.com/grpc/grpc/blob/master/examples/node/static_codegen/README.md)

And the versions over 3.0.0 support [@grpc/grpc-js](https://www.npmjs.com/package/@grpc/grpc-js)(grpc-tools@1.8.1 required). Please read the doc: [@grpc/grpc-js support](https://github.com/agreatfool/grpc_tools_node_protoc_ts/blob/v5.0.0/doc/grpcjs_support.md). **New version 5.0.0 is recommended for users who using grpc-tools over v1.9.0.**

## Breaking changes
### v4.0.0
Fix the issues along with [grpc-tools@1.9.0](https://github.com/grpc/grpc-node/releases/tag/grpc-tools%401.9.0), see: [PR#55](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/55). If you are using grpc-tools with version under `1.9.0`, you should `NOT` upgrade.

Also, if you want to use chain setters, you have to upgrade the version of package `google-protobuf` to at least `3.10.0`. See the difference: [v3.9.0](https://github.com/protocolbuffers/protobuf/blob/v3.9.0/js/message.js#L1088) vs [v3.10.0](https://github.com/protocolbuffers/protobuf/blob/v3.10.0/js/message.js#L1109).

### v2.4.0
The return value of type `ClientReadableStream` was wrong previously. And has been fixed in this version. Please be careful to update your code.
See detail: [PR#30](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/30).

### v2.2.0
Fix definition changes according to the version change of grpc official TypeScript definition, see: [index.d.ts@1.9.0](https://github.com/grpc/grpc-node/blob/v1.9.0/packages/grpc-native-core/index.d.ts).    
Detailed changes could be found here: [PR#14](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/14/files). 

### v2.0.0
Since v2.x.x, current project supports the official definition of grpc, see: [index.d.ts@1.8.4](https://github.com/grpc/grpc-node/blob/v1.8.4/packages/grpc-native-core/index.d.ts).     
Though the usage of tool, and generated codes shall not been changed, it's good to be double checked in your project when upgrade.

TSLint has been disabled in generated files. Please see the conversation: [#13](https://github.com/agreatfool/grpc_tools_node_protoc_ts/issues/13).

## ~~Note~~
~~This tools is using an unofficial grpc.d.ts definition, see: [grpc-tsd](https://www.npmjs.com/package/grpc-tsd).~~    
~~If you want to use this tool, you have to use definition mentioned.~~

## How to use
```bash
npm install grpc_tools_node_protoc_ts --save-dev

# generate js codes via grpc-tools
grpc_tools_node_protoc \
--js_out=import_style=commonjs,binary:./your_dest_dir \
--grpc_out=./your_dest_dir \
--plugin=protoc-gen-grpc=`which grpc_tools_node_protoc_plugin` \
-I ./proto \
./your_proto_dir/*.proto

# generate d.ts codes
protoc \
--plugin=protoc-gen-ts=./node_modules/.bin/protoc-gen-ts \
--ts_out=./your_dest_dir \
-I ./proto \
./your_proto_dir/*.proto
```

## Example
There is a complete & runnable example in folder `examples`.

Dirs:

* proto: sample proto definition
* bash: useful commands
    * build.sh: build js & d.ts codes from proto file, and tsc to build/*.js
    * server.sh: start the server
    * client.sh: start the client & send requests

### book.proto
```proto
syntax = "proto3";

package com.book;

message Book {
    int64 isbn = 1;
    string title = 2;
    string author = 3;
}

message GetBookRequest {
    int64 isbn = 1;
}

message GetBookViaAuthor {
    string author = 1;
}

service BookService {
    rpc GetBook (GetBookRequest) returns (Book) {}
    rpc GetBooksViaAuthor (GetBookViaAuthor) returns (stream Book) {}
    rpc GetGreatestBook (stream GetBookRequest) returns (Book) {}
    rpc GetBooks (stream GetBookRequest) returns (stream Book) {}
}

message BookStore {
    string name = 1;
    map<int64, string> books = 2;
}

enum EnumSample {
    option allow_alias = true;
    UNKNOWN = 0;
    STARTED = 1;
    RUNNING = 1;
    caseTest = 2;
    How_about_This = 3;
    alllowercase = 4;
}

// Message with reserved keywords
// see: https://github.com/google/protobuf/blob/cc3fa2ec80d196e045ae05797799f079188106f3/js/compatibility_tests/v3.0.0/test.proto#L66-L72
message SpecialCases {
    string normal = 1;
    // Examples of Js reserved names that are converted to pb_<name>.
    string default = 2;
    string function = 3;
    string var = 4;
}

message OneOfSample {
    oneof singleword {
        bool a1 = 1;
        bool b1 = 2;
    }

    oneof two_words {
        bool a_2 = 3;
        bool b_2 = 4;
    }
}
```

### book_pb.d.ts
```typescript
// package: com.book
// file: book.proto

/* tslint:disable */
/* eslint-disable */

import * as jspb from "google-protobuf";

export class Book extends jspb.Message { 
    getIsbn(): number;
    setIsbn(value: number): Book;

    getTitle(): string;
    setTitle(value: string): Book;

    getAuthor(): string;
    setAuthor(value: string): Book;


    serializeBinary(): Uint8Array;
    toObject(includeInstance?: boolean): Book.AsObject;
    static toObject(includeInstance: boolean, msg: Book): Book.AsObject;
    static extensions: {[key: number]: jspb.ExtensionFieldInfo<jspb.Message>};
    static extensionsBinary: {[key: number]: jspb.ExtensionFieldBinaryInfo<jspb.Message>};
    static serializeBinaryToWriter(message: Book, writer: jspb.BinaryWriter): void;
    static deserializeBinary(bytes: Uint8Array): Book;
    static deserializeBinaryFromReader(message: Book, reader: jspb.BinaryReader): Book;
}

export namespace Book {
    export type AsObject = {
        isbn: number,
        title: string,
        author: string,
    }
}

export class GetBookRequest extends jspb.Message { 
    getIsbn(): number;
    setIsbn(value: number): GetBookRequest;


    serializeBinary(): Uint8Array;
    toObject(includeInstance?: boolean): GetBookRequest.AsObject;
    static toObject(includeInstance: boolean, msg: GetBookRequest): GetBookRequest.AsObject;
    static extensions: {[key: number]: jspb.ExtensionFieldInfo<jspb.Message>};
    static extensionsBinary: {[key: number]: jspb.ExtensionFieldBinaryInfo<jspb.Message>};
    static serializeBinaryToWriter(message: GetBookRequest, writer: jspb.BinaryWriter): void;
    static deserializeBinary(bytes: Uint8Array): GetBookRequest;
    static deserializeBinaryFromReader(message: GetBookRequest, reader: jspb.BinaryReader): GetBookRequest;
}

export namespace GetBookRequest {
    export type AsObject = {
        isbn: number,
    }
}

export class GetBookViaAuthor extends jspb.Message { 
    getAuthor(): string;
    setAuthor(value: string): GetBookViaAuthor;


    serializeBinary(): Uint8Array;
    toObject(includeInstance?: boolean): GetBookViaAuthor.AsObject;
    static toObject(includeInstance: boolean, msg: GetBookViaAuthor): GetBookViaAuthor.AsObject;
    static extensions: {[key: number]: jspb.ExtensionFieldInfo<jspb.Message>};
    static extensionsBinary: {[key: number]: jspb.ExtensionFieldBinaryInfo<jspb.Message>};
    static serializeBinaryToWriter(message: GetBookViaAuthor, writer: jspb.BinaryWriter): void;
    static deserializeBinary(bytes: Uint8Array): GetBookViaAuthor;
    static deserializeBinaryFromReader(message: GetBookViaAuthor, reader: jspb.BinaryReader): GetBookViaAuthor;
}

export namespace GetBookViaAuthor {
    export type AsObject = {
        author: string,
    }
}

export class BookStore extends jspb.Message { 
    getName(): string;
    setName(value: string): BookStore;


    getBooksMap(): jspb.Map<number, string>;
    clearBooksMap(): void;


    serializeBinary(): Uint8Array;
    toObject(includeInstance?: boolean): BookStore.AsObject;
    static toObject(includeInstance: boolean, msg: BookStore): BookStore.AsObject;
    static extensions: {[key: number]: jspb.ExtensionFieldInfo<jspb.Message>};
    static extensionsBinary: {[key: number]: jspb.ExtensionFieldBinaryInfo<jspb.Message>};
    static serializeBinaryToWriter(message: BookStore, writer: jspb.BinaryWriter): void;
    static deserializeBinary(bytes: Uint8Array): BookStore;
    static deserializeBinaryFromReader(message: BookStore, reader: jspb.BinaryReader): BookStore;
}

export namespace BookStore {
    export type AsObject = {
        name: string,

        booksMap: Array<[number, string]>,
    }
}

export class SpecialCases extends jspb.Message { 
    getNormal(): string;
    setNormal(value: string): SpecialCases;

    getDefault(): string;
    setDefault(value: string): SpecialCases;

    getFunction(): string;
    setFunction(value: string): SpecialCases;

    getVar(): string;
    setVar(value: string): SpecialCases;


    serializeBinary(): Uint8Array;
    toObject(includeInstance?: boolean): SpecialCases.AsObject;
    static toObject(includeInstance: boolean, msg: SpecialCases): SpecialCases.AsObject;
    static extensions: {[key: number]: jspb.ExtensionFieldInfo<jspb.Message>};
    static extensionsBinary: {[key: number]: jspb.ExtensionFieldBinaryInfo<jspb.Message>};
    static serializeBinaryToWriter(message: SpecialCases, writer: jspb.BinaryWriter): void;
    static deserializeBinary(bytes: Uint8Array): SpecialCases;
    static deserializeBinaryFromReader(message: SpecialCases, reader: jspb.BinaryReader): SpecialCases;
}

export namespace SpecialCases {
    export type AsObject = {
        normal: string,
        pb_default: string,
        pb_function: string,
        pb_var: string,
    }
}

export class OneOfSample extends jspb.Message { 

    hasA1(): boolean;
    clearA1(): void;
    getA1(): boolean;
    setA1(value: boolean): OneOfSample;


    hasB1(): boolean;
    clearB1(): void;
    getB1(): boolean;
    setB1(value: boolean): OneOfSample;


    hasA2(): boolean;
    clearA2(): void;
    getA2(): boolean;
    setA2(value: boolean): OneOfSample;


    hasB2(): boolean;
    clearB2(): void;
    getB2(): boolean;
    setB2(value: boolean): OneOfSample;


    getSinglewordCase(): OneOfSample.SinglewordCase;
    getTwoWordsCase(): OneOfSample.TwoWordsCase;

    serializeBinary(): Uint8Array;
    toObject(includeInstance?: boolean): OneOfSample.AsObject;
    static toObject(includeInstance: boolean, msg: OneOfSample): OneOfSample.AsObject;
    static extensions: {[key: number]: jspb.ExtensionFieldInfo<jspb.Message>};
    static extensionsBinary: {[key: number]: jspb.ExtensionFieldBinaryInfo<jspb.Message>};
    static serializeBinaryToWriter(message: OneOfSample, writer: jspb.BinaryWriter): void;
    static deserializeBinary(bytes: Uint8Array): OneOfSample;
    static deserializeBinaryFromReader(message: OneOfSample, reader: jspb.BinaryReader): OneOfSample;
}

export namespace OneOfSample {
    export type AsObject = {
        a1: boolean,
        b1: boolean,
        a2: boolean,
        b2: boolean,
    }

    export enum SinglewordCase {
        SINGLEWORD_NOT_SET = 0,
    
    A1 = 1,

    B1 = 2,

    }

    export enum TwoWordsCase {
        TWO_WORDS_NOT_SET = 0,
    
    A_2 = 3,

    B_2 = 4,

    }

}

export enum EnumSample {
    UNKNOWN = 0,
    STARTED = 1,
    RUNNING = 1,
    CASETEST = 2,
    HOW_ABOUT_THIS = 3,
    ALLLOWERCASE = 4,
}
```

### book_grpc_pb.d.ts
```typescript
// package: com.book
// file: book.proto

/* tslint:disable */
/* eslint-disable */

import * as grpc from "grpc";
import * as book_pb from "./book_pb";

interface IBookServiceService extends grpc.ServiceDefinition<grpc.UntypedServiceImplementation> {
    getBook: IBookServiceService_IGetBook;
    getBooksViaAuthor: IBookServiceService_IGetBooksViaAuthor;
    getGreatestBook: IBookServiceService_IGetGreatestBook;
    getBooks: IBookServiceService_IGetBooks;
}

interface IBookServiceService_IGetBook extends grpc.MethodDefinition<book_pb.GetBookRequest, book_pb.Book> {
    path: string; // "/com.book.BookService/GetBook"
    requestStream: false;
    responseStream: false;
    requestSerialize: grpc.serialize<book_pb.GetBookRequest>;
    requestDeserialize: grpc.deserialize<book_pb.GetBookRequest>;
    responseSerialize: grpc.serialize<book_pb.Book>;
    responseDeserialize: grpc.deserialize<book_pb.Book>;
}
interface IBookServiceService_IGetBooksViaAuthor extends grpc.MethodDefinition<book_pb.GetBookViaAuthor, book_pb.Book> {
    path: string; // "/com.book.BookService/GetBooksViaAuthor"
    requestStream: false;
    responseStream: true;
    requestSerialize: grpc.serialize<book_pb.GetBookViaAuthor>;
    requestDeserialize: grpc.deserialize<book_pb.GetBookViaAuthor>;
    responseSerialize: grpc.serialize<book_pb.Book>;
    responseDeserialize: grpc.deserialize<book_pb.Book>;
}
interface IBookServiceService_IGetGreatestBook extends grpc.MethodDefinition<book_pb.GetBookRequest, book_pb.Book> {
    path: string; // "/com.book.BookService/GetGreatestBook"
    requestStream: true;
    responseStream: false;
    requestSerialize: grpc.serialize<book_pb.GetBookRequest>;
    requestDeserialize: grpc.deserialize<book_pb.GetBookRequest>;
    responseSerialize: grpc.serialize<book_pb.Book>;
    responseDeserialize: grpc.deserialize<book_pb.Book>;
}
interface IBookServiceService_IGetBooks extends grpc.MethodDefinition<book_pb.GetBookRequest, book_pb.Book> {
    path: string; // "/com.book.BookService/GetBooks"
    requestStream: true;
    responseStream: true;
    requestSerialize: grpc.serialize<book_pb.GetBookRequest>;
    requestDeserialize: grpc.deserialize<book_pb.GetBookRequest>;
    responseSerialize: grpc.serialize<book_pb.Book>;
    responseDeserialize: grpc.deserialize<book_pb.Book>;
}

export const BookServiceService: IBookServiceService;

export interface IBookServiceServer {
    getBook: grpc.handleUnaryCall<book_pb.GetBookRequest, book_pb.Book>;
    getBooksViaAuthor: grpc.handleServerStreamingCall<book_pb.GetBookViaAuthor, book_pb.Book>;
    getGreatestBook: grpc.handleClientStreamingCall<book_pb.GetBookRequest, book_pb.Book>;
    getBooks: grpc.handleBidiStreamingCall<book_pb.GetBookRequest, book_pb.Book>;
}

export interface IBookServiceClient {
    getBook(request: book_pb.GetBookRequest, callback: (error: grpc.ServiceError | null, response: book_pb.Book) => void): grpc.ClientUnaryCall;
    getBook(request: book_pb.GetBookRequest, metadata: grpc.Metadata, callback: (error: grpc.ServiceError | null, response: book_pb.Book) => void): grpc.ClientUnaryCall;
    getBook(request: book_pb.GetBookRequest, metadata: grpc.Metadata, options: Partial<grpc.CallOptions>, callback: (error: grpc.ServiceError | null, response: book_pb.Book) => void): grpc.ClientUnaryCall;
    getBooksViaAuthor(request: book_pb.GetBookViaAuthor, options?: Partial<grpc.CallOptions>): grpc.ClientReadableStream<book_pb.Book>;
    getBooksViaAuthor(request: book_pb.GetBookViaAuthor, metadata?: grpc.Metadata, options?: Partial<grpc.CallOptions>): grpc.ClientReadableStream<book_pb.Book>;
    getGreatestBook(callback: (error: grpc.ServiceError | null, response: book_pb.Book) => void): grpc.ClientWritableStream<book_pb.GetBookRequest>;
    getGreatestBook(metadata: grpc.Metadata, callback: (error: grpc.ServiceError | null, response: book_pb.Book) => void): grpc.ClientWritableStream<book_pb.GetBookRequest>;
    getGreatestBook(options: Partial<grpc.CallOptions>, callback: (error: grpc.ServiceError | null, response: book_pb.Book) => void): grpc.ClientWritableStream<book_pb.GetBookRequest>;
    getGreatestBook(metadata: grpc.Metadata, options: Partial<grpc.CallOptions>, callback: (error: grpc.ServiceError | null, response: book_pb.Book) => void): grpc.ClientWritableStream<book_pb.GetBookRequest>;
    getBooks(): grpc.ClientDuplexStream<book_pb.GetBookRequest, book_pb.Book>;
    getBooks(options: Partial<grpc.CallOptions>): grpc.ClientDuplexStream<book_pb.GetBookRequest, book_pb.Book>;
    getBooks(metadata: grpc.Metadata, options?: Partial<grpc.CallOptions>): grpc.ClientDuplexStream<book_pb.GetBookRequest, book_pb.Book>;
}

export class BookServiceClient extends grpc.Client implements IBookServiceClient {
    constructor(address: string, credentials: grpc.ChannelCredentials, options?: object);
    public getBook(request: book_pb.GetBookRequest, callback: (error: grpc.ServiceError | null, response: book_pb.Book) => void): grpc.ClientUnaryCall;
    public getBook(request: book_pb.GetBookRequest, metadata: grpc.Metadata, callback: (error: grpc.ServiceError | null, response: book_pb.Book) => void): grpc.ClientUnaryCall;
    public getBook(request: book_pb.GetBookRequest, metadata: grpc.Metadata, options: Partial<grpc.CallOptions>, callback: (error: grpc.ServiceError | null, response: book_pb.Book) => void): grpc.ClientUnaryCall;
    public getBooksViaAuthor(request: book_pb.GetBookViaAuthor, options?: Partial<grpc.CallOptions>): grpc.ClientReadableStream<book_pb.Book>;
    public getBooksViaAuthor(request: book_pb.GetBookViaAuthor, metadata?: grpc.Metadata, options?: Partial<grpc.CallOptions>): grpc.ClientReadableStream<book_pb.Book>;
    public getGreatestBook(callback: (error: grpc.ServiceError | null, response: book_pb.Book) => void): grpc.ClientWritableStream<book_pb.GetBookRequest>;
    public getGreatestBook(metadata: grpc.Metadata, callback: (error: grpc.ServiceError | null, response: book_pb.Book) => void): grpc.ClientWritableStream<book_pb.GetBookRequest>;
    public getGreatestBook(options: Partial<grpc.CallOptions>, callback: (error: grpc.ServiceError | null, response: book_pb.Book) => void): grpc.ClientWritableStream<book_pb.GetBookRequest>;
    public getGreatestBook(metadata: grpc.Metadata, options: Partial<grpc.CallOptions>, callback: (error: grpc.ServiceError | null, response: book_pb.Book) => void): grpc.ClientWritableStream<book_pb.GetBookRequest>;
    public getBooks(options?: Partial<grpc.CallOptions>): grpc.ClientDuplexStream<book_pb.GetBookRequest, book_pb.Book>;
    public getBooks(metadata?: grpc.Metadata, options?: Partial<grpc.CallOptions>): grpc.ClientDuplexStream<book_pb.GetBookRequest, book_pb.Book>;
}
```

## About jstype options of protobuf

JavaScript can safely handle numbers below `Number.MAX_SAFE_INTEGER`. Beyond the limit, it begins to overflow:

```
> 90071992547409912131 + 1
90071992547409920000
```

If you are expecting large, 64bit fields consider using a `jstype` option to override the field type.

```proto
# example.proto
message Example {
  fixed64 id = 1 [jstype = JS_STRING];
}
```

```typescript
// example_pb.d.ts
export namespace Example {
    export type AsObject = {
        id: string
    }
}
```

## Changes
### 5.0.0
Migrating to option `grpc_js` from [grpc-tools@1.9.0](https://github.com/grpc/grpc-node/releases/tag/grpc-tools%401.9.0).

Two points:

* Using option `grpc_js` in `grpc_tools_node_protoc ... --grpc_out=grpc_js:...`
* Style of generated js codes looks like traditional `grpc_tools_node_protoc ...  --plugin=protoc-gen-grpc=which grpc_tools_node_protoc_plugin`

Difference between grpc-tools version `1.8.1` and `1.9.0`, and also between `generate_package_definition` and `grpc_js` could be found here: [@grpc/grpc-js support @5.0.0](https://github.com/agreatfool/grpc_tools_node_protoc_ts/blob/v5.0.0/doc/grpcjs_support.md).

Users who are still using version 3.0.0 - 4.1.5 would not be affected. In version 5.0.0 options `generate_package_definition` and `grpc_js` are all available. 

Though, 5.0.0 is recommended for users who using grpc-tools@1.9.0 or over, don't forget to switch your option to `grpc_js`. And remember, if you switch the cli command option to `grpc_js`, you have to adjust your typescript implementation, see: [examples/src/grpcjs](https://github.com/agreatfool/grpc_tools_node_protoc_ts/tree/v5.0.0/examples/src/grpcjs).

See more details:

* [PR#71](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/71)
* [Broken ts imports after moving to grpc-js #70](https://github.com/agreatfool/grpc_tools_node_protoc_ts/issues/70)
* [Broken typescript exports after moving to @grpc/grpc-js#1600](https://github.com/grpc/grpc-node/issues/1600#issuecomment-705097639)

### 4.1.5
Use string literal type for path. See: [PR#69](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/69).

### 4.1.4
Add `.npmignore` to shrink the size of npm package. See: [PR#65](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/65).

### 4.1.3
Update doc to solve issues like: [Issue#63](https://github.com/agreatfool/grpc_tools_node_protoc_ts/issues/63).

### 4.1.1 - 4.1.2
Update grpc-js example codes. Fix vulnerabilities of example packages. Fix type of third param of Client class, see: [PR#62](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/62).

### 4.1.0
Switch the type of `requestStream` & `responseStream` in file `*_grpc_pb.d.ts` from `boolean` to `true | false`. See: [PR#58](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/58).

### 4.0.0
[grpc-tools@1.9.0](https://github.com/grpc/grpc-node/releases/tag/grpc-tools%401.9.0) changes the generated js codes a lot. Current version is released to fix the setters issue, see: [PR#55](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/55). Note: This is a breaking change, if you are still using grpc-tools with version under `1.9.0`, you `DO NOT` need to upgrade, version `4.0.0` would break your codes.

Also, if you want to use chain setters, you have to upgrade the version of package `google-protobuf` to at least `3.10.0`. See the difference: [v3.9.0](https://github.com/protocolbuffers/protobuf/blob/v3.9.0/js/message.js#L1088) vs [v3.10.0](https://github.com/protocolbuffers/protobuf/blob/v3.10.0/js/message.js#L1109).

### 3.0.0
[@grpc/grpc-js](https://www.npmjs.com/package/@grpc/grpc-js) now is supported. See: [Issue#56](https://github.com/agreatfool/grpc_tools_node_protoc_ts/issues/56). Users who still using [grpc](https://www.npmjs.com/package/grpc) would **NOT** be affected. More detailed information, please go to [@grpc/grpc-js support](https://github.com/agreatfool/grpc_tools_node_protoc_ts/blob/master/doc/grpcjs_support.md). Note: This upgrade requires grpc-tools version `1.8.1`.

### 2.5.11
Fix vulnerabilities. See: [PR#54](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/54).

### 2.5.10
Fix issue of oneof functionality. See: [PR#53](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/53).

### 2.5.9
Fix vulnerabilities. See: [Issue#52](https://github.com/agreatfool/grpc_tools_node_protoc_ts/issues/52).

### 2.5.8
Fix vulnerabilities. See: [Issue#49](https://github.com/agreatfool/grpc_tools_node_protoc_ts/issues/49).

### 2.5.6 - 2.5.7
Add "/* eslint-disable */" in generated codes. See: [Issue#48](https://github.com/agreatfool/grpc_tools_node_protoc_ts/issues/48). Also update package.json, to fix vulnerabilities.

### 2.5.5
Fix extensions with reserved keywords. See: [PR#46](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/46).

### 2.5.4
Codes enhancement, use explicit model in coding. See: [PR#43](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/43). There is no functionality changes in this version update.

### 2.5.3
Update doc, add `About vulnerability` part.

### 2.5.2
Fix vulnerability. See: [PR#41](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/41).

### 2.5.1
Remove unnecessary dependency `grpc` from package.json. See: [Issue#40](https://github.com/agreatfool/grpc_tools_node_protoc_ts/issues/40).

### 2.5.0
Add support for `[jstype = JS_STRING]` overrides. See: [PR#37](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/37).

### 2.4.2
Update grpc version in package.json of examples, to keep consistent.
Update handlebars-helpers version to 0.10.0 to fix vulnerability version of randomatic.

### 2.4.1
Use gRPC-native ServiceError objects for errbacks in client methods. See: [PR#33](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/33).

### 2.4.0
Fix(ClientReadableStream): Fixes return type generic argument of. See: [PR#30](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/30).

### 2.3.2
Fix code generation error when there is no package defined in proto file. See: [PR#28](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/28).

### 2.3.1
Fix an Enum case bug. See: [Issue#25](https://github.com/agreatfool/grpc_tools_node_protoc_ts/issues/25) & [PR#26](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/26).

### 2.3.0
Add a new server implementation interface signature, with this the server implementation could be verified. See: [Issue#22](https://github.com/agreatfool/grpc_tools_node_protoc_ts/issues/22). And please also check the new example using this new feature: [link](https://github.com/agreatfool/grpc_tools_node_protoc_ts/blob/33946064e6d5134a32edffa0d1c0bce80055fe56/examples/src/server.ts#L11-L71).

### 2.2.5
Fix issue of reversed JavaScript keyword code generation. See: [Issue#20](https://github.com/agreatfool/grpc_tools_node_protoc_ts/issues/20) & [PR#21](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/21).

### 2.2.4
Fix issue of conflicted I{$MethodName} name, see: [Issue#19](https://github.com/agreatfool/grpc_tools_node_protoc_ts/issues/19). 

### 2.2.3
Fix definitions. [fix: add missing argument grpc.Client~CallOptions for requests](https://github.com/agreatfool/grpc_tools_node_protoc_ts/pull/15/commits/ea3bff861201446346a2e6dfe511edc8f0cb6fdf)

## About vulnerability
Since this tool is used to generate `typescript signature` for the codes generated from `grpc_tools_node_protoc`, the codes actually run with your application are from `grpc_tools_node_protoc`, not this tool. So it's still safe to be used even there are some vulnerabilities in it.

If you want your project to have no vulnerabilities detected, you could simply install this tool into global (out of your application project). And use it when you want to generate the typescript signature in development as usual.

## About Docker
Sample below, just the idea, not perfect. Also some more info: [Issue#38](https://github.com/agreatfool/grpc_tools_node_protoc_ts/issues/38#issuecomment-465475399). 

```Dockerfile
FROM node:8.4.0

RUN apt-get update && \
    apt-get -y install git unzip build-essential autoconf libtool

RUN git clone https://github.com/google/protobuf.git && \
    cd protobuf && \
    ./autogen.sh && \
    ./configure && \
    make && \
    make install && \
    ldconfig && \
    make clean && \
    cd .. && \
    rm -r protobuf

RUN git clone https://github.com/agreatfool/grpc_tools_node_protoc_ts.git grpc_protoc && \
    cd grpc_protoc && \
    npm i -g grpc-tools@1.6.6 --unsafe-perm && \
    npm i -g typescript --unsafe-perm && \
    npm i --unsafe-perm
```
```bash
$ docker build -t node_protoc_plugin:0.1 .
$ docker run --rm -i -t node_protoc_plugin:0.1 /bin/bash
$ root@63249303596f:/# cd /grpc_protoc && ./bash/build.sh
```

## Environment
```bash
node --version
# v10.15.2
npm --version
# 6.14.4
protoc --version
# libprotoc 3.11.1
grpc_tools_node_protoc --version
# libprotoc 3.11.4
npm list -g --depth=0 | grep grpc-tools
# grpc-tools@1.9.1
tsc --version
# Version 3.7.3
```
