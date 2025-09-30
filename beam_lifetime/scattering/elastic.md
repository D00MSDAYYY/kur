Данное тип рассеяния обусловлен столкновением ускоряемых частиц с частицами остаточного газа.
## Константы из файла CONSTANTS_CONFIG.py

```
siberia2.beta = 1
siberia2.AverageBetatronXFunction = 10
siberia2.AverageBetatronYFunction = 5
siberia2.AverageBetatronFunction = ( siberia2.AverageBetatronXFunction + siberia2.AverageBetatronYFunction ) / 2
siberia2.HorizontalAperture = 20e-3
siberia2.VerticalAperture = 12e-3
siberia2.eA = 10e-3**2 / siberia2.AverageBetatronFunction
siberia2.eA_mm_mrad = 10 
siberia2.P_Pa = 1E-7
siberia2.P_Torr = siberia2.P_Pa * ( 7.50062 * 1E-3 )
siberia2.RevolutionFrequency = 2.4147E6
siberia2.Energy_GeV = 2.5
siberia2.gamma = siberia2.Energy_GeV / 0.511e-3
```

## Wiedmann 2 ed стр 323

Код в файле **scattering/elastic.py** . Продублирую здесь основные моменты на всякий случай.

Данная формула дана в СГС системе, поэтому ей не пользуюсь и она по какой-то причине дает очень маленький результат **(0.380)** с такими параметрами: 

![[Screenshot 2025-09-25 at 11.47.09.png]]

Вот более простая формула:
![[Screenshot 2025-09-25 at 11.48.57.png]]

```
def elactic_scattering_wiedemann2(p_CGS, eA, b_m, P_nTorr):
	tau_hours = 10.25 * ( p_CGS**2 * eA ) / ( b_m * P_nTorr )
	return tau_hours

lifetime.elastic_wiedemann2['value'] = elastic_scattering_wiedemann2( 2.5,
CONSTANTS_CONFIG.siberia2.eA_mm_mrad,
CONSTANTS_CONFIG.siberia2.AverageBetatronFunction,
CONSTANTS_CONFIG.siberia2.P_Torr * 1e9 )
```
Выдает значение **114** часа, что ближе к правде.

## Chao 2 ed стр 323

![[Screenshot 2025-09-25 at 13.01.45.png]]

![[Screenshot 2025-09-25 at 13.01.59.png]]

```

def elastic_scattering_chao(beta, nZ, Z, A_acceptance, beta_func_value, gamma, P_Torr, T_K):
	ng = 9.656E24 * nZ * P_Torr / T_K
	r_e, _, _ = constants.physical_constants['classical electron radius']
	sigma_el = 2 * math.pi * r_e**2 * Z**2 * beta_func_value / ( gamma**2 * A_acceptance )
	inv = ng * beta * constants.c * sigma_el
	tau = 1 / inv / 3600
	return tau

lifetime.elastic_chao['value'] = elastic_scattering_chao(
	beta=CONSTANTS_CONFIG.siberia2.beta,
	nZ=CONSTANTS_CONFIG.n_Z_avg,
	Z=CONSTANTS_CONFIG.Z_avg,
	A_acceptance=CONSTANTS_CONFIG.siberia2.eA,
	beta_func_value=CONSTANTS_CONFIG.siberia2.AverageBetatronFunction,
	gamma=CONSTANTS_CONFIG.siberia2.gamma,
	P_Torr=CONSTANTS_CONFIG.siberia2.P_Torr,
	T_K=CONSTANTS_CONFIG.T_gas_K
	)
```

Выдает значение **325** .

