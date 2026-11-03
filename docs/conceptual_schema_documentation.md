# Conceptual Data Schema

## How to read it

* **Rectangles (Entities):** biological or physical data categories (e.g., an RNA strand, a chemical), in the future - main tables in the database
* **Diamonds (Relationships):** how two entities interact biologically or logically. Many-to-Many relationships will become connecting tables
* **Circles (Attributes):** specific data fields belonging to an entity or relationship
* **Numbers (min, max Cardinality):** defines the rules for data entry. Read from the perspective of one entity looking through the relationship to the other
    * *Example: 0,n means a record can exist with zero connections (optional), or it can have many connections*
    * *Example: 1,1 means a record must have exactly one connection (mandatory)*

## Core Entities and their Attributes

### RNA

* **RIBOCEPTOR:** the parent RNA transcript
    * Attributes:
        * `name`: common name of the RNA
        * `full_sequence`: complete nucleotide string
        * `genomic_context`: location in the genome (e.g., 5' UTR)
        * `validation_status`: indicates if it is experimentally verified or computationally predicted

* **STRUCTURAL MOTIF:** the sensory domain (aptamer). Separated from the Riboceptor so that the database can store Tandem Riboswitches (one RNA strand containing multiple independent sensing pockets)
    * Attributes:
        * `rfam_id`: identifier from the Rfam database
        * `sequence_fasta`: the raw nucleotide sequence of just the sensor
        * `dot_bracket`: 2D secondary structure notation
        * `start_coord`, `end_coord`: exact positions on the parent strand

* **RIBOZYME:** the catalytic domain capable of RNA cleavage
    * Attributes:
        * `class`: Type of ribozyme (e.g., Hammerhead)
        * `catalytic_activity`: The chemical reaction performed
        * `dot_bracket`: 2D secondary structure notation
        * `start_coord`, `end_coord`: Exact positions on the parent strand

### Triggers and Helpers

* **STIMULUS:** the environmental trigger that activates the motif
    * Attributes:
        * `name`: specific name of the trigger
        * `category`: broad class for filtering (e.g., Metabolite, Ion)
        * `pubchem_id`: standard chemical identifier

* **COFACTOR:** An auxiliary molecule strictly required for the structural motif to fold or function properly
    * Attributes:
        * `name`: name of the helper molecule
        * `chebi_id`: standard chemical identifier

### Context

* **REGULATORY MECHANISM:** The biological action executed by the RNA upon activation
    * *Example: "Translation inhibition", "Transcription attenuation"*
    * Attributes:
        * `mechanism_type`: the main category of action
        * `description`: Short textual explanation (not that important, could be omitted)

* **TAXONOMY:** The biological organism or metagenomic source where the RNA is found. Anchored to NCBI to ensure global standardization
    * *Example: Escherichia coli*
    * Attributes:
        * `name`: scientific Latin name
        * `ncbi_taxid`: official identifier from NCBI

* **TAXONOMY RANK:** the hierarchical level of the taxonomy entry
    * *Example: "Species", "Genus", "Phylum"*
    * Attributes:
        * `name`: The specific rank level

### Experimental Evidence

* **3D_STRUCTURE:** spatial models of the folded RNA
    * *Example: PDB entry 7ELP*
    * Attributes:
        * `pdb_id`: 4-character code from the Protein Data Bank
        * `resolution`: quality of the model (numeric)
        * `method`: how the structure was solved (e.g., X-ray)