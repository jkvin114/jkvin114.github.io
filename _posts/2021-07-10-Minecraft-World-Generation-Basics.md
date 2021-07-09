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

Add features for each biome

`PlainsBiome.java`

```java
public final class PlainsBiome extends Biome {
    protected PlainsBiome() {
        super(new BiomeBuilder().surfaceBuilder(add surface builders).precipitation(Precipitation.RAIN).biomeCategory(BiomeCategory.PLAINS).depth(0.125f).scale(0.05f).temperature(0.8f).downfall(0.4f).waterColor(4159204).waterFogColor(329011).parent(null));
        this.addStructureStart(add village);
        this.addStructureStart(add pillager outpost);
        this.addStructureStart(add mineshaft);
        this.addStructureStart(add stronghold);
        BiomeDefaultFeatures.addDefaultCarvers(this);  //add caves
        .
        .
        .
        .
     }
}
```

> Carvers(caves and canyons)

`package net.minecraft.world.level.chunk; ChunkStatus.java`

```java
    public class ChunkStatus {
        static{
            CARVERS = registerSimple("carvers", ChunkStatus.SURFACE, 0, ChunkStatus.PRE_FEATURES, ChunkType.PROTOCHUNK, (serverLevel, chunkGenerator, list, chunk) -> chunkGenerator.applyCarvers(serverLevel.getBiomeManager().withDifferentSource(chunkGenerator.getBiomeSource()), chunk, GenerationStep.Carving.AIR));
        }
    }
```

Method that adds default carvers for each biomes

`BiomeDefaultFeatures.java`

```java
public class BiomeDefaultFeatures {

        public static void addDefaultCarvers(final Biome biome) {
        biome.addCarver(GenerationStep.Carving.AIR, Biome.makeCarver((WorldCarver<C>)WorldCarver.CAVE, (C)new ProbabilityFeatureConfiguration(cave rarity)));

        biome.addCarver(GenerationStep.Carving.AIR, Biome.makeCarver((WorldCarver<C>)WorldCarver.CANYON, (C)new ProbabilityFeatureConfiguration(canyon rarity)));
    }
}
```
