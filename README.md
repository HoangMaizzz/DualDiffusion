# DualDiffusion

We propose DualDiffusion, a speculative decoding frame- work that combines both approaches: a lightweight KV-cached MDLM drafts multiple denoising steps rapidly, while a bidirectional MDLM verifies outputs using full context.

## Package Management
This project uses `uv` for package management. To install the required dependencies, run the following command:
```bash
uv sync
```

## Core Components

### `dual_pipeline.py`
This file contains the core orchestration logic for the Dual Diffusion pipeline. The main function, `dual_diffusion_generate`, manages the entire process:
1.  **Drafting:** It begins by using a fast drafter model to generate a sequence of tokens.
2.  **Verification:** The drafted sequence is then passed to a more powerful verifier model.
3.  **Comparison:** A verification algorithm (from `verification_algos.py`) is used to compare the outputs of the two models.
4.  **Remasking:** Based on the comparison, some tokens may be "re-masked" to be generated again in the next iteration.
5.  **Iteration:** The process repeats, refining the generated sequence with each pass.

### `verification_algos.py`
This file provides various strategies for comparing the outputs of the drafter and verifier models. These functions determine which tokens are accepted and which should be re-generated. Key algorithms include:
-   `exact_match_verification`: Remasks tokens where the drafter and verifier outputs disagree.
-   `confidence_threshold_verification`: Remasks tokens if the verifier's confidence is below a certain threshold.
-   `trust_verifier`: Simply accepts the verifier's output, completing the generation in a single pass.

## Testing
To run the test pipeline, you can use the `test_pipeline.ipynb` notebook. Make sure you have a Jupyter environment installed and running. Open and execute the cells in the notebook to see the pipeline in action.
```bash
jupyter notebook test_pipeline.ipynb
```