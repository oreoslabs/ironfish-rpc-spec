# 1. RPC Overview
This section provides instructions on interface standard of Ironfish rpc request and response, and how to perform common requests.

1. **Request Interface Standard**
   ```typescript
   {
      type: 'message',
      data: {
        mid: number
        type: string
        auth: string | null | undefined
        data: unknown | undefined
      }
    }
   ```
   
2. **Response Interface Standard**  
   ```typescript
   {
      type: 'message' | 'malformedRequest' | 'error' | 'stream'
      data: SocketRpcResponse | SocketRpcError | SocketRpcStream
    }
      ```
   ```typescript
   type SocketRpcResponse = {
     id: number;
     status: number;
     data: unknown | undefined;
   }
   
   type SocketRpcStream = {
     id: number
     data: unknown | undefined
   }
   
   type SocketRpcError = {
     code: string
     message: string
     stack?: string
   }
   ```

3. **Supported Communication Protocols**  
   Ironfish node supports rpc request over `IPC`, `TCP` and `TLS`. 


4. **Convenience Libraries**  
   While you may choose to interact directly with Ironfish node via the RPC API, there are often easier options. [oreos in Typescript](https://www.npmjs.com/package/oreos) supports a subset of Ironfish rpc methods and provides wrappers on top of the RPC API. 
