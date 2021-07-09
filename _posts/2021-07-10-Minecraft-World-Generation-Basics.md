---
layout: post
title: World Generation First Steps!
---

`package net.minecraft.world.level.biome;`

`Biomes.java`

```
public abstract class Biomes {
    public static final Biome PLAINS;
     static {
        PLAINS = register(1, "plains", new PlainsBiome());
}
}
```
