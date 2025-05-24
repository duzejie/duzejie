array of structures (AoS)和structure of arrays (SoA)，这分别代表着两种不同的数组组织形式。
- [SoA vs AoS - 技术专栏 - Unity官方开发者社区](https://developer.unity.cn/projects/61ff5161edbc2a001cf9856e)
![[Pasted image 20230526192201.png]]


### Array-of-Structures (AoS)

The Array-of-Structures layout is the most common and intuitive layout.

### Structure-of-Arrays (SoA)

The Structure-of-Arrays layout is less intuitive to work with because it will store each field of a struct into its own array. Thus, our set of vector set would look like that:
```c
struct SetOfVector3 {
	x: [f32; 1024], 
	y: [f32; 1024], 
	z: [f32; 1024], 
}
```

### Array-of-Structures-of-Arrays (AoSoA)

Now let's combine both SoA and AoS to obtain: Array-of-Structures-of-Arrays (AoSoA). The idea is to first define a _wide_ 3D vector, i.e., we pack several vectors into a single struct:

```rust
struct WideVector3 {
    x: [f32; 4],
    y: [f32; 4],
    z: [f32; 4],
}
```

[SIMD Array-of-Structures-of-Arrays in nalgebra and comparison with ultraviolet · The rustsim organization](https://www.rustsim.org/blog/2020/03/23/simd-aosoa-in-nalgebra/)

[Memory Layout Transformations (intel.com)](https://www.intel.com/content/www/us/en/developer/articles/technical/memory-layout-transformations.html)




[njroussel/aosoa: Header-only C++ container to easily convert between AOS and SOA data layouts (github.com)](https://github.com/njroussel/aosoa)

