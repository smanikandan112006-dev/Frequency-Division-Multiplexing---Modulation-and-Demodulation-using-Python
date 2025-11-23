# Frequency-Division-Multiplexing---Modulation-and-Demodulation-using-Python

## Aim:

To generate an FDM signal by multiplexing multiple baseband message signals on different carrier frequencies, transmit (sum) them, optionally add channel noise, then recover each message by bandpass filtering and coherent demodulation in Python (Google Colab). Observe time & frequency domain signals and measure recovery quality.


## Apparatus Required:

Google Colab (or any Python environment)

Python libraries: numpy, matplotlib, scipy (scipy.signal)


## Theory:

FDM places different message signals in separate, non-overlapping frequency bands by modulating each message onto a distinct carrier frequency. The multiplexed signal is the sum of all modulated channels. At the receiver, bandpass filters (or tuned filters) isolate each channel; then each isolated carrier is demodulated (coherently multiplied by a synchronized carrier) and low-pass filtered to recover the original baseband.

## Procedure:

1 — Imports and parameters

2 — Create message signals and carriers

3 — Modulate each message (standard AM DSB-SC) and form FDM signal

4 — Frequency domain (spectrum) of FDM signal

5 — (Optional) Add AWGN noise to FDM signal

6 — Receiver: isolate each channel with bandpass filter

7 — Demodulate each isolated channel (coherent) and low-pass filter to recover baseband
 ## Program:
 ```
import numpy as np
import matplotlib.pyplot as plt
from scipy import signal
fs = 50000                 
T = 0.01                    
t = np.arange(0, T, 1/fs)   
N = 5                      
msg_freq = np.array([120, 240, 340, 500, 800])
carrier_freq = np.array([3000, 6000, 9000, 12000, 15000])
messages = np.sin(2*np.pi*msg_freq[:, None] * t)
carriers = np.cos(2*np.pi*carrier_freq[:, None] * t)
modulated = messages * carriers
fdm_signal = np.sum(modulated, axis=0)
demodulated_raw = 2 * (fdm_signal[None, :] * carriers)   
cutoff_hz = 1200.0
b, a = signal.butter(6, cutoff_hz / (0.5 * fs), btype='low')
demodulated = signal.filtfilt(b, a, demodulated_raw, axis=1)
plt.rcParams.update({'figure.max_open_warning': 0})
fig1, axes1 = plt.subplots(N, 1, figsize=(8, 8), sharex=True)
fig1.suptitle("Original Message Signals")
for i, ax in enumerate(axes1):
    ax.plot(t, messages[i, :])
    ax.set_ylabel(f"m{i+1}")
axes1[-1].set_xlabel("Time (s)")
plt.tight_layout(rect=[0, 0, 1, 0.96])

fig2, ax2 = plt.subplots(1, 1, figsize=(10, 3))
ax2.plot(t, fdm_signal)
ax2.set_title("FDM Composite Signal")
ax2.set_xlabel("Time (s)")
ax2.set_ylabel("Amplitude")
plt.tight_layout()

fig3, axes3 = plt.subplots(N, 1, figsize=(8, 8), sharex=True)
fig3.suptitle("Demodulated (Recovered) Signals after LPF")
for i, ax in enumerate(axes3):
    ax.plot(t, demodulated[i, :])
    ax.set_ylabel(f"rec{i+1}")
axes3[-1].set_xlabel("Time (s)")
plt.tight_layout(rect=[0, 0, 1, 0.96])

plt.show()
import math
end_sample = int(0.002 * fs) 
plt.figure(figsize=(8,3))
plt.plot(t[:end_sample], messages[i, :end_sample], label='original m1')
plt.plot(t[:end_sample], demodulated[i, :end_sample], '--', label='recovered m1')
plt.legend()
plt.xlabel("Time (s)")
plt.title("Original vs Recovered (channel 1) — zoom")
plt.show()
```
## Output:
<img width="790" height="789" alt="image" src="https://github.com/user-attachments/assets/cbac45c8-c6eb-490f-b05b-8995da577e51" />
<img width="989" height="290" alt="image" src="https://github.com/user-attachments/assets/f0f17c2a-595b-4338-82c6-df12fa22d285" />
<img width="790" height="789" alt="image" src="https://github.com/user-attachments/assets/e17a02a5-3726-4df0-a5e7-4df3e699b2dd" />
<img width="689" height="316" alt="image" src="https://github.com/user-attachments/assets/d9b86bf3-b289-4de6-8c97-5740e638d798" />


## Result:
Frequency Division Multiplexing (FDM) allows multiple signals to share a single channel by assigning each one a unique frequency band. It enables simultaneous transmission without interference using proper filtering. FDM is commonly used in radio, TV, and communication systems. However, it requires more bandwidth and careful channel spacing.

