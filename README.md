# Ideal, Natural, & Flat-top -Sampling
# Aim
Write a simple Python program for the construction and reconstruction of ideal, natural, and flattop sampling.

# Tools required
Google colab

# Program

```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Parameters
fs, T, fm, pulse_rate = 1000, 1, 5, 50
t = np.arange(0, T, 1/fs)

# Message signal
message = np.sin(2*np.pi*fm*t)

# Sampling indices
step = int(fs/pulse_rate)
indices = np.arange(0, len(t), step)

# Flat-top sampling
flat = np.zeros_like(t)
width = int(fs/(2*pulse_rate))

for i in indices:
    flat[i:i+width] = message[i]

# Low-pass filter
b, a = butter(5, (2*fm)/(0.5*fs), btype='low')
reconstructed = lfilter(b, a, flat)

# Plotting
plt.figure(figsize=(14,10))

plt.subplot(4,1,1)
plt.plot(t, message)
plt.title("Original Message Signal")
plt.grid()

plt.subplot(4,1,2)
plt.stem(t[indices], np.ones_like(indices), basefmt=" ")
plt.title("Ideal Sampling Instances")
plt.grid()

plt.subplot(4,1,3)
plt.plot(t, flat)
plt.title("Flat-Top Sampled Signal")
plt.grid()

plt.subplot(4,1,4)
plt.plot(t, reconstructed, color='green')
plt.title("Reconstructed Signal")
plt.grid()

plt.tight_layout()
plt.show()
```
# Output Waveform

<img width="1390" height="989" alt="image" src="https://github.com/user-attachments/assets/4ff6f8f2-d924-4331-987d-23f2b5e5fd9d" />

# Results

Impulse sampling gives perfect reconstruction, while natural and flat-top sampling allow approximate reconstruction, with flat-top introducing slight amplitude distortion.
