# DeepLearningwAudio
Deep Learning Task involving CNNs and UrbanSound8k dataset

I pulled .wav files from my machine, so to run the code on your machine, please access the dataset at https://urbansounddataset.weebly.com/

## Preprocessing Comparison:

| Model | Run Time   | Accuracy |  
|----------|------------------|-------------|  
| Log Mel  | 5 min  | 47% |  
| STFT     | 14 min     | 51% |  
| CQT      | 4.5 hrs | 50% |  
| MFCC+Delta | 9.5 hrs      | 42% |  
| MFCC    | 10.25 hrs      | 44% | 

Since the Spectrogram (STFT) had the best accuracy on the simple model and a reasonable runtime, I will proceed with this preprocessing method and tune model to try to improve performance.  See tuning parameters in funciton below.  Depending on runtime I will tune some of these and possibly batch size and/or number of epochs.  This model tuning will be performed in a separate notebook, UrbanSoundModelTune.ipynb

## Model Results:

Here are the results of the model tuning.  Base model refers to create_cnn_model run with default arguments.  Please scroll below to see code and more detailed performance of each model.
| Model | Accuracy    | Notes | Next Steps |
|----------|------------------|-------------|----------|  
| 1        | 35%        | Base model, 10 epochs, Batch size = 32 | Doesn't seem to have converged--> increase epochs |  
| 2        | 37% | Base model, 20 epochs, Batch size = 32 | Increase batch size to see stabilize model |
| 3        | 38%        | Base model, 20 epochs, Batch size = 64 |  Model ran faster, still didn't converge |
| 4        | 45%        | Base model, 20 epochs, Batch size = 128 | Larger batch size-->significant improvement, still not converged | 
| 5        | 43%    | Base model, 30 epochs, Batch size = 128 | Model overfit at 30 epochs, slow lr, remove dropout | 
| 6        | 46%          | No Dropout, 25 epochs, Batch size = 128 lr = 0.0001 | closest to simple model acc, slow lr more, smaller batches for stability | 
| 7        | 43%       | No Dropout, 25 epochs, Batch size = 64, lr = 0.00001 | overfit, add back dropout | 
| 8        | 41%            | Included Dropout but adjusted to .25, .25, .25, 25 epochs, Batch size = 64, lr = 0.00001 | didn't converge, more epochs | 
| 9        | 48%     | .25, .25, .25 Dropout, 60 epochs, Batch size = 64, lr = 0.00001 |  still didn't converge, more epochs |
| 10        | 49%     | .25, .25, .25 Dropout, 80 epochs, Batch size = 64, lr = 0.00001 |  still didn't converge, more epochs |
| 11        | 50%     | .25, .25, .25 Dropout, 150 epochs, Batch size = 64, lr = 0.00001 |  overfit a little, run with log-mel |
| 12        | 54%     | .25, .25, .25 Dropout, 150 epochs, Batch size = 64, lr = 0.00001, log-mel |  much faster (about 1/3 of the runtime of spectrogram), didn't converge, more epochs |
| 13        | 56%     | .25, .25, .25 Dropout, 200 epochs, Batch size = 64, lr = 0.00001, log-mel |  insufficient RAM to run more epochs | 
