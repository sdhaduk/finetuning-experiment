# ReadME  

The packages and versions I used are listed in pyproject.toml under dependencies

## model_fine_tuning.ipynb
This notebook contains the process of setting up parameters, initializing models, initializing the dataset, fine-tuning the models and saving them.

## model_eval.ipynb
This notebook contains the process of evalutating the fine-tuned models. It includes loading the saved models, extracting responses from each model, and feeding those responses to gemma3:4b model on Ollama.
