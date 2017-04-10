# macronovae-rosswog
Macronova SEDs from Rosswog et al. (2017), [arxiv:1611.09822](https://arxiv.org/abs/1611.09822), in `sncosmo`-friendly ASCII format that can be used with `read_griddata_ascii()`:

```python
import sncosmo

phase, wave, flux = sncosmo.read_griddata_ascii(filename)
source = sncosmo.TimeSeriesSource(phase, wave, flux)
model = sncosmo.Model(source=source)
```

For rate calculations you can use the `RateCalculator` class in `rates.py`:

```python
import rates
ratecalc = rates.RateCalculator(model, band='sdssg', ratefunc=lambda z: 3e-7)

n_exp = ratecalc.get_n_expected(21)
```
### File description

The SED file format is the following:
* column 1: time since merger [days]
* column 2: wavelength [Å]
* column 3: flux [erg s^-1 cm^-2 Å^-1]. 

The models can be identified from the file names in the following way:
* Dynamic ejecta models for NSNS mergers are named according to the NS masses, i.e. ns12n14 stands for 1.2 and 1.4 M_sun.
* Dynamic ejecta models for NSBH mergers are numbered according to Table 1 in the paper.
* Wind models are numbered according as in Table 2 of the paper.
* If no nuclear mass model (FRDM or DZ31) is stated in the file name, MNmodel1 with FRDM was used. Otherwise it is MNmodel2 with the stated nuclear mass model.
* Some file names contain a string indicating the opacity, e.g. kappa10 for an opacity of 10 cm^2 g^-1. If the opacity is not stated, it is 10 cm^2 g^-1 for dynamic ejecta and 1 cm^2 g^-1 for winds.
