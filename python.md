
# Signal Extraction
Function that will interpolate the value of a measure of a given frequency to fill a specific length
```python
def resample(data, n):
    m = len(data)
    xin, xout = np.arange(n, 2*m*n, 2*n), np.arange(m, 2*m*n, 2*m)
    return interp1d(xin, data, 'nearest', fill_value='extrapolate')(xout)
```

# Plots