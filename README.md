🧬 Enhanced Biosecurity Screening Pipeline
📝 Overview
This Python script is an enhanced bioinformatics pipeline designed for screening a raw DNA sequence for potentially dangerous protein sequences. It uses Biopython for sequence translation and offers two distinct methods for predicting protein function (GO Terms): a fast local heuristic or the comprehensive InterProScan API web service. It also includes optional integration for ESMFold to predict 3D protein structure confidence. The final step performs a risk classification based on a set of known dangerous Gene Ontology (GO) terms.

✨ Key Features

Six-Frame Translation: Accurately translates the DNA sequence in all six possible reading frames to find all potential protein sequences.


Protein Filtering: Retains only protein segments longer than 30 amino acids that consist of standard amino acids.

Flexible GO Term Prediction:


InterProScan API (Recommended/Accurate): Uses a robust external web service to predict GO terms.


Local Heuristic (Fast/Offline): Employs simple sequence motif checks (e.g., high Cysteine, basic amino acid content, hydrophobic stretches) for quick, offline prediction.


Optional 3D Structure Prediction: Integrates ESMFold to predict 3D protein structure confidence (pLDDT score).


Risk Classification: Screens predicted GO terms against a defined list of DANGEROUS_GO_TERMS to classify the protein as "DANGEROUS" or "SAFE".

🚀 Getting Started
Prerequisites
You will need Python 3.x and a working Python environment. For best performance, especially with ESMFold, a GPU with CUDA is highly recommended.

Installation
The required libraries must be installed before running the script.

Usage
Save the provided code as a Python file (e.g., alpha_screen.py).

Modify the SAMPLE_DNA variable with your target sequence.

Choose your screening method in the "Example Usage" section at the bottom of the script.

Method Options:
Example of Running the Fast Heuristic (Default):

⚙️ Core Logic Breakdown
Function Prediction (predict_go_terms_interpro)
This function interacts with the European Bioinformatics Institute (EBI) InterProScan web service.


Submission: The protein sequence is submitted to the API with a request for GO terms ('goterms': 'true') and specific fast applications (Pfam, TIGRFAM, Gene3D).


Polling: The script continuously checks the job status every 5 seconds until it returns 'FINISHED'.


Retrieval: Once finished, it retrieves the results in JSON format and parses out all unique GO term IDs found by the service.

Function Prediction (predict_go_terms_heuristic)
This function applies simplified, fast checks to predict basic protein function:


High Cysteine Content (> 15%) suggests a possible toxin activity (GO:0090729).

Presence of hydrophobic stretches (e.g., 70% hydrophobic residues in a 20-AA window) suggests an integral membrane component (GO:0016021).


High Basic Amino Acid Content (> 15% K or R) suggests DNA binding (GO:0003677).

⚠️ Configuration Notes

Email for InterPro: If using use_interpro=True, you must provide a valid email address for the API submission (e.g., email="your_email@example.com"), as required by EBI.

ESMFold Limits: ESMFold is computationally intensive and has sequence length limits. The script skips ESMFold for sequences longer than 400 amino acids to prevent extremely long runtimes and tokenization issues.

Dangerous GO Terms: The set DANGEROUS_GO_TERMS is the core of the risk assessment. Modify this set to tailor the pipeline to your specific biosecurity concerns.
