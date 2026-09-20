# Lista 3 — Titanic

O arquivo principal é [`titanic_lista03.ipynb`](titanic_lista03.ipynb). Ele adapta os dois notebooks do Titanic da Lista 2, mantendo a leitura, a divisão estratificada 80/20, a exploração por sexo/classe e a visualização/extração de regras. A base utilizada é `../List02/titanic completo.csv`, que permanece inalterada.

## Execução

Na raiz do repositório, crie um ambiente Python e instale as dependências:

```sh
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r Lists/List03/requirements.txt
```

Abra o notebook no VS Code/Jupyter, selecione esse ambiente como kernel e execute todas as células em ordem. O diretório do kernel pode ser a raiz do repositório ou `Lists/List03`. O notebook já inclui as saídas da execução realizada; não é necessário executar novamente apenas para ler os resultados. As versões registradas foram utilizadas com Python 3.13.

## Experimento

- Imputação: KNNImputer e IterativeImputer com RandomForestRegressor, alternativa à implementação original de MissForest permitida no enunciado.
- Balanceamento: SMOTE e RandomUnderSampling, conforme a exigência explícita do item 1.b.
- Modelos: Árvore de Decisão e Random Forest.
- Otimizadores: RandomizedSearchCV e Optuna/TPE, com 12 candidatos e os mesmos três folds estratificados por busca.
- Total: 16 configurações, 192 avaliações de candidatos e 576 ajustes de validação, além dos ajustes finais.
- Seleção: F1 de sobreviventes na validação. Teste reservado para avaliação final, sem imputação ajustada no teste nem reamostragem do teste.

Imputação e balanceamento são ajustados dentro de cada fold. O cache começa vazio para cada busca e os tempos de ajuste final são separados. `boat` e `body` são removidos por conterem informações posteriores ao desastre. As limitações da imputação aproximada, do SMOTE em dados mistos e do holdout por passageiro são discutidas no notebook.

## Entregáveis em `resultados/`

| Arquivo | Conteúdo |
| --- | --- |
| `comparacao_imputacao_numerica.csv` | Distribuições observadas, somente imputadas e após preenchimento; distância de Wasserstein |
| `comparacao_imputacao_categorica.csv` | Proporções dos portos e variação total |
| `comparacao_balanceamento.csv` | Classes antes e depois de SMOTE/undersampling |
| `hiperparametros_otimizadores.csv` | Melhores parâmetros, F1 de validação e tempos das 16 buscas |
| `metricas_teste.csv` | Acurácia, precisão, recall, F1 e F1 macro por configuração |
| `resultados_consolidados.csv` | Parâmetros, validação, tempos e métricas em uma tabela |
| `historico_buscas.csv` | Todos os candidatos avaliados |
| `regras_arvore.txt` / `arvore_completa.svg` | Regras e visualização completa da árvore otimizada |
| `padroes_regras_arvore.csv` | Caminhos das folhas e suporte nos passageiros originais de treino |
| `conclusao.md` | Discussão dos resultados dos itens 4.b–4.f |

Também são exportados gráficos PNG, resumos por fator, matrizes de confusão, previsões de teste e um intervalo bootstrap exploratório para a diferença de F1 entre os modelos selecionados pela validação. A conclusão não usa o teste para selecionar a configuração final.
