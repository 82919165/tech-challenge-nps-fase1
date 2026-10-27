# Tech Challenge Fase 1 — Case NPS Preditivo

Projeto da pós FIAP PosTech AI Scientist (Fase 1).

## 1. Objetivo do projeto
Entender quais fatores operacionais (pedido, logística e atendimento) mais influenciam a satisfação do cliente (NPS) de um e-commerce e em que ponto da jornada a empresa pode agir de forma preventiva, antes da pesquisa de NPS.

Pergunta orientadora: *quais fatores operacionais mais influenciam o NPS e em que ponto a empresa pode intervir para evitar detratores?*

## 2. Descrição da base de dados
- Arquivo: `data/desafio_nps_fase_1.csv` (2.500 pedidos, 19 colunas, um pedido por cliente).
- Grupos de variáveis: cliente, pedido, logística, atendimento e resultado/satisfação (`nps_score`, `repeat_purchase_30d`, `csat_internal_score`).
- Dicionário de dados: *(preencher a partir do enunciado)*.

## 3. Metodologia
Segue o CRISP-DM: entendimento do negócio, definição da target, preparação dos dados, EDA com foco em negócio e (opcional) modelo preditivo.

1. Entendimento do negócio — *(link/resumo)*
2. Definição da target — `nps_score` *(justificativa e riscos)*
3. Tratamento e EDA — `notebooks/01_carga_e_tratamento.ipynb`, `notebooks/02_eda.ipynb`
4. Modelo preditivo (opcional) — `notebooks/03_modelo.ipynb`

## 4. Principais resultados
*(preencher após a EDA)*

## 5. Como reproduzir
```bash
git clone https://github.com/82919165/tech-challenge-nps-fase1.git
cd tech-challenge-nps-fase1
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook notebooks/
```

## 6. Estrutura de pastas
```
data/        base de dados original
notebooks/   tratamento, EDA e modelo
models/      modelos treinados (se houver)
reports/     figuras e material de apoio
```

## 7. Limitações e riscos
*(preencher: regra de corte do NPS, vazamento de dados, causalidade x correlação)*

## 8. Uso de IA
O Claude (Anthropic) foi usado como tutor e apoio: estrutura inicial do repositório (pastas e modelo de README) e orientação de raciocínio durante o estudo. A análise, as decisões metodológicas, a interpretação dos resultados e as conclusões deste projeto são de autoria do aluno.
