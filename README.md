# Artificial neural network as a natural tool in solution of variational problem in hydrodynamics

This repository is the official implementation of Artificial neural network as a natural tool in solution of variational problem in hydrodynamics.

## Requirements

Install requirements.txt in a
[**Python>=3.9.12**](https://www.python.org/) environment, including
[**PyTorch>=2.0.0**](https://pytorch.org/get-started/locally/).

To install requirements:

```setup
pip install -r requirements.txt
```

## Training and Evaluation

To train and evaluate the model(s) in the paper, open file **main.ipynd** in jupyter notebook and run command **"Run All"**. Section **"Сonstants for selecting inputs"** contain all the hyperparameters required for reproducing the experiment results. The results presented in Table below were obtained for 2D flows between 2 parallel plates (**case 1**). The following parameters should be variated: **SIZE - 128, 256; FLUID_TYPE - "non", "new"; MODEL_NAME - "MLP", "Unet"**. **Cases 2-5** are for the more complicated flow domains. Section **"Training"** refers to training and sections **"Visualization of errors" and "Calculate error"** correspond to comparison with known analytical solutions (parallel plates or pipe only). 

### Case 1 - 2D Parllel plates:

	MASK_INDEX = 3 # Index of mask (3D: {0: pipe}, 2D: {0: Aorta, 1: Liver, 2: Parallel plates + cilynder, 3: Parallel plates})
	USE_3D = False # Use or not 3D
	MODEL_NAME = "MLP" # Model type (MLP or Unet)
	SIZE = 128 # Number of points for each X
	L = [0.016] * 3 # Domain size
	FLUID_TYPE = 'non' # Fluid type (new on non)
	Q_3 = 0.000004 if FLUID_TYPE == 'new' else 2.57e-06 # Flow rate
	MODEL_NAME_FINAL = f'{MODEL_NAME}_{SIZE}_{L[0]}_{FLUID_TYPE}'
	CALC_ERROR = True # Calculate or not errors

### Case 2 - 2D Aorta:

	MASK_INDEX = 0 # Index of mask (3D: {0: pipe}, 2D: {0: Aorta, 1: Liver, 2: Parallel plates + cilynder, 3: Parallel plates})
	USE_3D = False # Use or not 3D
	MODEL_NAME = "Unet" # Model type (MLP or Unet)
	SIZE = 128 # Number of points for each X
	L = [0.035, 0.02, 0.035] # Domain size
	FLUID_TYPE = 'new' # Fluid type (new on non)
	Q_3 = 2e-6 # Flow rate
	MODEL_NAME_FINAL = f'{MODEL_NAME}_{SIZE}_{L[0]}_{FLUID_TYPE}'
	CALC_ERROR = False # Calculate or not errors

### Case 3 - 2D Liver:

	MASK_INDEX = 1 # Index of mask (3D: {0: pipe}, 2D: {0: Aorta, 1: Liver, 2: Parallel plates + cilynder, 3: Parallel plates})
	USE_3D = False # Use or not 3D
	MODEL_NAME = "Unet" # Model type (MLP or Unet)
	SIZE = 128 # Number of points for each X
	L = [0.15, 0.025, 0.15] # Domain size
	FLUID_TYPE = 'new' # Fluid type (new on non)
	Q_3 = 4e-5 # Flow rate
	MODEL_NAME_FINAL = f'{MODEL_NAME}_{SIZE}_{L[0]}_{FLUID_TYPE}'
	CALC_ERROR = False # Calculate or not errors

### Case 4 - 2D Parllel plates + cylinder:

	MASK_INDEX = 2 # Index of mask (3D: {0: pipe}, 2D: {0: Aorta, 1: Liver, 2: Parallel plates + cilynder, 3: Parallel plates})
	USE_3D = False # Use or not 3D
	MODEL_NAME = "Unet" # Model type (MLP or Unet)
	SIZE = 128 # Number of points for each X
	L = [0.016] * 3 # Domain size
	FLUID_TYPE = 'new' # Fluid type (new on non)
	Q_3 = 1e-5 # Flow rate
	MODEL_NAME_FINAL = f'{MODEL_NAME}_{SIZE}_{L[0]}_{FLUID_TYPE}'
	CALC_ERROR = False # Calculate or not errors

### Case 5 - 3D Pipe:

	MASK_INDEX = 0 # Index of mask (3D: {0: pipe}, 2D: {0: Aorta, 1: Liver, 2: Parallel plates + cilynder, 3: Parallel plates})
	USE_3D = True # Use or not 3D
	MODEL_NAME = "MLP" # Model type (MLP or Unet)
	SIZE = 128 # Number of points for each X
	L = [0.064] * 3 # Domain size
	FLUID_TYPE = 'new' # Fluid type (new on non)
	Q_3 = 6.39e-06 # Flow rate
	MODEL_NAME_FINAL = f'{MODEL_NAME}_{SIZE}_{L[0]}_{FLUID_TYPE}'
	CALC_ERROR = True # Calculate or not errors


## Results

Our model achieves the following performance :

| Model | SIZE |  Fluid type   | Simulation time, sec | $max(v_3)$ error, % | $mean(v_3)$ error, % |
| ----  | ---- | ------------- | -------------------- | ------------------- | -------------------- |
| MLP   | 128  | Newtonian     | 150                  | 4.57                | 4.37                 |
| MLP   | 128  | Non-Newtonian | 140                  | 5.22                | 2.14                 |
| MLP   | 256  | Newtonian     | 350                  | 2.83                | 1.67                 |
| MLP   | 256  | Non-Newtonian | 360                  | 2.15                | 1.41                 |
| U-Net | 128  | Newtonian     | 400                  | 2.65                | 5.34                 |
| U-Net | 128  | Non-Newtonian | 170                  | 2.31                | 6.72                 |
| U-Net | 256  | Newtonian     | 600                  | 0.76                | 4.8                  |
| U-Net | 256  | Non-Newtonian | 700                  | 1.31                | 4.89                 |