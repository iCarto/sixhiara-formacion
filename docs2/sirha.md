# SIRHA

A ferramenta SIRHA é um Sistema de Informação Geográfico desenhado sobre aplicações de software livre para o uso do pessoal técnico das ARAs na gestão e planificação dos recursos hídricos no Norte de Mozambique.

A aplicação SIRHA está formada por tres módulos claramente diferenciados:

* Módulo 1: SIG de gestão de inventário. SIRHA: Inventario
* Módulo 2: Módulo de gestão de utentes. SIRHA: Utentes
* Módulo 3: Visor Web (IDE). SIRHA: Visor Web
* Módulo transversal: Base de dados cartográfica, hidrológica e técnica

# SIRHA: Inventario

Aplicativo baseado en gvSIG que emprega os esquemas da base de dados: `inventario`, `inventario_dominios`, `elle` e `cbase_ara` da base de dados.

# SIRHA: Utentes

Aplicativo web que emprega os esquemas da base de dados: `utentes`, `domains`, e `cbase_ara`

# SIRHA: Visor Web

Aplicativo web estático. Os dados são exportados do banco de dados central, mas não precisa de um back-end ou banco de dados para funcionar.

# Base de dados

Base de dados cartográfica, hidrológica e técnica. [Modelo E-R da base de dados](https://icarto.gitlab.io/sixhiara-formacion/)

