#  Vision-Language-In-Context-Learning-Driven-Few-Shot-Visual-Inspection-Model (VISAPP 2025)

## Contents
- [Setup](#Setup)
- [Dataset](#Dataset)
- [Train](#train)
- [Evaluation](#evaluation)
- [Acknowledgement](#acknowledgement)

## Setup

1. preprocess
```bash
sudo apt install g++
git clone https://github.com/ia-gu/Vision-Language-In-Context-Learning-driven-Few-Shot-Visual-Inspection-Model
cd Vision-Language-In-Context-Learning-driven-Few-Shot-Visual-Inspection-Model
```

2. Install Package
```Shell
conda create -n vicl python=3.10 -y
conda activate vicl
pip install --upgrade pip  # enable PEP 660 support
pip install -e .
pip install cog
```

3. Install additional packages for training cases
```Shell
pip install -e ".[train]"
pip install flash-attn --no-build-isolation
```

4. (Optional) Replace transformers.trainer.trainer.py with eval_utils/trainer.py if you want to save checkpoints in every n epochs

5. Download contents
- [Model weights before fine-tuning](https://huggingface.co/mucai/vip-llava-7b/tree/main)
- Getting model weights after fine-tuning, contact us(vpn@cv.info.gifu-u.ac.jp) or implement ```run.sh``` to train model samely as us.

## Dataset
Our training dataset is [here](training_dataset). These images are collected from the Web, and scraped by hand. 

## Train
Our base model is [ViP-LLaVA](https://github.com/WisconsinAIVision/ViP-LLaVA), and we use the 3rd stage weight. Details are following: 

>ViP-LLaVA training consists of three stages: (1) feature alignment stage: use our 558K subset of the LAION-CC-SBU dataset to connect a *frozen pretrained* vision encoder to a *frozen LLM*; (2) visual instruction tuning stage: 665K image-level instruction data from LLaVA-1.5 and 520K region-level instruction data using visual prompts. (3) finetuning on GPT-4V data. 

>LLaVA is trained on 8 A100 GPUs with 80GB memory. To train on fewer GPUs, you can reduce the `per_device_train_batch_size` and increase the `gradient_accumulation_steps` accordingly. Always keep the global batch size the same: `per_device_train_batch_size` x `gradient_accumulation_steps` x `num_gpus`.

Cited by [ViP-LLaVA](https://github.com/WisconsinAIVision/ViP-LLaVA).

Our additional fine-tuning and evaluation are on 8*RTX6000Ada GPUs with 48GB memory

### Hyperparameters
Hyperparameters used fine-tuning are provided below.

| Hyperparameter | Global Batch Size | Learning rate | Epochs | Weight decay |
| --- | ---: | ---: | ---: | ---: | ---: |
|    | 32 | 1e-4 | 300 | 0 |

## Evaluation
We evaluate our model on three dataset, [MVTec-AD](https://www.mvtec.com/company/research/datasets/mvtec-ad) and [VisA](https://github.com/amazon-science/spot-diff). 
Use ```predict_*.py``` for evaluation and ```calcurate_result_*.ipynb``` for getting MCC and F1-score result.

Also, use ```prepare_*.ipynb``` to prepare prompts for each dataset.


## Citation

```bibtex

@inproceedings{VisionLanguageInContextLearningDrivenFewShotVisualInspectionModel,
  author      = {Ueno, S.; Hayashi, Y.; Nakatsuka, S.; Yamada, Y.; Aizawa, H. and Kato, K},
  title       = {Vision-Language In-Context Learning Driven Few-Shot Visual Inspection Model},
  booktitle   = {20th International Joint Conference on Computer Vision, Imaging and Computer Graphics Theory and Applications},
  year      = {2025},
  pages     = {253--260},
  isbn      = {978-989-758-728-3},
  issn      = {2184-4321}
}
```

## Acknowledgement

- [ViP-LLaVA](https://github.com/WisconsinAIVision/ViP-LLaVA): the vision-language model, which has strong multimodality and visual prompts recognition. 

## Caution
**Usage and License Notices**: All of the contents here is intended and licensed for research use only. Also, please follow the license agreement of ViP-LLaVA, LLaMA, Vicuna and GPT-4.

