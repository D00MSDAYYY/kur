Данное тип рассеяния обусловлен потерей ускоряемыми частицами энергии на излучение вследствие торможения.

```
def bremhsstrahlung_scattering_wiedemann(P_Torr, energy_acceptance):
	"""
	particle_accelerator_physics_3ed_wiedemann.pdf стр 328
	Возвращает:
	tau : время жизни [часы]
	"""
	P_nTorr = P_Torr / 1e-9
	tau_inv = 0.00653 * P_nTorr * math.log(1 / energy_acceptance)
	tau = 1 / tau_inv
	return tau
```