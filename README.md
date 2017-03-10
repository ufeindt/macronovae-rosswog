# macronovae-rosswog
Macronova SEDs from Rosswog et al. (submitted), [arxiv:1611.09822](https://arxiv.org/abs/1611.09822), in `sncosmo`-friendly ASCII format that can be used with `read_griddata_ascii()`:

```python
import sncosmo

phase, wave, flux = sncosmo.read_griddata_ascii(filename)
source = sncosmo.TimeSeriesSource(phase, wave, flux)
model = sncosmo.Model(source=source)
```

For rate calculations you can use the `RateCalculator` class in `rates.py`:

```python
ratecalc = rates.RateCalculator(model, band='sdssg', ratefunc=lambda z: 3e-7)

n_exp = ratecalc.get_n_expected(21)
```
