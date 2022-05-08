---
title: 'Resampling: the Fourier Method'
date: 2022-05-97 22:00:00
categories: [DSP esstntials]
tags: [dsp, fourier, sampling]
math: true
# mermaid: true
---

In the last post, we saw how to downsample a signal using the polyphase FIR filter.
In fact, the polyphase FIR filter can be used for upsampling as well, or generally speaking, for resampling.
In [Scipy](https://scipy.org), there are two resampling functions: `resample` and `resample_poly`.
The default `scipy.signal.resample` function is used to resample a signal using the Fourier method.
And the `scipy.signal.resample_poly` function is used to resample a signal using the polyphase FIR filter.
In this post, we will see how the Fourier method works.

## Performance and Benefits

Before going into detail, let's see how the performance of the Fourier method compares to the polyphase FIR filter.

According to [this document](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.resample_poly.html#scipy.signal.resample_poly):
> This polyphase method will likely be faster than the Fourier method in `scipy.signal.resample` when the number of samples is large and prime, or when the number of samples is large and up and down share a large greatest common denominator.

However, the Fourier method is more flexible and can be used for other purposes.
The benefits of the Fourier method are:

1. The resulting frequency can be controlled with a much smaller granularity.
2. the downsampled signal can be chosen from any band of the original signal, not just the low-frequency band.
3. Similarly, the upsampled signal can have the original signal set to an arbitrary frequency, not just the DC.  

## Method

As the name suggested, the Fourier method resamples a signal in the frequency domain.
The components in the low-frequency band are the same in the frequency domain no matter whether it is downsampling or upsampling.
The resampling is done by removing or adding the components in the high-frequency band.
Therefore, the Fourier method resampling has three general steps:

1. Transform the signal to the frequency domain using the fast Fourier transform (FFT).
2. Remove the desired amount of high-frequency components for downsampling, or add the desired amount of zeros to the high-frequency band for upsampling;
3. Transform the signal back to the time domain using the inverse fast Fourier transform (IFFT) and compensate for the power difference.

During the FFT and IFFT, the number of points equals the number of signal samples.
The number of frequency samples to remove or add can be found by the difference between the length of the original signal `n_original` and the resampled signal `n_resampled`.
If the sampling rate of the original signal and the resampled signal are `fs_original` and `fs_resampled`, respectively, then the length of the resampled signal is: `n_resampled = (fs_resampled / fs_original) * n_original` and round to the nearest integer.

If the band of interest is not the low-frequency band, or around DC, a frequency shift can be introduced in the time domain.
For the downsampling case, the wideband signal is multiplied by a carrier such that the band of interest is shifted to the low-frequency band (around DC), and then the signal is transformed to the frequency domain for resampling.
For upsampling case, the resulting wideband signal is multiplied by a carrier such that the band of interest is shifted to the desired frequency.

## Demo Code

Here I will show you how to use the Fourier method to downsample a complex signal using Matlab.
The upsampling case is similar.

```matlab
n_original = length(original_signal);
% frequency shifted by fc Hz
t = (0:n_original - 1) / fs_original;
original_signal = original_signal .* exp(-1j * 2 * pi * fc * t');
% FFT
ft_original = fft(original_signal);
n_resampled = round((fs_resampled / fs_original) * n_original);
% initialize the resampled signal
ft_resampled = zeros(n_resampled, 1);
% downsampling and keep the low-frequency band
ft_resampled(1:floor(n_resampled/2)) = ft_original(1:floor(n_resampled/2));
ft_resampled(ceil(n_resampled/2):end) = ft_original(n_original - ceil(n_resampled/2) + 1:n_original);
% IFFT and compensate for the power difference
resampled_signal = ifft(ft_resampled) * (length(ft_resampled) / length(original_signal));
```

All right, that is all for this post. Thank you for reading.
