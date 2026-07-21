# Modelagem Matemática do Veículo - Função de Barreira de Controle (CBF) para o ACC

Enquanto a CLF quer levar o estado para um **ponto** específico $$( V_f = V_d )$$, a CBF quer manter o estado **dentro de uma região** segura. 
No ACC, a região segura é definida pela distância entre os carros: ela nunca pode ser negativa (colisão) nem menor que um limite seguro.

## 1. Função de Barreira de Controle (CBF) - Segurança

Para aplicar a CBF, precisamos definir matematicamente o que é "seguro". Definimos o conjunto $\mathcal{C}$$ (safe set) como:

<div align="center">
  <img src="https://latex.codecogs.com/png.image?%5Ccolor%7Bblack%7D%20%5Cmathcal%7BC%7D%20%3D%20%5C%7B%20x%20%5Cin%20%5Cmathbb%7BR%7D%5E2%20%5C%2C%20%7C%20%5C%2C%20h(x)%20%5Cgeq%200%20%5C%7D">
</div>

A função **$h(x)$** que define a fronteira da segurança no ACC é a distância relativa (distância mínima em relação ao veículo líder) descontada do tempo de reação $\tau_h$:

<div align="center">
  <img src="https://latex.codecogs.com/png.image?%5Ccolor%7Bblack%7D%20h(x)%20%3D%20D%20-%20%5Ctau_h%20%5Ccdot%20V_f">
</div>

Onde:
- $$( \tau_h > 0 )$$: Tempo de headway desejado (ex: 1.8 segundos). Garante que a distância seja proporcional à velocidade.
- Se $(h(x) > 0)$: Estamos seguros (distância folgada).
- Se $(h(x) = 0)$: Estamos no limite da segurança (fronteira).
- Se $(h(x) < 0)$: Violamos a segurança (colisão ou distância perigosa).


### 1.1. Derivadas de Lie da CBF

<div align="center">
  <img src="https://latex.codecogs.com/png.image?%5Ccolor%7Bblack%7D%20L_fh%20%3D%20%5Cnabla%20h%20%5Ccdot%20f(x)%20%3D%20%5Ctau_h%20%5Ccdot%20%5Cfrac%7BF_r(V_f)%7D%7Bm%7D%20%2B%20(V_l%20-%20V_f)">
</div>
<p>
<div align="center">
  <img src="https://latex.codecogs.com/png.image?%5Ccolor%7Bblack%7D%20L_gh%20%3D%20%5Cnabla%20h%20%5Ccdot%20g(x)%20%3D%20-%5Ctau_h">
</div>
<p>
*(Nota: Estes cálculos são exatamente os realizados pelo script `LIE_2026.m` quando ele executa `Lfhacc` e `Lghacc`).*

### 1.2. Restrição de Segurança (CBF)

A condição para que a distância nunca se torne negativa (invariância do conjunto seguro) é:

<div align="center">
  <img src="https://latex.codecogs.com/png.image?%5Ccolor%7Bblack%7D%20L_fh%20%2B%20L_gh%20%5Ccdot%20u%20%2B%20%5Cgamma%20h(x)%20%5Cgeq%200">
</div>
<p>
Onde $$( \gamma > 0 )$$: Parâmetro que ajusta a "agressividade" da barreira. Quanto maior, mais cedo o sistema começa a frear para evitar a violação da segurança.

<a href="modelandocbf.md">Ver modelagem do CBF.</a>

a CLF puxa o sistema para $$( V_d )$$, a CBF o segura para que $$( D )$$ não caia abaixo de $$( \tau_h V_f )$$, criando um equilíbrio dinâmico entre desempenho e segurança.










