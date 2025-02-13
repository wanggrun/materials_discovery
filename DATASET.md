# Materials Discovery: GNoME Datasets

* [GNoME] (#gnome)
* [Convex Hull] (#convexhull)
* [Properties] (#properties)
* [Structures] (#structures)

## GNoME

The GNoME dataset provides ~381,000 novel structures that update the convex hull of known stable materials. Associated files are provided in the public Cloud Bucket defined by ```gs://gdm_materials_discovery``` with the following organization:

```
gdm_materials_discovery
└───gnome_data
│   │   stable_materials_hull.csv
│   │   stable_materials_r2scan.csv
│   │   stable_materials_summary.csv
│   │   by_composition.zip
│   │   by_id.zip
│   │   by_reduced_formula.zip
```

There are two main options for downloading the dataset to a local directory.

* **Command line:** the command line interface can be used to directly download structures from the public bucket via a command of the form ```gsutil -m cp -r gs://gdm_materials_discovery/ data/``` 
* **Python script:** Helper scripts have also been provided using that can be run using ```python download_data_wget.py``` or ```python download_data_cloud.py``` with the optional ```data_dir``` flag in order to determine the output location. The latter may be preferable if the user already has Google Cloud CLI already authorized.

## Convex Hull

First, in ```stable_materials_hull.csv```, we provide a CSV containing the compositions of all novel materials as well as 
corresponding energies. This file is the simplest and most useful for calculating the convex hull energies over all stable materials. New materials can be benchmarked against this file for decomposition energy estimates.

Note, construction of the complete convex hull requires energies from Materials Project (MP), the Open Quantum Materials Database (OQMD), and WBM. In an update to be released shortly, we will the recalculated DFT measurements from entries originating from MP, OQMD, and WBM that allow for the generation of the complete convex hull. Once this is complete, the union with ```stable_materials_hull.csv``` will provide the updated convex hull.

## Properties

Beyond energy, a number of additional properties may be of interest for the novel crystal structures. ```stable_materials_summaries.csv``` provides a number of additional descriptors that may be helpful when processing or looking for crystals with specific properties.

A list of the properties provided and textual descriptions is provided below:

* **Composition:** alphabetically-ordered composition
* **MaterialId:** a unique id corresponding to the entry
* **Reduced Formula:** reduced chemical formula
* **Elements:** chemical system
* **NSites:** number of atoms
* **Volume:** volume in units Å^3
* **Density:** density in units Å^3 / atom
* **Point Group:** assigned point group
* **Space Group:** assigned space group
* **Space Group Number:** assigned space group number
* **Crystal System:** assigned crystal system
* **Corrected Energy:** energy adjusted by MP2020 corrections
* **Formation Energy Per Atom:** normalized energy corrected by reference elements
* **Decomposition Energy Per Atom:** decomposition energy relative to the downloaded Materials Project convex hull
* **Dimensionality Cheon:** dimensionality predicted by Cheon et al. 2017
* **Bandgap:** calculated bandgap
* **Is Train:** in training set for associated machine learning models
* **Decomposition Energy Per Atom All:** distance to convex hull of all entries
* **Decomposition Energy Per Atom Relative:**
distance to convex hull of all entries except for the current
* **Decomposition Energy Per Atom MP:**
distance to convex hull of all entries from Materials Project (including recalculations)
* **Decomposition Energy Per Atom MP OQMD:**
distance to convex hull of all entries from Materials Project + Open Quantum Materials Database (including recalculations)

Additional measurements (e.g. air stability) could be made available across the dataset for any high-throughput searches. Please get in touch.

## Structures

Each of the associated rows of the CSV provided above is associated with a crystal structure. We provide compressed directories of associated CIFs, a standard file format within the materials science community. Note, three versions of the compressed directories exist, where file names allow for lookup by unique identifier, reduced formula, or by composition.

## r^2SCAN

Validation of the associated structures was completed using r^2SCAN. ```stable_materials_r2scan.csv``` provides all r^2SCAN calculations performed on stable materials. Note, stability metrics change with the choice functional (as discussed in the associated paper), so not all released materials remain stable according to this metric.

## Caveats

Due to numerical precision (and errors arising from the computational simulations), we use a threshold of 5e-5 eV as the threshold for determining whether a material is on the convex hull. For all measurements, the provided materials update the convex hull of a snapshotted version of Materials Project and similar databases. Therefore, as more crystals are discovered by the scientific community, the above set may not remain stable.

