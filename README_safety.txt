README.txt
==========

Safety Evaluation on MorningStar
--------------------------------

This project runs safety evaluation for the fine-tuned Gemma models on the University of Ottawa MorningStar server using Jupyter notebooks.

The safety evaluation was run in:

    /home/uottawa.o.univ/hchan123/Hournor_Project

Main notebooks:

    safety_270m.ipynb
    safety_1b-pt.ipynb
    safety_4b-pt.ipynb


1. Required Files
-----------------

Before running the notebooks on MorningStar, upload all required files to the project folder.

Required files include:

    1. The evaluated model folders
       Example:
           gemma-3-270m
           gemma-3-1b
           gemma-3-4b
           checkpoint folders such as checkpoint-1000, checkpoint-2000, etc.

    2. Safety benchmark files
       Example:
           SORRY-Bench
           StrongREJECT
           Do-Not-Answer, if used

    3. Judge or evaluator models
       Example:
           SORRY-Bench judge model
           StrongREJECT evaluator model
           DNA classifier, if used

    4. The Jupyter notebooks
       Example:
           safety_270m.ipynb
           safety_1b-pt.ipynb
           safety_4b-pt.ipynb


2. MorningStar Environment
--------------------------

The evaluation was run using MorningStar Jupyter with:

    Python: 3.10.12
    PyTorch: 2.7.0
    CUDA available: True
    CUDA: 12.8
    GPU: NVIDIA H100 PCIe MIG 1g.10gb

Important package versions:

    transformers: 5.1.0
    peft: 0.18.1
    datasets: 4.5.0
    accelerate: 1.12.0

The MorningStar node shows 2 x NVIDIA H100 PCIe GPUs in nvidia-smi, but the Jupyter session uses a MIG 1g.10gb GPU slice.


3. How to Run on MorningStar
----------------------------

Step 1: Open MorningStar Jupyter.

Step 2: Go to the project folder:

    /home/uottawa.o.univ/hchan123/Hournor_Project

Step 3: Open the notebook for the model you want to evaluate:

    safety_270m.ipynb      for the 270M model
    safety_1b-pt.ipynb     for the 1B PT model
    safety_4b-pt.ipynb     for the 4B PT model

Step 4: Run the notebook cells from top to bottom.

Each notebook should:

    1. Load the local model.
    2. Load the local safety benchmark.
    3. Generate model responses.
    4. Run the safety judge or evaluator.
    5. Save the judged or scored results.
    6. Calculate the final safety metrics.
    7. Generate plots, if needed.


4. Offline Mode
---------------

MorningStar may not have normal internet access. Therefore, models and datasets should be loaded from local folders.

If needed, add this near the beginning of the notebook:

    import os

    os.environ["TRANSFORMERS_OFFLINE"] = "1"
    os.environ["HF_HUB_OFFLINE"] = "1"
    os.environ["HF_DATASETS_OFFLINE"] = "1"

This prevents HuggingFace from trying to download files online.


5. Checking GPU
---------------

In a Jupyter notebook cell, use:

    !nvidia-smi

In the MorningStar terminal, use:

    nvidia-smi

To check whether PyTorch can access the GPU, run:

    import torch

    print(torch.cuda.is_available())
    print(torch.cuda.get_device_name(0))


6. Expected Outputs
-------------------

The notebooks should save result files such as:

    *_results.jsonl
    *_judged.jsonl
    *_scored.jsonl
    safety_summary.csv
    checkpoint_safety_summary.csv
    sorrybench_refusal_rate_plot.png
    strongreject_safe_rate_plot.png

The exact output names may depend on the notebook.


7. Notes
--------

All models, benchmark datasets, and judge models must be downloaded before uploading to MorningStar. The evaluation should not rely on downloading files during runtime.

For reproducibility, keep the same decoding settings across all models and checkpoints, such as:

    max_new_tokens
    do_sample
    temperature
    top_p
    repetition_penalty

This allows the safety results of the 270M, 1B, and 4B models to be compared fairly.
