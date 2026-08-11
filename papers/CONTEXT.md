# Stage Contract: Literature Processing & Paper Intake (Layer 2)

Defines the explicit inputs, processing workflow, and outputs for ingesting and extracting research papers into the literature knowledge base.

---

## 1. Contract Inputs

- **Layer 4 Working Artifact:** Raw PDF/text file in `papers/inbox/paper.pdf`, or an online paper reference (arXiv ID, DOI, URL).
- **Layer 3 Reference Template:** `papers/templates/extraction_prompt.md`
- **Layer 3 Code Convention:** `_config/code_conventions.md` (Section 1: Clean Cloning Protocol)
- **Layer 0 Identity Rule:** `GEMINI.md` (Rule 8: Factual Grounding & Zero Hallucination)

---

## 2. Standard Processing Workflow

### Step 1: Paper Sourcing & Reading
1. Open and inspect the incoming document (from `papers/inbox/` or web search tools).
2. Read the title, abstract, and architecture to extract the primary model name and core concept.

### Step 2: Auto-Naming & Directory Setup
1. Formulate a concise, standardized directory slug (e.g., `2024_mamba`, `swin_transformer`, `med_sam`).
2. Create directory `papers/<concise_paper_name>/`.
3. Move or save the source PDF to `papers/<concise_paper_name>/paper.pdf`.

### Step 3: Optional Repository Intake (`/repos/`)
If the paper provides an official open-source code repository link:
1. Determine `<repo_name>`.
2. Execute clean shallow-clone command:
   ```bash
   git clone --depth 1 <paper_repository_url> repos/<repo_name>
   rm -rf repos/<repo_name>/.git
   ```
3. Mark `/repos/<repo_name>/` as clean, read-only reference material.

### Step 4: Lossless Technical Extraction & Strict Grounding
1. Apply `papers/templates/extraction_prompt.md` to extract LaTeX math, loss structures, tensor dimensions, hyperparameters, and benchmark metrics.
2. **Grounding Directive:** Extract ONLY facts explicitly stated in the paper/code. Mark any unstated parameters as `[Not Specified in Paper]` or ask the user for clarification. Never guess or hallucinate missing values.
3. If a repository was cloned, map key mathematical claims to exact file paths and class names inside `/repos/<repo_name>/`.

---

## 3. Contract Outputs

- **Structured Note:** `papers/<concise_paper_name>/summary.md`
- **Source Artifact:** `papers/<concise_paper_name>/paper.pdf`
- **Reference Codebase (Optional):** Clean static folder under `/repos/<repo_name>/`
