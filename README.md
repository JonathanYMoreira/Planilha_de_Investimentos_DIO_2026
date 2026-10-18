# Renda na Prática: simulador de fundos imobiliários

Planilha em Excel que simula investimentos em fundos imobiliários (FIIs), do aporte de cada mês até os dividendos que ele rende. Feita como desafio prático, a partir do passo a passo do Expert Felipe Aguiar (Felipão), com layout e recursos próprios.

> Os valores da planilha (salário, aportes, taxas e percentuais) são **exemplos**. Nada aqui é recomendação de investimento.

## Arquivo

- `simulador_fiis.xlsx`
  - aba **SIMULADOR**: a ferramenta
  - aba **APOIO**: tabela de perfis, descrição dos tipos de fundo e listas

## Perguntas que a ferramenta responde

| Pergunta | Onde aparece (aba SIMULADOR) |
|---|---|
| Quanto investir por mês? | `C11` (aporte informado) ou `C10` (sugestão de 30% do salário); o valor usado fica em `C13` |
| Por quantos anos? | `C14` |
| Qual a taxa de rendimento mensal? | `C15` |
| Quanto de patrimônio vai acumular? | `F8` |
| Quanto vai receber de dividendos por mês? | `F11` |

Outras seções:

- **Projeção por prazo** (`B20:F24`): patrimônio, total aportado e dividendos em 2, 5, 10, 20 e 30 anos.
- **Comparativo** (`B28:F30`): aporte informado × sugestão do salário, no prazo escolhido.
- **Divisão por perfil** (`B37:E43`): quanto do aporte vai para cada tipo de fundo, com gráfico de pizza.

## Como VF e PROCV entram nos cálculos

**VF** (`FV` na versão em inglês) calcula o valor futuro dos aportes mensais:

```
=VF(taxa_mensal; qtd_anos*12; -aporte_usado)
```

- `taxa_mensal`: rendimento por mês
- `qtd_anos*12`: prazo convertido em meses
- `-aporte_usado`: negativo, para o resultado sair positivo

Os dividendos mensais são o patrimônio vezes o yield mensal: `=patrimonio*yield_mensal`.

**PROCV** com **chave composta** acha o percentual de cada tipo de fundo para o perfil escolhido. Na aba APOIO, a coluna A junta perfil e tipo (por exemplo, `Moderado-TIJOLO`). Na aba SIMULADOR:

```
=PROCV(perfil_escolhido&"-"&$B37; tabela_perfis; 4; FALSO)
```

Trocar o perfil na lista muda os percentuais, os valores e o gráfico.

## Intervalos nomeados

| Nome | Onde está | O que é |
|---|---|---|
| `salario` | SIMULADOR!C8 | Salário mensal |
| `pct_salario` | SIMULADOR!C9 | % do salário sugerido para investir |
| `sugestao_aporte` | SIMULADOR!C10 | Salário × percentual sugerido |
| `aporte_informado` | SIMULADOR!C11 | Aporte digitado |
| `base_aporte` | SIMULADOR!C12 | Qual aporte usar (informado ou sugestão) |
| `aporte_usado` | SIMULADOR!C13 | Aporte da simulação |
| `qtd_anos` | SIMULADOR!C14 | Prazo em anos |
| `taxa_mensal` | SIMULADOR!C15 | Taxa de rendimento mensal |
| `yield_mensal` | SIMULADOR!C16 | Dividend yield mensal |
| `patrimonio` | SIMULADOR!F8 | Patrimônio acumulado |
| `total_aportado` | SIMULADOR!F9 | Total investido no prazo |
| `rendimento_acumulado` | SIMULADOR!F10 | Patrimônio menos total aportado |
| `dividendo_mensal` | SIMULADOR!F11 | Dividendos por mês |
| `perfil_escolhido` | SIMULADOR!C33 | Perfil selecionado |
| `tabela_perfis` | APOIO!A5:D22 | Tabela de chave, perfil, tipo e % |
| `lista_perfis` | APOIO!I5:I7 | Lista da validação de perfil |
| `lista_base` | APOIO!M5:M6 | Lista da validação de base do aporte |

## Percentuais de cada perfil

Os percentuais foram **criados por mim como exemplo didático**. Não vêm de gestora, relatório ou fonte externa, e podem ser editados na coluna D da aba APOIO (cada perfil precisa somar 100%).

| Tipo de FII | Conservador | Moderado | Arrojado |
|---|---|---|---|
| Papel | 35% | 30% | 25% |
| Tijolo | 40% | 30% | 20% |
| Híbridos | 10% | 15% | 10% |
| FOFs | 10% | 10% | 5% |
| Desenvolvimento | 0% | 10% | 25% |
| Hotelarias | 5% | 5% | 15% |
| **Total** | **100%** | **100%** | **100%** |

## O que mudei em relação à ferramenta do Expert

- Layout em duas colunas (parâmetros e respostas) e banner feito com células, sem imagem.
- Lista para escolher a base do aporte (informado ou 30% do salário), com um comparativo lado a lado.
- Resultados extras: total aportado, rendimento acumulado e dividendo como % do salário.
- Cenários de 2 a 30 anos com patrimônio, total aportado e dividendos.
- Intervalos nomeados em todas as fórmulas, inclusive nos cenários.
- Separei a taxa de rendimento mensal (faz o patrimônio crescer) do yield mensal (dividendo sobre o patrimônio).
- Perfis e percentuais próprios (Conservador, Moderado e Arrojado).
- Verificação automática de que cada perfil soma 100%.
- Descrição de cada tipo de fundo na aba APOIO.
- Validação de dados nos campos de entrada e comentários com as premissas.

## Prints

A mesma simulação em dois perfis:

| Conservador | Arrojado |
|---|---|
| [Perfil conservador](prints/conservador.png) | [Perfil arrojado](prints/arrojado.png) |

> Antes de publicar, salve os prints na pasta `prints/` do repositório com esses nomes.

## Como usar

1. Abra `simulador_fiis.xlsx` no Excel.
2. Altere apenas as células de fonte azul com fundo creme.
3. As células cinza são calculadas e não devem ser editadas.
4. Escolha o perfil na lista da seção 5 para ver a divisão do aporte.
