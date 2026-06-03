# Artigo-renov-veis-bess-eletroposto
Produzir um short-paper (4 páginas, padrão IEEE) relacionado a eletropostos e mobilidade elétrico:Integração Energética e Impactos na Rede Energias renováveis, armazenamento, Vehicle-to-Grid (V2G) e qualidade da energia.

# Otimização Inteligente de Sistemas Híbridos FV+BESS+EV em Eletropostos Rodoviários

**[English version below](#english-version)**

---

## 🎯 Resumo Executivo

Este repositório contém a implementação completa de um **sistema de otimização energética para eletropostos híbridos**, integrando:
- **Painéis Fotovoltaicos (FV)** — Geração distribuída com dados reais do NSRDB
- **Sistema de Armazenamento em Bateria (BESS)** — Gerenciamento inteligente de SoC
- **Carregadores de Veículos Elétricos (EV)** — Controle de carga flexível
- **Restrição de Demanda Contratada** — Minimização de violações via programação linear

**Resultado Principal**: Redução de **647,33 kWh** em violações de demanda contratada em horizonte de 24h, com **MAPE de 9,90%** em validação contra PVSYST.

![Badge Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=flat-square)
![Badge License](https://img.shields.io/badge/License-CC--BY--4.0-green?style=flat-square)
![Badge Status](https://img.shields.io/badge/Status-Pesquisa%20Ativa-orange?style=flat-square)

---

## 📋 Conteúdo

1. [Características Principais](#características-principais)
2. [Requisitos & Instalação](#requisitos--instalação)
3. [Quick Start (2 minutos)](#quick-start)
4. [Estrutura de Arquivos](#estrutura-de-arquivos)
5. [Metodologia Técnica](#metodologia-técnica)
6. [Reproduzibilidade](#reproduzibilidade)
7. [Resultados Principais](#resultados-principais)
8. [Referências](#referências)
9. [Citação](#citação)

---

## ✨ Características Principais

### 1️⃣ **Processamento de Dados Meteorológicos (Módulo 1)**
- Extração de irradiância via API **NSRDB** (NASA/NREL)
- Cálculo de posição solar e decomposição de irradiância (Modelo de Erbs)
- Modelagem de perdas térmicas em módulos (Sandia Model)

### 2️⃣ **Simulação de Desempenho FV (Módulo 2)**
- Modelo PVWatts com 195 painéis de 525 Wp
- Validação contra PVSYST com **MAPE = 9,90%** (erro anual < 1%)
- Potência nominal: 80 kWp AC (inversor 80 kVA)

### 3️⃣ **Gerenciamento de Bateria (Módulo 3)**
- BESS com controle de carga/descarga prioritária
- Simulação de ciclos diários com eficiência 95%
- Lógica: carregar durante o dia (máx. 10 kW) → descarregar à noite

### 4️⃣ **Otimização Linear (Módulo 4)**
- Formulação em **CVXPY** (programação convexa)
- Variáveis: potência de carregadores, SoC, violação, curtailment
- Objetivo: minimizar violação da demanda contratada (150 kW)

### 5️⃣ **Exportação Científica (Módulo 5)**
- Gráficos vetoriais em **PDF/SVG** (padrão IEEE de duas colunas)
- Relatorios em Markdown com métricas detalhadas
- Suporte bilíngue (PT-BR e EN)

---

## 🔧 Requisitos & Instalação

### Pré-requisitos
- **Python 3.9+** (testado em 3.10, 3.11)
- **Conda** ou **venv** para ambientes isolados
- **Git** (opcional, para clonar o repositório)

### Dependências Principais
```
pandas>=1.3.0
numpy>=1.21.0
matplotlib>=3.4.0
cvxpy>=1.3.0        # Otimização linear
pvlib>=0.9.0        # Cálculos FV
scipy>=1.7.0        # Estatística
```

### Instalação Passo a Passo

#### Opção A: Com Conda (Recomendado)
```bash
# Clonar repositório
git clone https://github.com/seu-usuario/eletroposto-otimizacao.git
cd eletroposto-otimizacao

# Criar ambiente conda
conda create -n eletroposto-env python=3.11
conda activate eletroposto-env

# Instalar dependências
pip install -r requirements.txt
```

#### Opção B: Com venv
```bash
python -m venv venv
source venv/bin/activate  # Linux/Mac
# ou
venv\Scripts\activate  # Windows

pip install -r requirements.txt
```

### Variáveis de Ambiente

Se usar dados do **NSRDB**, configure sua chave API:
```bash
export NSRDB_API_KEY="sua_chave_aqui"
export NSRDB_EMAIL="seu_email@example.com"
```

---

## 🚀 Quick Start

### 1. Gerar Figuras Vetoriais (2 min)

```bash
python ieee_figure_exporter.py
```

**Saída:**
- Pasta `figuras_ieee/figures/` — PDFs vetoriais em inglês
- Pasta `figuras_ieee_ptbr/figures/` — PDFs vetoriais em português
- Relatórios em Markdown com análise de cada figura

### 2. Rodar Simulação Completa (15 min)

```bash
python v2_usina_solar_e_carregadores_inicial.py
```

**Processa:**
1. Importa dados do NSRDB (2021, localização RAZON/Denver)
2. Calcula posição solar e irradiância incidente
3. Simula geração FV com PVWatts
4. Executa despacho de bateria e otimização de carga de EVs
5. Exporta CSVs e gráficos

---

## 📁 Estrutura de Arquivos

```
.
├── README.md                                    # Este arquivo
├── requirements.txt                             # Dependências Python
│
├── SCRIPTS (Principais)
├── v2_usina_solar_e_carregadores_inicial.py   # ⭐ Script principal completo
├── ieee_figure_exporter.py                      # Exportador IEEE (PDF/SVG)
├── eletroposto_fv_bess_pyomo.py                 # Alternativa com Pyomo
│
├── DADOS DE ENTRADA (CSV)
├── opt_solution_timeseries_with_pv.csv          # Série temporal completa (96 passos)
├── opt_chargers_with_pv_data.csv                # Carregamento de EVs (solicitado vs. otimizado)
├── opt_soc_with_pv_data.csv                     # Estado de carga da bateria (SoC)
├── opt_grid_draw_with_pv_data.csv               # Potência na rede vs. limite contratado
├── opt_energy_summary_with_pv.csv               # Resumo energético (kWh)
│
├── DADOS DE SAÍDA (FIGURAS)
├── figuras_ieee/                                # Figuras em inglês (padrão IEEE)
│   ├── figures/
│   │   ├── figure_01_power_balance.pdf          # Balanço de potência
│   │   ├── figure_02_ev_charging.pdf            # Despacho de carregadores
│   │   ├── figure_03_bess_soc.pdf               # Estado da bateria
│   │   └── figure_04_energy_summary.pdf         # Resumo energético
│   └── report/
│       └── ieee_report.md                       # Relatório técnico (EN)
│
├── figuras_ieee_ptbr/                           # Figuras em português
│   ├── figures/
│   │   └── (mesmos 4 PDFs com rótulos PT-BR)
│   └── report/
│       └── relatorio_ieee_ptbr.md               # Relatório técnico (PT-BR)
│
├── RECURSOS ADICIONAIS
├── Usina_Solar_e_Carregadores_inicial.ipynb     # Notebook Jupyter (versão antiga)
├── ref.bib                                      # Referências BibTeX
├── referenciasintroerevisao.bib                 # Referências adicionais
└── IEEE_Journal_Paper_Template.docx             # Template para manuscrito
```

---

## 🧪 Metodologia Técnica

### Módulos 1 e 2 são referentes a projeto preliminar e estão descritos no notebook `Usina_Solar_e_Carregadores_inicial.ipynb`. Os módulos 3, 4 e 5 são implementados no script `v2_usina_solar_e_carregadores_inicial.py` e detalhados a seguir.

### Modulo 3: Gerenciamento BESS

```
Carga (diurna, 06h-18h):
  P_charge_max = 10 kW
  η_carga = 95%
  SoC_min = 20% | SoC_max = 100%

Descarga (noturna, 20h-01h):
  P_descarga_fixo = 5 kW
  η_descarga = 95%
  SoC_final >= SoC_min
```

### Modulo 4: Otimização Linear (CVXPY)

**Formulação:**
```
minimizar:  1000·Σ(Violação) + 5·Σ(Curtail_EV) + 1·Σ(BESS_uso) + 1·Σ(Curtail_PV)

sujeito a:
  Potência rede ≤ 150 kW (limite contratado)
  0 ≤ P_carregador[i,t] ≤ P_solicitado[i,t]
  SoC[t+1] = SoC[t] + η·P_carga[t]·dt - P_descarga[t]·dt/η
  20 ≤ SoC[t] ≤ 100 kWh
```

**Solver:** SCS (Splitting Conic Solver), tempo típico < 5s

### Modulo 5: Exportação Científica

- **Figuras:** PDF vetorial 300 DPI, fontes Times New Roman
- **Dimensões:** Coluna única (3.5") ou dupla (7.16")
- **Cores:** Paleta acessível (daltonismo considerado)
- **Relatórios:** Markdown com métricas, análises, notas para publicação

---

## 🔄 Reproduzibilidade

### Dados Necessários

1. **Irradiância solar** (NSRDB ou arquivo local)
   - Latitude: 39.7423 °N
   - Longitude: -105.1785 °W (RAZON, Denver)
   - Ano: 2021 (recomendado)

2. **Perfis de carga**
   - Comercial: simulado com picos em 10h e 16h
   - EVs: 10 sessões/dia, distribuídas 7h-20h

3. **Parâmetros do sistema**
   - Painéis: 195 × 525 Wp (fixo, 40°/180°)
   - Inversor: 80 kVA
   - BESS: 30 kWh, eficiência 95%

### Passos para Reproduzir Resultados

```bash
# 1. Verificar dependências
python -c "import cvxpy, pvlib, pandas; print('OK')"

# 2. Rodar simulação completa
python v2_usina_solar_e_carregadores_inicial.py

# 3. Gerar figuras vetoriais
python ieee_figure_exporter.py

# 4. Validar saídas
ls figuras_ieee/figures/  # Deve ter 4 PDFs
ls figuras_ieee_ptbr/figures/  # Deve ter 4 PDFs PT-BR
```

**Tempo esperado:**
- Extração NSRDB: ~2-3 min (primeira vez)
- Simulação: ~5-10 min
- Exportação figuras: ~1 min
- **Total: ~15 min**

---

## 📊 Resultados Principais

### Balanço Energético (24h)

| Métrica | Valor | Unidade |
|---------|-------|---------|
| Demanda comercial | 2.313 | kWh |
| Solicitação EV (não controlado) | 1.005 | kWh |
| Geração FV disponível | 1.002 | kWh |
| **Violação (não controlado)** | **647,33** | **kWh** |
| **Violação (otimizado)** | **~0,0002** | **kWh** |
| **Redução de violação** | **99,99%** | **%** |

### Despacho de Carregadores EV

| Cenário | Energia | Curtailment | Motivo |
|---------|---------|-----------|--------|
| Solicitado | 1.005 kWh | — | Demanda não flexível |
| Otimizado | 859,22 kWh | 145,78 kWh (14,5%) | Respeita limite contratado |

### Performance FV vs. PVSYST

| Métrica | Simulado | PVSYST | Erro |
|---------|----------|--------|------|
| Energia anual | 169,45 MWh | 169,23 MWh | +0,13% |
| MAPE diário | — | — | 9,90% |
| Performance Ratio (PR) | 0,82 | 0,82 | Concordância ✓ |

---

## 📚 Referências

### Artigos Técnicos (BibTeX)
Veja [ref.bib](ref.bib) para lista completa de 40+ referências em:
- Otimização de microrredes
- Modelagem FV (PVWatts, PVSYST)
- Gerenciamento de BESS
- Carregamento de EVs

### Recursos Externos
- [NSRDB Data Viewer](https://nsrdb.nrel.gov/) — Dados de irradiância (livre, requer cadastro)
- [PVLib Documentation](https://pvlib-python.readthedocs.io/)
- [CVXPY (Convex Optimization)](https://www.cvxpy.org/)
- [IEEE Publication Guidelines](https://www.ieee.org/publications/rights/index.html)

---

## 🎓 Citação

Se usar este código em pesquisa, cite como:

**BibTeX:**
```bibtex
@software{eletroposto2026,
  title={Otimização Inteligente de Sistemas Híbridos FV+BESS+EV em Eletropostos},
  author={[Letícia Sampaio Drummond Valladares] and [Athos Lima Bernardes; Hamul Marcel P. F. Freitas; Júlio César Chaves Nunes; Ryan Modesto Cavalheiri Doro; Wendel Lucas Silva, Member]},
  year={2026},
  url={https://github.com/leticiasdrummond/Artigo-renov-veis-bess-eletroposto.git},
  note={FAPESP Processo nº  2025/17250-7}
}
```

**APA:**
```
[Seu Nome] et al. (2026). Otimização Inteligente de Sistemas Híbridos FV+BESS+EV 
em Eletropostos Rodoviários. GitHub Repository. 
https://doi.org/10.5281/zenodo.XXXXX
```

---

## 📧 Contato & Colaboração

- **Autor Principal:** [seu-email@instituição.br]
- **Afiliação:** [Universidade/Departamento]
- **Financiamento:** FAPESP (Fundação de Amparo à Pesquisa do Estado de São Paulo)

Para dúvidas, sugestões ou colaborações:
- Abra uma **Issue** no GitHub
- Envie um **Pull Request** com melhorias
- Contacte por email

---

## ⚖️ Licença

Este projeto é distribuído sob a licença **Creative Commons Attribution 4.0 (CC-BY-4.0)**.

Você é livre para:
- ✅ Usar em pesquisa e educação
- ✅ Modificar e estender
- ✅ Compartilhar (com atribuição)

Veja [LICENSE](LICENSE) para detalhes completos.

---

## 🔗 Links Rápidos

- [Instruções Detalhadas](docs/INSTALL.md) — Setup avançado
- [API Reference](docs/API.md) — Documentação de funções
- [FAQ](docs/FAQ.md) — Perguntas frequentes
- [Contributing](CONTRIBUTING.md) — Guia para colaboradores

---

<a id="english-version"></a>

# Smart Optimization of Hybrid PV+BESS+EV Systems in Highway EV Charging Stations

## 🎯 Executive Summary

This repository contains the complete implementation of an **energy optimization system for hybrid EV charging stations**, integrating:
- **Photovoltaic Panels (PV)** — Distributed generation with real NSRDB data
- **Battery Energy Storage System (BESS)** — Intelligent state-of-charge management
- **EV Chargers** — Flexible charging control
- **Contracted Demand Constraint** — Violation minimization via linear programming

**Main Result**: Reduction of **647.33 kWh** in demand violation over 24h horizon, with **MAPE of 9.90%** validated against PVSYST.

---

## ⚡ Key Features (EN)

1. **Meteorological Data Processing** — NSRDB API integration + Erbs decomposition
2. **PV Performance Simulation** — PVWatts model with 80 kWp (195 × 525 Wp modules)
3. **BESS Management** — Daily charge/discharge cycles with 95% efficiency
4. **Linear Optimization** — CVXPY formulation for demand constraint compliance
5. **Scientific Export** — IEEE-compliant vector graphics (PDF/SVG) + bilingual reports

---

### Quick Start (English)

```bash
# Generate vector figures
python ieee_figure_exporter.py

# Run full simulation
python v2_usina_solar_e_carregadores_inicial.py
```

**Output:** 4 publication-ready figures in `figuras_ieee/figures/` (English) and `figuras_ieee_ptbr/figures/` (Portuguese)

---

For the complete English documentation, please refer to the sections above (translated sections are marked with 🇬🇧).

---

**Last Updated:** June 3, 2026  
**Status:** Active Research · Actively Maintained  
**Python:** 3.9+ · License: CC-BY-4.0

