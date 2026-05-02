# **Fourier Series**  

We can write any function $f(\theta)$ as a Fourier series sum:

$$f(\theta) = \sum_{n=-\infty}^{\infty} c_n e^{in\theta}$$

The coefficients $c_n$ are defined as:

$$c_n = \frac{1}{2\pi}\int_0^{2\pi} e^{-i n \theta} f(\theta) \text{d}\theta$$

We can translate our main variable to $t$ instead of $\theta$, to match data defined over time. This also means the upper limit of the integral turns into our period $T$, and $\text{d}\theta$ turns into $\frac{2\pi}{T}\text{d}t$. This gives: 

$$f(t) = \sum_{n=-\infty}^{\infty} c_n e^{i n t}$$

and

$$c_n = \frac{1}{T}\int_0^{T} e^{-i n t} f(t) \text{d}t$$

# **Fourier Transform**

A **Fourier Transform** ($\mathcal{F}$) computes a function $\tilde{f}(\omega)$, which spikes at the frequencies ($\omega$-values) corresponding to the coefficients ($c_n$ values) in the original Fourier series. The Fourier Transform is defined like this:

$$\mathcal{F}(f) = \tilde{f}(\omega) = \int_{-\infty}^\infty e^{-i 2\pi \omega t} f(t) \text{d}t $$

We also have an **Inverse Fourier Transform** ($\mathcal{F}^{-1}$), which is analogous to the formula for the Fourier series of $f(t)$ in terms of the $c_n$ coefficients (the exact relationship is shown further below at the bottom of the notebook).

$$\mathcal{F}^{-1}(\tilde{f}) = f(t) = \int_{-\infty}^\infty \tilde{f}(\omega) e^{i 2\pi\omega t} \text{d}t $$

# **Discretizing the Fourier Transform**

For a function limited to finite time period, such as the data files we have worked with in class, we can reduce the integral to that period, because outside that period the function is 0.

$$\tilde{f}(\omega) = \int_0^{T} e^{-i 2 \pi \omega t} f(t) \text{d}t $$

We can now discretize this integral for ease of computing. To do so, we approximate the infinitessimal $\text{d}t$ as the discrete $\Delta t = \frac{T}{N}$. This results in discrete time values $t = 0, \frac{T}{N}, \frac{2T}{N},  \ldots , \frac{(N-1)T}{N}$, and frequency values $\omega = 0, \frac{1}{T}, \frac{2}{T}, \ldots , \frac{N-1}{T}$.

$$\tilde{f}(\omega) = \frac{T}{N} \sum_{t=0}^{(N-1)T/N} e^{-i 2\pi\omega t} f(t) $$

We can apply a similar discretization to the **Inverse Fourier Transform**. For this transformation, we approximate the infinitessimal $\text{d}\omega$ as the discrete $\Delta \omega = \frac{1}{T}$. Here is the resulting discretized sum:

$$f(t) = \frac{1}{T} \sum_{\omega = 0}^{(N-1)/T} \tilde{f}(\omega) e^{i 2\pi\omega t} $$

Next, we can reformulate the sums to count from 0 to N-1, to be consistent between the two of them. Plugging in $t = t_n = \frac{nT}{N}$ and $\omega = \omega_k = \frac{k}{T}$. Each of $n$ and $k$ count up from 0 to N-1. These substitutions yield:

$$\tilde{f}(\omega_k) = \frac{T}{N} \sum_{n=0}^{N-1} e^{-i 2\pi kn/N} f(t_n)$$

$$f(t_n) = \frac{1}{T} \sum_{k = 0}^{N-1} \tilde{f}(\omega_k) e^{i 2\pi kn/N}$$

# **Discrete Fourier Transform**

From here, let's look at something called the **Discrete Fourier Transform**. This is a formula derived exactly like the process above, with a key simplication: $t_n = n$ and $\omega_k = k$. This assumption strips the units out of time and frequency, and sets $\Delta t = 1$ and $\Delta \omega = 1$. In other words, each step in time is "1 time-unit" and each step in frequency is "1 frequency-unit." This also sets $T=N$. The **Discrete Fourier Transform** is written as $X(k)$, which is the same as $\frac{N}{T} \tilde{f}(\omega_k)$.

$$X(k) = \sum_{n=0}^{N-1} e^{-i 2\pi kn/N}f(t_n)$$

The corresponding **Inverse Discrete Fourier Transform** is written as a small $x(n)$, which is the same as $f(t_n)$.

$$x(n) = \frac{1}{T} \sum_{k = 0}^{N-1} \tilde{f}(\omega_k) e^{i 2\pi kn/N}$$

Written in terms if $X$ and $x$, we get:

$$X(k) = \sum_{n=0}^{N-1} e^{-i 2\pi kn/N} x(n)$$

and 

$$x(n) = \frac{1}{N} \sum_{k = 0}^{N-1} X(k) e^{i 2\pi kn/N}$$

These two formulas are the values that get computed when you call `np.fft.fft` and `np.fft.ifft`.