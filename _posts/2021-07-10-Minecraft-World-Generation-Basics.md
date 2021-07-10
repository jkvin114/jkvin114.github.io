---
layout: post
title: World Generation First Steps!
---

World generation steps overview
-------------

`ChunkStatus.java(simplified)`
```java
ststic{
        STRUCTURE_REFERENCES = ( () -> createReferences());
        BIOMES = (ChunkStatus.STRUCTURE_REFERENCES () -> createBiomes());
        NOISE = ( ChunkStatus.BIOMES,() -> fillFromNoise());
        SURFACE = ( ChunkStatus.NOISE,  () -> buildSurfaceAndBedrock());
        CARVERS = (ChunkStatus.SURFACE, () -> applyCarvers(by air));
        LIQUID_CARVERS = ( ChunkStatus.CARVERS, () -> applyCarvers(by liquid));
        FEATURES = register(ChunkStatus.LIQUID_CARVERS, () -> applyBiomeDecoration())
       }
```

One chunk generates by these steps:
1. STRUCTURE_REFERENCES(decides where the structure will generate)
2. BIOMES(decides biomes for each coordinates)
3. NOISE(generates the height and shape of the terrain)
4. SURFACE(generates bedrock and surface blocks)
5. CARVERS and LIQUID_CARVERS (generates caves and canyons)
6. FEATURES(generates other decorations such as structures,plants,ores, and lakes)


>Generates ths shape of the terrain which differs by the biomes.\
>fill every block with stones by default.

![steps1](/images/wg_steps(6).png)
![steps2](/images/wg_steps(12).png)

>Generates bedrock

![steps3](/images/wg_steps(11).png)

>Fill every air block below y=64 with water and generate surface blocks

![steps4](/images/wg_steps(10).png)
![steps5](/images/wg_steps(5).png)
![steps6](/images/wg_steps(4).png)

>Generates caves and canyons

![steps7](/images/wg_steps(9).png)

>Generates structure if the biome can generate that structure

![steps8](/images/wg_steps(3).png)

>Generates blocks such as ores and sand&clay disks
>
![steps9](/images/wg_steps(8).png)
![steps10](/images/wg_steps(2).png)

>Generates plants of the biome

![steps10](/images/wg_steps(7).png)
![steps10](/images/wg_steps(1).png)
















Register biomes and make it as constant
`package net.minecraft.world.level.biome;`

`Biomes.java`

```java
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
