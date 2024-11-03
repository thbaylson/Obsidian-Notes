[[Game Development Vocabulary]]
Computer Science Vocabulary

"A _NativeArray_ exposes a buffer of _native_ memory to managed code, making it possible to share data between managed and _native_ without marshalling costs." - [Unity Documentation](https://docs.unity3d.com/2022.3/Documentation/ScriptReference/Unity.Collections.NativeArray_1.html)

"A normal array uses managed memory to allocate on the Heap. On the other hand, a native array gives direct access to native memory outside of the managed environment. This results in faster performance especially for tasks like physics, rendering, or anything that's multi-threaded. That also means it's not managed by the garbage collection, so when we're finished using this memory we need to release it using the dispose method."  - [git-amend](https://www.youtube.com/watch?v=vxZx_PXo-yo)

#Unity 