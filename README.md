## Informações acadêmicas

| Informação | Detalhes |
|---|---|
| **Disciplina** | Sistemas Distribuídos |
| **Professor** | Francisco Airton Pereira da Silva |
| **Aluno** | Pedro Henrique Silva Rodrigues |
| **Artigo no Overleaf** | [Acessar artigo](https://www.overleaf.com/project/6af635c2c05d8dcc5e45d2e3/share#e0d2e32847fcd898f61b8d9e098e4c197155ae9616f91de2) |
| **Vídeo** | [Assistir no YouTube](https://youtu.be/vGJu7ZeDJLs) |

# Trabalho 07 — Redes de Petri Estocásticas

Modelo e avaliação quantitativa de um **servidor web com fila limitada**, desenvolvido para aplicar os conceitos de Redes de Petri Estocásticas (SPN).

## Objetivos

- Representar o fluxo de requisições em um servidor web;
- Avaliar o impacto da taxa de chegada, do número de servidores e da capacidade do sistema;
- Medir desempenho, tempo médio de resposta, probabilidade de descarte e utilização dos servidores.

## Modelo

O modelo está disponível em [`config/servidor_web_spn.json`](config/servidor_web_spn.json).

### Fluxo das requisições

1. **Arrival** gera requisições de acordo com uma distribuição exponencial.
2. **Accept** aceita a requisição quando há espaço disponível no sistema.
3. **Reject** descarta a requisição quando todos os slots estão ocupados.
4. **StartService** encaminha uma requisição da fila para um servidor livre.
5. **FinishService** encerra o atendimento, libera o servidor e o slot e contabiliza a requisição concluída.
6. **Discard** remove as requisições rejeitadas do modelo.

### Parâmetros padrão

| Parâmetro | Valor | Descrição |
|---|---:|---|
| `ARRIVAL_DELAY` | 1,25 | Intervalo médio entre chegadas |
| `SERVICE_TIME` | 1,25 | Tempo médio de atendimento |
| `NUM_SERVERS` | 2 | Número de servidores |
| `QUEUE_CAPACITY` | 10 | Capacidade da fila |
| `SYSTEM_CAPACITY` | 12 | Capacidade total do sistema |
| `DROP_PASSAGE` | 0,0001 | Tempo médio para remover requisições descartadas |

O sistema inicia com 12 slots disponíveis e 2 servidores ociosos.

## Métricas

| Métrica | Descrição |
|---|---|
| **TP** | Throughput, ou taxa de requisições em atendimento |
| **MRT** | Tempo médio de resposta |
| **DP** | Percentual de requisições descartadas |
| **U** | Utilização média dos servidores |


## Cenários avaliados

Os dados exportados estão em [`results/csv/`](results/csv/) e os gráficos em [`results/graphics/`](results/graphics/).

### Throughput (TP)

Variação do intervalo médio entre chegadas, com 2 servidores:

| 1 / `ARRIVAL_DELAY` | TP |
|---:|---:|
| 0,40 | 0,397993 |
| 0,80 | 0,798819 |
| 1,20 | 1,189358 |
| 1,60 | 1,473799 |
| 2,00 | 1,573621 |

### Tempo médio de resposta (MRT)

Variação do número de servidores, com `ARRIVAL_DELAY = 0,80`:

| Servidores | MRT |
|---:|---:|
| 1 | 12,979505 |
| 2 | 2,798965 |
| 3 | 1,475086 |

### Probabilidade de descarte (DP)

Variação da capacidade total, com `ARRIVAL_DELAY = 1,60`:

| Capacidade do sistema | DP (%) |
|---:|---:|
| 4 | 2,051588 |
| 8 | 0,047980 |
| 12 | 0,001190 |

### Utilização (U)

Variação da carga de chegada para 1, 2 e 3 servidores:

| 1 / `ARRIVAL_DELAY` | 1 servidor | 2 servidores | 3 servidores |
|---:|---:|---:|---:|
| 0,80 | 92,289022% | 49,926194% | 33,352624% |
| 1,20 | 99,735537% | 74,334902% | 50,084008% |
| 1,60 | 99,994451% | 92,112413% | 66,456746% |

## Principais observações

- O aumento da taxa de chegada eleva o throughput até a capacidade de atendimento do sistema.
- A inclusão de servidores reduz significativamente o tempo médio de resposta.
- A ampliação da capacidade total diminui a probabilidade de descarte.
- Com poucos servidores, a utilização se aproxima de 100% em cenários de maior carga.

## Estrutura do repositório

```text
.
├── config/
│   └── servidor_web_spn.json # Arquivo json com as configurações do modelo
├── results/
│   ├── csv/       # Dados numéricos das simulações
│   ├── graphics/  # Gráficos dos cenários
│   └── model/     # PDF do modelo
├── LICENSE
└── README.md
```

## Reprodução

1. Acesse a [ferramenta de modelagem SPN da Jupiter Analytics](https://www.jupiteranalytics.com.br) e importe [`config/servidor_web_spn.json`](config/servidor_web_spn.json).
2. Execute as simulações alterando os parâmetros de cada cenário.
3. Exporte os dados para CSV e os gráficos para PDF.
4. Compare os resultados com os arquivos existentes em [`results/`](results/).

## Licença

Este projeto está licenciado sob a **GNU Affero General Public License v3.0 (AGPL-3.0)**. Consulte o arquivo [`LICENSE`](LICENSE) para acessar o texto completo da licença.
