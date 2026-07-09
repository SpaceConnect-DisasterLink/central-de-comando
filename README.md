<div align="center">

# 🚨 DisasterLink · Central de Comando

**Onde a Defesa Civil faz a triagem das vítimas, acompanha os canais de comunicação e despacha as equipes de resgate**

Score de urgência de 0 a 100, equipe mais próxima pela fórmula de Haversine, simulação de falha em cascata dos canais e sensores que detectam sobreviventes.

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)
![Sem dependências](https://img.shields.io/badge/depend%C3%AAncias-nenhuma-2E7D32?style=flat-square)
![JSON](https://img.shields.io/badge/exporta%C3%A7%C3%A3o-JSON-555555?style=flat-square&logo=json&logoColor=white)

<sub>Computational Thinking with Python · Global Solution 2026.1 · Space Connect · FIAP</sub>

</div>

---

## Sobre o projeto

Quando uma enchente, um desabamento ou um incêndio acontece, a comunicação costuma cair junto: sem sinal de celular, sem internet e, muitas vezes, sem luz. As vítimas não conseguem pedir socorro, as equipes de resgate não sabem para onde ir e a Defesa Civil coordena as ações às cegas. Foi o que aconteceu no Rio Grande do Sul em 2024, quando municípios inteiros ficaram dias sem comunicação.

O **DisasterLink** nasceu para manter esse canal aberto. A solução completa reúne um **kit de emergência portátil**, uma central de comando para a Defesa Civil e um aplicativo para as equipes de resgate. O kit recebe os pedidos de socorro das vítimas por rádio LoRa e envia as informações pela rede 4G ou, quando ela falha, pelo satélite Iridium. Assim, a comunicação funciona em qualquer lugar do planeta, sem depender da infraestrutura terrestre.

Este repositório traz a **central de comando**, um sistema que roda no terminal. Ela recebe os pedidos de socorro, calcula um **score de urgência** para cada vítima e indica a equipe mais próxima para cada caso, sempre começando pelos mais graves. Também simula os canais de comunicação, com troca automática quando um deles cai, e os sensores que detectam sobreviventes sem que eles precisem pedir ajuda.

O objetivo é simples: que nenhum pedido de socorro se perca quando a comunicação cai, e que o resgate chegue primeiro a quem mais precisa.

> **Importante:** pessoas, equipes e coordenadas do projeto são **fictícias**. Canais e sensores são **simulados**.

---

## Score de urgência

Cada vítima recebe uma nota de **0 a 100**, calculada pelo tipo de emergência, pelas horas sem resposta, por ser ou não uma pessoa vulnerável e pela quantidade de ferimentos:

| Score | Nível |
|-------|-------|
| 85 ou mais | 🔴 Crítico |
| 60 a 84 | 🟠 Alto |
| 35 a 59 | 🟡 Médio |
| Menos de 35 | 🟢 Baixo |

---

## Módulos do sistema

| Módulo | Para que serve |
|---|---|
| **1. Registro e triagem de vítimas** | Cadastrar, listar por urgência, atualizar o status, pesquisar e ver a ficha completa de cada vítima, com a equipe designada e o tempo de chegada. |
| **2. Simulador de comunicação** | Simular o envio por cada canal, a falha em cascata com troca automática, o custo de operação e um comparativo entre os canais. |
| **3. Equipes de resgate e rotas** | Cadastrar, designar e liberar equipes. A vítima mais urgente recebe a equipe mais próxima, calculada pela fórmula de Haversine, com o tempo de chegada pela velocidade da equipe. |
| **4. Simulador de detecção passiva** | Simular sensores térmico, acústico, radar e Bluetooth, um por vez ou numa varredura combinada. |
| **5. Painel e relatórios** | Painel em tempo real, estatísticas da triagem (score médio, desvio padrão e distribuição por nível), mapa de calor no terminal, log e exportação para JSON. |

O menu principal mostra o status em tempo real (vítimas, casos críticos e equipes disponíveis), e a tecla **[D]** carrega um cenário de demonstração pronto para testar.

### Canais de comunicação

| Prioridade | Canal | Latência | Custo | Confiabilidade |
|---|---|---|---|---|
| 1 | Satélite Iridium SBD | ~90 s | US$ 0,04/msg | 95% |
| 2 | Mesh LoRa | ~30 s | Gratuito | 85% |
| 3 | 4G/5G | ~2 s | R$ 35/mês | 65% |
| 4 | Rádio HF | ~5 min | Gratuito | 75% |
| 5 | Wi-Fi (portal cativo) | < 1 s | Gratuito | 50% |

### Sensores de detecção passiva

| Sensor | Alcance | Uso |
|---|---|---|
| Térmico / infravermelho | 50 m | À noite ou com fumaça |
| Acústico | 20 m | Sob escombros |
| Radar | 3 m | Abaixo da superfície |
| Bluetooth LE | 30 m | Sinal do celular da vítima |

---

## Como executar

### Pré-requisitos

- [Python](https://www.python.org/downloads/) 3.10 ou superior (o código usa `match/case`)
  Verifique com: `python --version`

### Pelo terminal

```bash
# Clone o repositório
git clone https://github.com/SpaceConnect-DisasterLink/central-de-comando.git

# Entre na pasta
cd central-de-comando

# Execute
python Disasterlink.py
```

<details>
<summary><b>Pelo VS Code</b></summary>

1. Abra o VS Code e clique em **File > Open Folder**
2. Selecione a pasta do projeto (central-de-comando)
3. Abra o arquivo `Disasterlink.py`
4. Clique em **▶ Run Python File**, no canto superior direito

> **Dica:** ao abrir, aperte **D** no menu principal para carregar o cenário de demonstração e testar todos os módulos com dados prontos.

</details>

---

## Estrutura

```
central-de-comando/
├── Disasterlink.py   ← Código-fonte da central
└── README.md         ← Este arquivo
```

## Dados exportados

O módulo 5 exporta tudo para um arquivo JSON (`disasterlink_AAAAMMDD_HHMMSS.json`):

```json
{
  "exportado_em": "2026-06-02T14:35:00",
  "versao": "DisasterLink — FIAP Global Solution 2026",
  "resumo": { "total_vitimas": 5, "total_equipes": 3, "total_logs": 12 },
  "vitimas": [...],
  "equipes": [...],
  "logs": [...]
}
```

---

## Limitações desta versão

A central é um **protótipo acadêmico**. Nesta versão:

- Os dados ficam na memória enquanto o programa roda. Para guardar, é preciso exportar para JSON.
- Canais de comunicação e sensores são simulados, sem conexão com o kit.
- O tempo de chegada usa a distância em linha reta, sem considerar ruas ou obstáculos.

## Tecnologias utilizadas

- **Python 3.10+:** lógica, menus, simulações e cálculos, só com bibliotecas padrão.
- **JSON:** exportação dos dados.

## Equipe

| Nome | RM |
|---|---|
| Caique Kenji Yafuco | 570368 |
| Guilherme Tomé Nogueira | 570144 |
| Lucas de Andrade Astorini | 569119 |
| Sabrina Lopes da Silva | 571870 |
| Sofia Satomi Hagio | 569120 |
