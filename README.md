# Artigo-renov-veis-bess-eletroposto
Produzir um short-paper (4 páginas, padrão IEEE) relacionado a eletropostos e mobilidade elétrico:Integração Energética e Impactos na Rede Energias renováveis, armazenamento, Vehicle-to-Grid (V2G) e qualidade da energia.


# Dependências para: Otimização de Sistemas Híbridos FV+BESS+EV
# Uso: pip install -r requirements.txt
# 
# Versão mínima de Python: 3.9+
# Testado em: Python 3.10, 3.11 (Windows, Linux, macOS)

# Processamento de Dados
pandas>=1.3.0,<2.0.0
numpy>=1.21.0,<1.27.0

# Visualização
matplotlib>=3.4.0,<3.10.0
seaborn>=0.11.0,<0.14.0

# Otimização Matemática
cvxpy>=1.3.0,<1.5.0
scipy>=1.7.0,<1.14.0

# Cálculos Fotovoltaicos
pvlib>=0.9.0,<0.10.5
pytz>=2021.3

# Dados Meteorológicos
requests>=2.27.0,<2.32.0

# Utilitários
python-dateutil>=2.8.2,<2.9.0

# OPCIONAL: Solvers alternativos para otimização
# scs>=3.0.0                    # Solver cónico (recomendado para CVXPY)
# mosek>=10.0.0                 # Solver comercial (requer licença)

# OPCIONAL: Pyomo (para modelo alternativo)
# pyomo>=6.4.0
# gurobi>=10.0.0                # Solver Gurobi (requer licença)

# OPCIONAL: Desenvolvimento e Testes
# pytest>=7.0.0
# pytest-cov>=3.0.0
# black>=22.0.0                 # Formatação de código
# pylint>=2.12.0                # Linting
# ipython>=8.0.0                # IPython shell
# jupyter>=1.0.0                # Jupyter Notebook
