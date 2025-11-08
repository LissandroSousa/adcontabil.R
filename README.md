# adcontabil: Análise de Demonstrações Contábeis em R

[![Lifecycle: experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

O pacote `adcontabil` é uma ferramenta desenvolvida em R para facilitar a **Análise de Demonstrações Contábeis** (DC), como Balanço Patrimonial (BP) e Demonstração do Resultado do Exercício (DRE). Ele oferece funções para padronizar dados contábeis no formato brasileiro, classificá-los em categorias financeiras e calcular análises verticais, horizontais e um conjunto abrangente de indicadores financeiros.

## Funcionalidades Principais

*   **Padronização de Dados:** Converte automaticamente valores financeiros no formato brasileiro (ex: `1.234,56` e `(500,00)`) para o formato numérico padrão do R.
*   **Categorização Contábil:** Classifica contas detalhadas de BP e DRE em categorias financeiras padronizadas (ex: Ativo Circulante Operacional - ACO, Receita Líquida).
*   **Análise Vertical e Horizontal (AV/AH):** Calcula a evolução e a composição das contas do BP e DRE, incluindo uma projeção simples para o ano seguinte.
*   **Cálculo de Indicadores:** Gera um conjunto completo de indicadores de liquidez, estrutura de capital, margens e rentabilidade (incluindo a decomposição DuPont).

## Instalação

Como o pacote ainda está em desenvolvimento, você pode instalá-lo diretamente do GitHub usando o pacote `devtools`:

```r
# Instale o devtools se ainda não o tiver
# install.packages("devtools")

devtools::install_github("LissandroSousa/adcontabil.R")
```
Ou diretamente pelo Cran:
```r
# instale o pacote
install.packages("adcontabil")
```
## Exemplo de Uso

O fluxo de trabalho típico envolve três etapas: padronizar o Balanço Patrimonial, padronizar a DRE e, em seguida, calcular as análises e indicadores.

### 1. Padronização do Balanço Patrimonial (BP)

A função `padronizar_balanco` recebe um `data.frame` com a primeira coluna sendo o nome da conta e as demais colunas sendo os valores anuais em formato de texto brasileiro.

```r
library(adcontabil)

# Dados de exemplo (formato brasileiro)
df_bp_raw <- data.frame(
  Conta = c("Caixa e equivalentes", "Contas a receber", "Fornecedores", "Patrimônio Líquido"),
  X2022 = c("1.000,00", "5.000,00", "(3.000,00)", "8.000,00"),
  X2023 = c("1.200,00", "6.000,00", "(3.500,00)", "9.000,00")
)

# Padroniza e agrega por categoria
bp_padronizado <- padronizar_balanco(df_bp_raw)

# Visualiza o resultado agregado
print(bp_padronizado$agregado)
```

### 2. Padronização da Demonstração do Resultado (DRE)

Similarmente, a função `padronizar_dre` processa a DRE.

```r
# Dados de exemplo da DRE
df_dre_raw <- data.frame(
  Conta = c("Receita Bruta", "Impostos sobre Vendas", "Custo dos Produtos Vendidos", "Despesas Operacionais", "Resultado Financeiro", "Imposto de Renda"),
  X2022 = c("100.000,00", "(15.000,00)", "(40.000,00)", "(10.000,00)", "5.000,00", "(5.000,00)"),
  X2023 = c("120.000,00", "(18.000,00)", "(48.000,00)", "(12.000,00)", "6.000,00", "(6.000,00)")
)

# Padroniza e agrega por categoria
dre_padronizada <- padronizar_dre(df_dre_raw)

# Visualiza o resultado agregado
print(dre_padronizada$agregado)
```

### 3. Análise Vertical e Horizontal (AV/AH)

Utilize a função `calcular_AV_AH` com o resultado agregado do BP.

```r
# Calcula AV/AH para o Balanço Patrimonial agregado
av_ah_bp <- calcular_AV_AH(bp_padronizado$agregado, tipo = "agregado")

# Análise Vertical e Horizontal
head(av_ah_bp$AV_AH)

# Projeção para o ano seguinte
head(av_ah_bp$Projecao)
```

### 4. Cálculo de Indicadores Financeiros

A função `indicadores` utiliza os resultados agregados do BP e da DRE para calcular os índices.

```r
# Calcula todos os indicadores
indices <- indicadores(bp = bp_padronizado$agregado, dre = dre_padronizada$agregado)

# Indicadores de Balanço Patrimonial (Liquidez e Estrutura)
print(indices$indicadores_bp)

# Indicadores de DRE (Margens)
print(indices$indicadores_dre)

# Indicadores Conjuntos (Rentabilidade e DuPont)
print(indices$indicadores_conjuntos)
```

## Contribuição

Contribuições são bem-vindas! Se você encontrar um bug ou tiver sugestões de melhoria, por favor, abra uma *issue* ou envie um *pull request*.

## Licença

Este projeto está licenciado sob a Licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.
