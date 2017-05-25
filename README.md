# macronovae-rosswog
Macronova SEDs from Rosswog et al. (2017), [arxiv:1611.09822](https://arxiv.org/abs/1611.09822), in `sncosmo`-friendly ASCII format that can be used with `read_griddata_ascii()`:

```python
import sncosmo

phase, wave, flux = sncosmo.read_griddata_ascii(filename)
source = sncosmo.TimeSeriesSource(phase, wave, flux)
model = sncosmo.Model(source=source)
```

The SEDs are scaled to 10 pc distance and thus leaving the model's `amplitude` parameter at the default value of 1 means that `model.bandmag()` will return absolute magnitude. To place the macronova at cosmological distances, divide the `amplitude` by the square of the luminosity distance:
```python
from astropy.cosmology import Planck15

model.set(z=0.1, amplitdue=1./Planck15.luminosity_distance(0.1).value**2)
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

Each file has a header with the model parameters but the models can also be identified from the file names in the following way:
* Dynamic ejecta models for NSNS mergers are named according to the NS masses, i.e. ns12n14 stands for 1.2 and 1.4 M_sun.
* Dynamic ejecta models for NSBH mergers are numbered according to Table 1 in the paper.
* Wind models are numbered according as in Table 2 of the paper.
* If no nuclear mass model (FRDM or DZ31) is stated in the file name, MNmodel1 with FRDM was used. Otherwise it is MNmodel2 with the stated nuclear mass model. The major difference of the mass models in this context comes from alpha decays of nuclei heavier than lead. Therefore, the mass formulae are mainly relevant for the dynamic ejecta, since the winds do not produce these very heavy elements.
* Some file names contain a string indicating the opacity, e.g. kappa10 for an opacity of 10 cm^2 g^-1. If the opacity is not stated, it is 10 cm^2 g^-1 for dynamic ejecta and 1 cm^2 g^-1 for winds.
