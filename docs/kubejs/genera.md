# Genera with KubeJS
Genera is the plural form of **genus**. But what is a genus?

## What is a genus?
The [genus](https://en.wikipedia.org/wiki/Genus), a taxonomic rank, denotes a group of closely-related species. It is
used as the left half of a species's scientific name. In Forestry, a genus can also define *default alleles* for its members,
overriding the defaults for the species.

_Bee genera_ often correspond to what you might call "bee lines." For example, the Diligent/Unweary/Industrious line of 
bees are all members of the genus `ForestryTaxa.GENUS_INDUSTRIOUS`, which stands for *industrapis*. 

For Forestry's trees and butterflies, their genera correspond directly to their real-life counterparts. For example,
the Yellow Ipe tree species, which corresponds to *Handroanthus serratifolius* in real life, belongs to the `ForestryTaxa.GENUS_HANDROANTHUS`.

Although many of the names in `ForestryTaxa` are written in English, you'll see in game that they're actually in Latin.
This is a convention from real-life taxonomy.

You can view all of Forestry's default genera in the [source code on GitHub](https://github.com/thedarkcolour/ForestryCE/blob/d7e58a17486ca60b1539d7ca02d3e9cf745327f6/src/main/java/forestry/api/genetics/ForestryTaxa.java#L84).
(Taxa, the plural of **taxon**, are taxonomic groups.)

## Creating a new genus
A genus may be defined using GenericEvent.defineTaxon. Here's an example of how you might recreate the Infernal genus
used by the Sinister/Fiendish/Demonic bees in KubeJS:
```js
// startup_scripts/my_first_genus.js

const GENUS_INFERNAL = 'diapis'

ForestryEvents.genetics(genetics => {
  genetics.defineTaxon(ForestryTaxa.FAMILY_BEES, GENUS_INFERNAL, genus => {
    genus.setDefaultChromosome(BeeChromosomes.TEMPERATURE_TOLERANCE, ForestryAlleles.TOLERANCE_DOWN_2);
    genus.setDefaultChromosome(BeeChromosomes.ACTIVITY, ForestryAlleles.ACTIVITY_METATURNAL);
    genus.setDefaultChromosome(BeeChromosomes.FLOWER_TYPE, ForestryAlleles.FLOWER_TYPE_NETHER);
    genus.setDefaultChromosome(BeeChromosomes.POLLINATION, ForestryAlleles.POLLINATION_AVERAGE);
  });
});
```

The first argument of `defineTaxon` is the *parent taxon*, which in the case of genera, is a family taxon. For all bees,
the parent family should be `ForestryTaxa.FAMILY_BEES`. For trees and butterflies, the family may vary, so I
recommend checking the real-life taxonomy of those species on Wikipedia or another reliable source. I'll explain later
how to define other taxa such as families, orders, phyla, etc.

The second argument of `defineTaxon` is the *taxon name*, which is the Latin name of the genus. You should make `const`
variables for your taxon names, like I did in the above example with `GENUS_INFERNAL` and like Forestry does with its
`ForestryTaxa` class. Bee genera often end with the *-apis* suffix, which comes from the genus of the real-life Western
Honey bee, *Apis mellifera*.

The third, **optional** argument of `defineTaxon` is a function that customizes the default alleles for members in that
taxon. For example, the Infernal genus sets four default alleles, including the Metaturnal allele. Any species registered
to this genus will now have Metaturnal as its default activity type instead of Diurnal. Look at this snippet from the code
for registering the Sinister species:

```js
ForestryEvents.apiculture(apiculture => {
  apiculture.registerSpecies(ForestryBeeSpecies.SINISTER, GENUS_INFERNAL, SPECIES_SINISTER, false, Color.of('#b3d5e4'))
    // ...
    .setGenome(genome => {
      genome.set(BeeChromosomes.SPEED, ForestryAlleles.SPEED_SLOWER);
      genome.set(BeeChromosomes.LIFESPAN, ForestryAlleles.LIFESPAN_NORMAL);
      genome.set(BeeChromosomes.EFFECT, ForestryAlleles.EFFECT_AGGRESSIVE);
    })
    // ...
});
```

Even though its genome doesn't specify Metaturnal as its activity type, it shows in game as being Metaturnal,
since it's a member of the Infernal Genus.

## Creating other taxa
The `defineTaxon` function can create other types of taxa that aren't necessarily genera. Let's see an example
with trees, defining the taxonomy for a new species, the [sweet orange tree](https://en.wikipedia.org/wiki/Citrus_%C3%97_sinensis).
Although technically a hybrid, we'll treat it as a species for the sake of this example. Wikipedia says its taxonomy is:

Kingdom: 	Plantae  
Clade: 	Tracheophytes  
Clade: 	Angiosperms  
Clade: 	Eudicots  
Clade: 	Rosids  
Order: 	Sapindales  
Family: 	Rutaceae  
Genus: 	Citrus  
Species: 	C. sinensis  

In Botany, classification is done through cladistics (hence the clades) rather than taxonomy. However, some of the clades
are actually taxa that are already used by trees in Forestry, and appear in `ForestryTaxa`. Here's the taxonomy I came
up with:

`ForestryTaxa.KINGDOM_PLANTAE` - The plant kingdom   
 `ForestryTaxa.PHYLUM_FLOWERING_PLANT` - The English name of the angiosperms phylum/clade  
  `ForestryTaxa.CLASS_ROSIDS` - The rosids class, a large group of flowering plants  
   `ForestryTaxa.ORDER_SAPINDALES` - An order of flowering plants including citrus trees, among others  
    `ForestryTaxa.FAMILY_RUTACEAE` - The citrus family  
     `ForestryTaxa.GENUS_CITRUS` - The genus of citrus trees  
      `sinensis` - The species name, derived from the right side of the binomial (scientific name)  

Although this entire taxonomy is already defined by base Forestry (used by Lemon trees) we'll recreate it in KubeJS:
```js
ForestryEvents.genetics(genetics => {
  genetics.defineTaxon(ForestryTaxa.KINGDOM_PLANT, ForestryTaxa.PHYLUM_FLOWERING_PLANT, phylum => {
    phylum.defineSubTaxon(ForestryTaxa.CLASS_ROSIDS, klass => {
      klass.defineSubTaxon(ForestryTaxa.ORDER_SAPINDALES, order => {
	    order.defineSubTaxon(ForestryTaxa.FAMILY_RUTACEAE, family => {
	      family.defineSubTaxon(ForestryTaxa.GENUS_CITRUS);
	    });
	  });
    });
  });
});
```

Just as we did before, we used `defineTaxon` to create a new taxon. However, we also use `defineSubTaxon`,
an elegant way to define entire taxonomies that clearly illustrates the hierarchy of the taxonomic rankings.
We do this instead of repeatedly calling `defineTaxon`, although you could still do that if you wanted to.

*The reason we write `klass` instead of `class` is because `class` is a reserved word with a special meaning in JavaScript,
so we can't use it for variable names.*