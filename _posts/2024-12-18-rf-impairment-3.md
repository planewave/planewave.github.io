---
title: 'Understanding RF Impairments - Part 3'
date: 2024-12-18 20:18:00
categories: [RF]
tags: [interview, Impairment, CFO, nonlinear]
math: true
mermaid: false
---

# Carrier Frequency Offset and Phase Noise

## Introduction

In the previous articles, we discussed thermal noise and amplifier nonlinear distortion in wireless communication systems. In this article, we will explore other two common RF impairments: carrier frequency offset (CFO) and phase noise (PN). Both impairments arise due to imperfections in oscillators and must be addressed in the design of wireless systems.

## Carrier Frequency Offset

Carrier frequency offset, or CFO, is one of the most commonly encountered RF impairments and requires consideration in the design of almost all wireless receivers. It manifests as a frequency shift of the carrier in the received signal.

In a single carrier system, the CFO can be visualized as a rotating constellation on the receiver side.

![](/assets/img/posts/rf-imp-3/cfo.png){: width="400"}

_The constellation of a 16QAM system rotates due to the CFO (from Matlab Documentation)_

### Causes of CFO

CFO can arise from:

- **Frequency differences between local oscillators**: Discrepancies between the transmitter and receiver oscillators.
- **Doppler shift**: Relative motion between the transmitter and receiver.

### Mathematical model

Mathematically, the received signal $y(t)$ can be expressed as
$$y(t)=x(t)e^{j2\pi(f_c+\Delta f)t}$$
where $x(t)$ is the transmitted signal, $f_c$ is the carrier frequency, and $\Delta f$ is the CFO.

### Imapact of CFO

OFDM systems are particularly vulnerable to CFO because it introduces inter-channel interference, disrupting the orthogonality of OFDM subcarriers, introducing intercarrier interference (ICI).

For example, at the 2.4 GHz band, CFO can reach tens of kilohertz, and at the 5.8 GHz band, it can reach hundreds of kilohertz. Wireless receiver design must account for CFO through estimation and compensation techniques.

### Measurement and Compensation

Several schemes can estimate and compensate for CFO, with one common method utilizing the training sequence in the received signal.

**Example: CFO Compensation in LTE**

CFO in LTE signals can be divided into two components:

1. Integer CFO: An integer multiple of the subcarrier spacing (15 kHz).
2. Fractional CFO: A remainder less than 15 kHz.

**Fractional CFO Estimation**: Leveraging the cyclic prefix (CP) structure of OFDM signals, we estimate the fractional CFO by measuring the phase difference between the CP and the symbol tail, then dividing it by the symbol duration.

**Integer CFO Estimation**: Using the Primary Synchronization Signal (PSS), a frequency-domain cross-correlation with a reference PSS reveals the integer CFO by identifying the correlation peak.
## Phase Noise

### Cause of PN

Phase noise results from oscillator imperfections. It manifests as rapid, short-term, random fluctuations in the carrier wave phase.

### Observable effects

- **Frequency domain**: Spreading of the carrier around the center frequency.
- **Time domain**: Misalignment of the carrier phase.

![](/assets/img/posts/rf-imp-3/phase-noise-1.png){: width="400"}

_The phase noise observed in the frequency and time domain_

For a single carrier system, the phase noise causes the constellation points to spread.

![](/assets/img/posts/rf-imp-3/phase-noise-const.png){: width="400"}

_The constellation of a QPSK system spreads due to the phase noise_

### Mathematical model

Without phase noise, the RF signal $y(t)$ can be simply written as
$$y(t)=x(t)e^{\mathrm{j} 2 \pi f_c t}$$

by taking into account the LO phase noise $\theta(t)$
$$y(t)=x(t)e^{\mathrm{j}(2\pi f_c t + \theta(t))}=x(t)e^{\mathrm{j}2 \pi f_c t}e^{\mathrm{j}\theta(t)}$$

The phase noise $\theta(t)$ is commonly described in the frequency domain by its power spectral density (PSD) in dBc/Hz, representing the noise power ratio at a frequency offset to the carrier power.

### Impact of PN

Similar to the CFO, phase noise is especially detrimental in OFDM systems as it destroys subcarrier orthogonality, resulting in ICI.

## Comparison between CFO and PN

While both CFO and PN stem from oscillator imperfections, their effects differ:

- **CFO**: A fixed offset in the frequency domain.
- **PN**: Random phase fluctuations in the carrier.

Combined, CFO and PN can significantly degrade wireless system performance, particularly in OFDM systems.

## Conclusion

In this article, we discussed two common RF impairments: carrier frequency offset and phase noise. Both impairments are mainly introduced by the imperfection of the oscillator and must be considered in the design of wireless systems. The CFO can be estimated and compensated using the training sequence in the received signal, while the phase noise can be mitigated by using a high-quality oscillator. The combined effect of CFO and phase noise can significantly degrade the performance of the wireless system, especially in the OFDM system.
