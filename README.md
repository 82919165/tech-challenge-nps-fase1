# Tech Challenge Fase 1 — Case NPS Preditivo

Projeto da pós FIAP PosTech AI Scientist (Fase 1) — Ítalo Henriques.

## Onde está cada entrega (leia primeiro)

| Entrega | Onde |
| --- | --- |
| Base original / base tratada | `data/desafio_nps_fase_1.csv` / `data/nps_tratado.csv` |
| Tratamento e definição da target | `notebooks/01_carga_e_tratamento.ipynb` |
| EDA (análise exploratória) | `notebooks/02_eda.ipynb` |
| Validação estatística | `notebooks/03_estatistica.ipynb` |
| Figuras | `reports/figuras/` |
| Apresentação (PowerPoint, para gestores) | `reports/apresentacao_nps.pptx` |
| Vídeo executivo (≤ 5 min, 4:15) | YouTube (não listado): https://youtu.be/Ee0Wc2aIxKk · Reserva (iCloud): https://www.icloud.com/iclouddrive/047tfONIaYHZswbapEO7-69ug · Detalhes em `reports/video.txt` |

## 1. Objetivo do projeto

Entender quais fatores operacionais (pedido, logística e atendimento) mais influenciam a satisfação do cliente (NPS) de um e-commerce e em que ponto da jornada a empresa pode agir de forma preventiva, antes da pesquisa de NPS.

Pergunta orientadora: *quais fatores operacionais mais influenciam o NPS e em que ponto a empresa pode intervir para evitar detratores?*

## 2. Descrição da base de dados

Arquivo `data/desafio_nps_fase_1.csv`: 2.500 pedidos, 19 colunas, um pedido por cliente. Após o tratamento: 2.346 pedidos e 20 colunas (`nps_class` criada) em `data/nps_tratado.csv`.

| Grupo | Variáveis |
| --- | --- |
| Cliente | `customer_id`, `customer_age`, `customer_region`, `customer_tenure_months` |
| Pedido | `order_id`, `order_value`, `items_quantity`, `discount_value`, `payment_installments` |
| Logística | `delivery_time_days`, `delivery_delay_days`, `freight_value`, `delivery_attempts` |
| Atendimento | `customer_service_contacts`, `resolution_time_days`, `complaints_count` |
| Resultado / satisfação | `nps_score` (nota 0–10, **target**), `repeat_purchase_30d`, `csat_internal_score` |

Variáveis de **resultado** (`csat_internal_score`, `repeat_purchase_30d`) acompanham ou vêm depois da nota e não são tratadas como causas. `resolution_time_days` e `customer_service_contacts` só são conhecidos depois da compra (ver seção 7).

## 3. Metodologia (CRISP-DM)

1. **Entendimento do negócio** — queda de satisfação medida pelo NPS; foco em fatores que a empresa controla.
2. **Definição da target** — `nps_score`; classificação: detrator < 7, neutro 7 a < 9, promotor ≥ 9, aplicada às notas decimais sem arredondar. NPS = % promotores − % detratores.
3. **Tratamento** — `01_carga_e_tratamento.ipynb`: tipos, nulos, duplicados, inconsistências e exclusões (2.500 → 2.346 pedidos).
4. **EDA** — `02_eda.ipynb`: correlações de Spearman, médias por faixa de atraso e de reclamações, cruzamento atraso × reclamações, região e idade.
5. **Validação estatística** — `03_estatistica.ipynb`: intervalo de confiança do NPS (bootstrap) e testes por região e por número de reclamações.
6. **Modelo preditivo** — parte opcional, não realizada.

## 4. Principais resultados

- **NPS geral: −79,1** (83,7% detratores, 11,7% neutros, 4,6% promotores; base tratada, IC 95% ≈ −81,2 a −77,1).
- **Atraso na entrega é o fator mais forte** (Spearman −0,57): perda contínua de cerca de 1 ponto de nota por dia de atraso (6,9 sem atraso → 1,4 com 5 dias). 88% dos pedidos chegam atrasados.
- **Reclamações** são o segundo fator (−0,50) e têm um degrau claro: de 1 para 2 reclamações, o % de detratores sobe de 30% para 65% (+35 pontos percentuais).
- **Atraso e reclamações são alavancas em boa parte independentes** (correlação entre elas 0,19): atacar uma não resolve a outra.
- **Contatos com o atendimento** têm efeito moderado (−0,34).
- Valor do pedido, frete, desconto, parcelas e itens quase não se relacionam com a nota (|ρ| ≤ 0,07).
- **Região e idade não segmentam o NPS**: diferenças entre regiões dentro do ruído estatístico.

Recomendações: prometer prazos realistas e melhorar transportadoras para reduzir atrasos; criar um gatilho de intervenção na **primeira reclamação**.

## 5. Como reproduzir

```bash
git clone https://github.com/82919165/tech-challenge-nps-fase1.git
cd tech-challenge-nps-fase1
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Abra a pasta no VS Code, escolha o kernel `.venv` e execute os notebooks em ordem (01 → 02 → 03) com **Run All**. Os notebooks leem `data/` e gravam figuras em `reports/figuras/`.

## 6. Estrutura de pastas

```
data/        base original e base tratada
notebooks/   01 tratamento, 02 EDA, 03 estatística
models/      (vazia: modelo opcional não realizado)
reports/     figuras, apresentação e instruções do vídeo
```

## 7. Limitações e riscos

- **Correlação não é causalidade.** Os dados são de um único momento; a “janela da primeira reclamação” é uma hipótese a validar com teste A/B.
- **Momento da informação (vazamento):** reclamações, contatos e tempo de resolução ocorrem depois da compra; só serviriam para prevenção se conhecidos antes da pesquisa de NPS.
- **Regra de corte:** notas decimais sem arredondamento; arredondar mudaria o NPS (sensibilidade entre −71,6 e −74,1).
- **Efeito teto:** com 84% de detratores, a nota média é melhor que % de detratores para comparar grupos.
- Linhas excluídas no tratamento (154) podem alterar levemente os números (NPS −80,0 na base completa).

## 8. Uso de IA

O Claude (Anthropic) foi usado como tutor e apoio: estrutura inicial do repositório (pastas e modelo de README), orientação de raciocínio, revisão de explicações e rascunhos de gráficos e apresentação. A análise, as decisões metodológicas, a interpretação dos resultados e as conclusões são de autoria do aluno, que revisou e executou todo o código.
