# Ideal, Natural, & Flat-top -Sampling
# Aim
Write a simple Python program for the construction and reconstruction of ideal, natural, and flattop sampling.

# Tools required
Google colab

# Program
# Ideal Sampling

```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import resample

fs, f = 100, 5
t = np.arange(0, 1, 1/fs)
signal = np.sin(2*np.pi*f*t)

# Continuous Signal
plt.figure(figsize=(10,4))
plt.plot(t, signal, label='Continuous Signal')
plt.title('Continuous Signal (fs = 100 Hz)')
plt.xlabel('Time [s]')
plt.ylabel('Amplitude')
plt.grid(); plt.legend(); plt.show()

# Sampled Signal
plt.figure(figsize=(10,4))
plt.plot(t, signal, alpha=0.7)
plt.stem(t, signal, linefmt='r-', markerfmt='ro', basefmt='r-',
         label='Sampled Signal (fs = 100 Hz)')
plt.title('Sampling of Continuous Signal (fs = 100 Hz)')
plt.xlabel('Time [s]')
plt.ylabel('Amplitude')
plt.grid(); plt.legend(); plt.show()

# Reconstruction
reconstructed = resample(signal, len(t))
plt.figure(figsize=(10,4))
plt.plot(t, reconstructed, 'r--',
         label='Reconstructed Signal (fs = 100 Hz)')
plt.title('Reconstruction of Sampled Signal (fs = 100 Hz)')
plt.xlabel('Time [s]')
plt.ylabel('Amplitude')
plt.grid(); plt.legend(); plt.show()

```
# Natural Sampling
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

fs, T, fm, pulse_rate = 1000, 1, 5, 50
t = np.arange(0, T, 1/fs)

# Message signal
msg = np.sin(2*np.pi*fm*t)

# Pulse train
pulse = np.zeros_like(t)
w = int(fs/pulse_rate/2)
step = int(fs/pulse_rate)
for i in range(0, len(t), step):
    pulse[i:i+w] = 1

# Natural sampling
nat = msg * pulse
samples = nat[pulse == 1]
times = t[pulse == 1]

# Zero-Order Hold reconstruction
rec = np.zeros_like(t)
for i, tm in enumerate(times):
    idx = np.argmin(np.abs(t - tm))
    rec[idx:idx+w] = samples[i]

# Low-pass filter
b, a = butter(5, 10/(0.5*fs), btype='low')
rec = lfilter(b, a, rec)

# Plot
plt.figure(figsize=(14,10))
plt.subplot(4,1,1); plt.plot(t,msg); plt.title("Original Message Signal"); plt.grid()
plt.subplot(4,1,2); plt.plot(t,pulse); plt.title("Pulse Train"); plt.grid()
plt.subplot(4,1,3); plt.plot(t,nat); plt.title("Natural Sampling"); plt.grid()
plt.subplot(4,1,4); plt.plot(t,rec,'g'); plt.title("Reconstructed Message Signal"); plt.grid()
plt.tight_layout(); plt.show()

```
# Flat-topped Sampling

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

# Ideal sampling
<img width="866" height="393" alt="image" src="https://github.com/user-attachments/assets/828c2b36-6543-4265-9bd7-8a79cd9a1767" />
<img width="866" height="393" alt="image" src="https://github.com/user-attachments/assets/29e1b993-6c63-4ecc-8ebd-2ca13f02fd00" />
<img width="866" height="393" alt="image" src="https://github.com/user-attachments/assets/c9fbbf4d-488d-4d3d-a1e0-de61ec537484" />

# Natural sampling
<img width="866" height="393" alt="image" src="https://github.com/user-attachments/assets/fc318540-6562-494d-8a2e-ccd67af0c1de" />

# Flat-topped Sampling
<img width="866" height="393" alt="image" src="https://github.com/user-attachments/assets/4ff6f8f2-d924-4331-987d-23f2b5e5fd9d" />

# Results

Impulse sampling gives perfect reconstruction, while natural and flat-top sampling allow approximate reconstruction, with flat-top introducing slight amplitude distortion.
