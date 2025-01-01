# Glossary
Forestry's API is quite complex and can be difficult to understand at first. I've written a glossary of
important concepts that you'll want to be aware of when writing addons or KubeJS scripts.

---

## Individual
An individual, also referred to as an organism, is a living thing represented by a block, item, or entity.
In Forestry, any bee, sapling, pollen, or butterfly is an individual. Leaf blocks on a tree are individuals,
and entity forms of butterflies are also individuals. Being an individual means:

- Can be mated with other individuals of the same species type to produce offspring
- Can be analyzed using the Portable Analyzer
- Can be researched using the Escritoire

---

## Species Type
A species type is a type of individual. In base Forestry, there are three species types: Bee, Tree, and Butterfly.
The species type defines the following traits:  

* The karyotype
* The life stages
* The item used to represent the species
* The research materials for members of the species
* The possible mutations between different species within the species type

### Karyotype
The karyotype defines the genome structure of a member of this species type.
More specifically, the karyotype defines the following properties:  

* All chromosomes a member's genome has
* The default alleles for each chromosome
* The alleles that each chromosome can take on
* Whether a chromosome is _weakly inherited_'

So, in the case of the Bee species type, the Karyotype defines the following:  

* Every bee has the same 13 chromosomes
* The allowed alleles for the `BeeChromosomes.SPEED` chromosomes are those found in `ForestryAlleles.DEFAULT_SPEEDS`

### Life Stage
A life stage is a way to represent different forms of the same species type.
A bee's default life stages:  

* Drone
* Princess
* Queen
* Larvae

A tree's default life stages:  

* Sapling
* Pollen

A butterfly's default life stages:  

* Butterfly
* Caterpillar
* Serum
* Cocoon

### Research Materials
A research material is just an item that can be used to get hints in the Escritoire minigame while researching a species.
A research material has a percentage associated with it that determines whether a hint will be successful or not.
This means that some research materials (ex. Honeydew) are better than others (ex. Honeycomb).

### Mutations
A mutation can occur when two individuals of different species are bred together that results in offspring that has a
a new, third species. Mutations define two different parent species, a resulting child species, and a percentage chance
for that mutation to occur. Mutations may also specify mutation conditions, for example, the holiday bees require the
date to be during a certain holiday, and other mutations might only be possible in certain biomes.

---

## Species
A species is a variation of a species type. A species has:

- Intrinsic traits, like colors, textures, temperature and humidity preferences, and a taxonomy
- Default genome, even with alleles its parents did not have
- Produce (ex. honey combs, cherries, silk cocoon)

### Bee Species
A bee species has:

- Colors for outline, body, and stripes
- Climate preferences for temperature and humidity
- Produce, items it can produce while working in a hive
- Specialties, items it can produce while in a jubilant state
- Jubilance conditions, determines when a bee is in a jubilant state

---

## Genome
All individuals in Forestry have a genome, which stores the alleles of a bee.
Specifically, the genome is a map of _chromosomes_ to _allele pairs_.

### Chromosome
A chromosome is a property of an individual, used as a key in the individual's genome. Common chromosomes include:  

* Bee Species
* Tree Species
* Fertility
* Lifespan
* Metabolism
* Activity
* \+ many others

Builtin chromosomes can be found in the 
[`BeeChromosomes`](https://github.com/thedarkcolour/ForestryCE/blob/1.20.1/src/main/java/forestry/api/genetics/alleles/BeeChromosomes.java), 
[`TreeChromosomes`](https://github.com/thedarkcolour/ForestryCE/blob/1.20.1/src/main/java/forestry/api/genetics/alleles/TreeChromosomes.java), and 
[`ButterflyChromosomes`](https://github.com/thedarkcolour/ForestryCE/blob/1.20.1/src/main/java/forestry/api/genetics/alleles/ButterflyChromosomes.java) classes. 
These classes list the chromosomes of each species type and their descriptions. The values of a chromosomes are called _alleles_.

### Allele Pair
An allele pair contains two _alleles_. The first allele is the _active_ allele, the second allele
is the _inactive_ allele. For most traits, only the active allele is used. However, for traits like the bee's products or
custom effect, it is possible for both the active and inactive alleles to be used, which means hybrid bees can have
two effects at once, and can produce products from two species at once.

### Allele
An allele is a named value in a genome. Builtin alleles are found in the `ForestryAlleles` class.
Alleles may be _dominant_ or _recessive_, which determines its importance during inheritance.
All alleles contain a value. In the case of simple chromosomes like Fertility or Lifespan, this value might be an integer
or a floating point number that functions as a multiplier of that characteristic. For more complex chromosomes, like the
species chromosome or the bee effect chromosome, this value is typically an object, like IBeeSpecies or IActivityType,
that has complex logic associated with it.

For example, if my bee's genome has `ForestryAlleles.ACTIVITY_DIURNAL` for its `BeeChromosomes.ACTIVITY` chromosome, then
the bee will only work during the daytime.

Another example is if my tree's genome has the `ForestryAlleles.TREE_EFFECT_BLOSSOMING` for its `TreeChromosomes.EFFECT` chromosome,
then my tree's leaves will spawn cherry blossom particles periodically, just like the Cherry tree added in Minecraft 1.20.