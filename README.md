# Detecting Logical Errors in Mathematical Proofs with NLP

## Team Members
- Liam Hu


## One-Sentence Task

**Given a mathematical theorem and a natural-language proof, we will use NLP to classify whether the proof is valid or invalid and, for invalid proofs, identify the type of reasoning error.**


## Illustrative Input and Desired Output

- **Example input:**
  - Theorem: If an integer $n$ is even, then $n^2$ is even.
  - Proof: "Let $n=2k$ for some integer $k$. Then $n^2=2k^2$. Since $2k^2$ is even, $n^2$ is even."
- **Desired output:**
  - {
  "validity": "Invalid",
  "error_type": "Invalid logical or mathematical inference"
}

## Background and Starting Point
We are interested in whether NLP models can recognize logical errors in mathematical proofs rather than merely match mathematical keywords. Our starting point is NaturalProofs (Welleck et al., 2021), a collection of mathematical statements and human-written proofs. Unlike the original work on retrieving references cited in proofs, we will study validity and error classification.

## Intended Output and Success

The project will produce a reproducible Python workflow that takes a theorem–proof pair and returns:
1. Validity classification: Valid or Invalid.
2. Error classification (if invalid): one category from a predefined taxonomy, initially including:
   - Invalid inference: a conclusion does not follow from the preceding statement.
   - Unjustified assumption: the proof relies on an assumption not established by the theorem or previous steps.
   - Missing essential justification: a necessary argument is absent, making the submitted proof insufficient; routine omitted algebra does not count.
   - Misapplied theorem / unmet conditions: a result is invoked without its required assumptions.
Circular reasoning is a possible extension. “Invalid” refers to a proof that does not adequately establish its theorem, not necessarily a false theorem. We will finalize annotation rules before training.

**Success statement:** The system will distinguish valid and flawed proofs and identify common errors on manually reviewed test cases, with an honest analysis of limitations.

## Preferred Data

### Corpus
NaturalProofs contains approximately 32,000 proofs, 32,500 theorem statements, and related definitions and references. One project item will be a theorem–proof pair, using theorem content, proof content, and source metadata. NaturalProofs does not contain our validity or error-type labels; we must create them. Published proofs are candidate positive examples, not formally verified ground truth.

### Source and Access
- Dataset or corpus name: NaturalProofs: Mathematical Theorem Proving in Natural Language (2021).
- Original source organization or creator: Compiled by Sean Welleck and collaborators from community-contributed ProofWiki material, the Stacks Project, and two mathematical textbooks.
- Original collection process: Researchers parsed ProofWiki XML (November 2020), Stacks Project LaTeX (April 2021), and LaTeX sources from real analysis and number theory textbooks into a unified JSON schema.
- Direct link: https://github.com/wellecks/naturalproofs
- Dataset paper: https://arxiv.org/abs/2104.01112
- Access method: Public GitHub repository and downloadable preprocessed JSON files; the repository includes preprocessing notebooks and a download script.
Access, licensing, privacy, or redistribution considerations: GitHub code is MIT-licensed; underlying texts have source-specific licenses (e.g., ProofWiki CC BY-SA 3.0, Stacks GNU FDL, Real Analysis CC BY-NC-SA 3.0). We will verify terms and attribution before redistributing text. No sensitive personal data is expected.

### Examples

The following examples are based on Theorem ID 0: "Closed Form for Triangular Numbers" from the NaturalProofs ProofWiki dataset. This theorem has multiple proofs in the original dataset.

**Theorem:** For every positive integer $n$,

$$
T_n = \sum_{i=1}^{n} i = \frac{n(n+1)}{2}.
$$
#### Example 1 — Valid Proof (Original, Simplified)
- Proof: Let $S=1+2+\cdots+n$ Writing this sum in both forward and reverse order gives $\(2S=n(n+1)\)$. Dividing both sides by 2, we obtain $S=\frac{n(n+1)}{2}$.
- Desired output:
  - Validity: Valid
  - Error Type: None
#### Example 2 — Invalid Logical/Mathematical Inference (Modified)
- Proof: Let $S=1+2+\cdots+n$. Pairing the terms gives $$2S=n(n+1)$$. Therefore, $S=n(n+1)$.
- Desired output:
  - Validity: Invalid
  - Error Type: Invalid Inference
- Reason: The final conclusion incorrectly omits division by 2.
#### Example 3 — Missing Essential Justification (Modified)
- Proof: When $n=1$, we have $T_1=1=\frac{1(1+1)}{2}$. Therefore, the formula holds for every positive integer $n$.
- Desired output:
  - Validity: Invalid
  - Error Type: Missing Essential Justification
- Reason: Verifying only the base case does not establish the formula for all positive integers.
Note: Example 1 is adapted from an existing proof in NaturalProofs. Examples 2 and 3 are synthetically modified to illustrate potential error categories. NaturalProofs does not provide validity or error-type labels, so modified examples must be reviewed before being used for training or evaluation.


### Fit with the Task

NaturalProofs provides theorem–proof pairs but not validity labels. We will extract paired text, remove non-content markup while preserving mathematics, record source IDs, and filter ambiguous entries. We will construct negative examples using controlled error injection (e.g., an incorrect equation or removed condition), then review and discard ambiguous or still-valid edits.

## Feasibility and Immediate Next Steps

- **Minimum viable project:** Build a checked dataset of short valid/invalid proofs, train binary and error-type classifiers, and report results in a poster.
- **Data-access check:** The repo and schema are accessible, and the team has already downloaded and inspected the JSON data. 
- **Largest current risk:** No target labels exist for training purpose, and the might requires long hours of manual labeling
- **Backup plan:** Priorize binary classification if necessary
- **Next two actions:**
  1. Clean the dataset, because it's huge and contain many extraneous information. OCT 25
  2. Create invalid proofs using the proofs in the dataset. Create target labels. OCT 30

## Possible Text-Analysis Method and Evaluation

Planned workflow: 
(1) Extract and clean theorem–proof pairs while retaining mathematical notation; 
(2) create manually checked valid and synthetically flawed examples; 
(3) compare TF-IDF + Logistic Regression with a fine-tuned BERT-family classifier for binary validity and error-type prediction; 
(4) report precision, recall, macro-F1, and confusion matrices for each task;

## Questions for the Instructor

1. Is this realistic? Would the final results be significant? Since mathematical proofs contain many notations and symbols beyond natural language, models might not be able to correctly embed and "understand" the proofs. Instead, they might rely on shortcut learning by simply looking for keywords to perform the classification tasks. This is also why I decided not to do proof-method classification.

2. Does this project meet the course project requirements?

## AI-Use Statement

I used ChatGPT to help me find specific datasets, understand the NaturalProofs research paper, and write this README. The initial project idea and main direction were my own, although AI also helped me clarify and finalize the idea and project scope. I reviewed the AI-generated suggestions and checked the relevant information against the original paper and examples from the downloaded dataset.

## References
- Welleck, S., Liu, J., Le Bras, R., Hajishirzi, H., Choi, Y., & Cho, K. (2021). NaturalProofs: Mathematical Theorem Proving in Natural Language. NeurIPS Datasets and Benchmarks. https://arxiv.org/abs/2104.01112
- NaturalProofs code and dataset: https://github.com/wellecks/naturalproofs
