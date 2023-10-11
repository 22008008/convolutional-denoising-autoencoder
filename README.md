### Convolutional Autoencoder for Image Denoising

## AIM:

```
To develop a convolutional autoencoder for image denoising application.
```

### Problem Statement and Dataset:

```
The image dataset is given and the model must denoise the images and show it with better quality and remove the unwanted noises and learn to show the better version of the images.
```

### Convolution Autoencoder Network Model:

![image](https://github.com/22008008/convolutional-denoising-autoencoder/assets/118343520/d8ee65db-0dc8-47fc-8e1d-98dbd579ddfc)


### DESIGN STEPS:

Step 1:

Import the necessary libraries and dataset.

Step 2:

Load the dataset and scale the values for easier computation.

Step 3:

Add noise to the images randomly for both the train and test sets.

Step 4:

Build the Neural Model using Convolutional, Pooling and Up Sampling layers. Make sure the input shape and output shape of the model are identical.

Step 5:

Pass test data for validating manually.

Step 6:

Plot the predictions for visualization.

### PROGRAM:

```
Developed by : Sri Ranjani Priya.P
Register Number : 212222220049
```

```
from tensorflow import keras
from tensorflow.keras import layers
from tensorflow.keras import utils
from tensorflow.keras import models
from tensorflow.keras.datasets import mnist
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
(x_train, _), (x_test, _) = mnist.load_data()
x_train.shape
x_train_scaled = x_train.astype('float32') / 255.
x_test_scaled = x_test.astype('float32') / 255.
x_train_scaled = np.reshape(x_train_scaled, (len(x_train_scaled), 28, 28, 1))
x_test_scaled = np.reshape(x_test_scaled, (len(x_test_scaled), 28, 28, 1))
noise_factor = 0.5
x_train_noisy = x_train_scaled + noise_factor * np.random.normal(loc=0.0, scale=1.0, size=x_train_scaled.shape) 
x_test_noisy = x_test_scaled + noise_factor * np.random.normal(loc=0.0, scale=1.0, size=x_test_scaled.shape) 

x_train_noisy = np.clip(x_train_noisy, 0., 1.)
x_test_noisy = np.clip(x_test_noisy, 0., 1.)
n = 10
plt.figure(figsize=(20, 2))
for i in range(1, n + 1):
    ax = plt.subplot(1, n, i)
    plt.imshow(x_test_noisy[i].reshape(28, 28))
    plt.gray()
    ax.get_xaxis().set_visible(False)
    ax.get_yaxis().set_visible(False)
plt.show()
input_img = keras.Input(shape=(28, 28, 1))

x = layers.Conv2D(32, (3,3), activation='relu',padding ='same')(input_img)
x = layers.MaxPool2D((2,2), padding = 'same')(x)
x = layers.Conv2D(32, (3,3), activation='relu',padding ='same')(x)
encoded = layers.MaxPooling2D((2, 2), padding='same')(x)

# Encoder output dimension is ## Mention the dimention ##

x = layers.Conv2D(32, (3,3), activation='relu',padding ='same')(encoded)
x = layers.UpSampling2D((2,2))(x)
x = layers.Conv2D(32, (3,3), activation='relu',padding ='same')(x)
x = layers.UpSampling2D((2,2))(x)
decoded = layers.Conv2D(1, (3, 3), activation='sigmoid', padding='same')(x)

autoencoder = keras.Model(input_img, decoded)
autoencoder.summary()
autoencoder.compile(optimizer='adam', loss='binary_crossentropy')
autoencoder.fit(x_train_noisy, x_train_scaled,
                epochs=2,
                batch_size=128,
                shuffle=True,
                validation_data=(x_test_noisy, x_test_scaled))
metrics = pd.DataFrame(autoencoder.history.history)
metrics.head()
metrics[['loss','val_loss']].plot()
decoded_imgs = autoencoder.predict(x_test_noisy)
n = 10
plt.figure(figsize=(20, 4))
for i in range(1, n + 1):
    
    ax = plt.subplot(3, n, i)
    plt.imshow(x_test_scaled[i].reshape(28, 28))
    plt.gray()
    ax.get_xaxis().set_visible(False)
    ax.get_yaxis().set_visible(False)

    ax = plt.subplot(3, n, i+n)
    plt.imshow(x_test_noisy[i].reshape(28, 28))
    plt.gray()
    ax.get_xaxis().set_visible(False)
    ax.get_yaxis().set_visible(False)    

    ax = plt.subplot(3, n, i + 2*n)
    plt.imshow(decoded_imgs[i].reshape(28, 28))
    plt.gray()
    ax.get_xaxis().set_visible(False)
    ax.get_yaxis().set_visible(False)
plt.show()
```

### OUTPUT:

### Training Loss, Validation Loss Vs Iteration Plot:

![image](https://github.com/22008008/convolutional-denoising-autoencoder/assets/118343520/a05d873b-3c97-452e-b9ea-9855799e165c)

### Original vs Noisy Vs Reconstructed Image:

![image](https://github.com/22008008/convolutional-denoising-autoencoder/assets/118343520/ff421e3a-5a82-4fff-99bf-86398b601be5)

### RESULT:

Thus, a Convolutional Autoencoder for Denoising was sucessfully implemented.
