# Guia da Simulação ACC - CLF-CBF-QP

Este documento descreve a arquitetura da simulação, a conexão entre os arquivos MATLAB/Simulink, a função de cada parâmetro e como ajustá-los para otimizar o desempenho do Controle de Cruzeiro Adaptativo (ACC).

---

## 1. Arquitetura dos Arquivos (Como eles se Conectam)

A simulação é composta por 4 arquivos principais que se comunicam em uma sequência lógica:

```mermaid
graph LR
    A[LIE_2026.m] -->|Gera simbolicamente| B(Parâmetros: LfV, LgV, Lfh, Lgh);
    B --> C[ACC_TESTE_2026.slx];
    D[INIT_ACC_2026.m] -->|Define condições iniciais e chama| C;
    C -->|A cada passo de tempo, chama| E[QPhild.m];
    E -->|Retorna u e delta| C;
    C -->|Retorna dados (t, Vf, D, h, u)| D;
    D -->|Plota gráficos| F[Resultados];
