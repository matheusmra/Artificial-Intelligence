**Configuração adotada.** Pelo maior F1 médio na validação, adotamos **MissForest + SMOTE + Random Forest + Optuna TPE**
(F1 CV = 0.753, desvio entre folds = 0.006).
No teste, obteve acurácia 0.836, precisão 0.771,
recall 0.810 e F1 0.790. O imputador MissForest
foi escolhido pela qualidade preditiva na validação, considerando conjuntamente balanceamento e modelo;
a menor mudança marginal isolada não foi usada como substituto dessa avaliação.

**Árvore × floresta (4.b).** Entre as configurações selecionadas por validação para cada família,
a árvore obteve F1 de teste 0.781 e acurácia 0.824; a floresta,
F1 0.790 e acurácia 0.836. O melhor F1 observado foi de **Random Forest**.
A diferença floresta − árvore foi 0.009, com intervalo bootstrap de 95%
[-0.034, 0.054], que inclui zero, portanto não sustenta uma vantagem clara nesta amostra.
Assim, generalização é discutida para este teste reservado, sem afirmar superioridade universal.

**Otimizadores (4.c).** Os melhores hiperparâmetros diferiram em 7 das 8
comparações pareadas (valores completos na tabela de buscas). Random Search teve F1 CV médio
0.736 e tempo médio 4.1 s por busca;
Optuna/TPE, 0.737 e 4.1 s.
**Optuna TPE** foi mais rápido em média e **Optuna TPE** teve maior F1 médio de validação.
Esses números expressam o compromisso tempo/qualidade com 12 candidatos; uma única semente e três folds
não permitem afirmar diferença estatisticamente significativa entre otimizadores.

**Imputação (4.d).** O F1 médio de teste foi 0.778 para KNN e
0.775 para MissForest (diferença absoluta
0.002). Essa diferença é descritiva, sem teste
formal de relevância. **KNN** preservou melhor as distribuições marginais numéricas
pelo critério adotado: Wasserstein normalizada média 0.0199 após imputação.
A tabela categórica também mostra o efeito nas proporções de porto. Como há pouquíssimas ausências
de tarifa/porto e não conhecemos os valores verdadeiros faltantes, não se pode concluir que a distribuição
mais parecida represente necessariamente a imputação mais correta.

**Balanceamento (4.e).** SMOTE obteve médias de precisão 0.752,
recall 0.809 e F1 0.779; RandomUnderSampling obteve
0.780, 0.770 e
0.774, respectivamente. Pelo F1 médio, **SMOTE**
produziu o melhor equilíbrio observado para sobreviventes. Ambos igualaram as classes no treino.
O teste conservou sua distribuição original; SMOTE acrescentou exemplos sintéticos e o undersampling
descartou exemplos da maioria. Não incluímos um controle sem balanceamento, portanto não afirmamos
que balancear é superior a não balancear.

**Padrões e interpretabilidade (4.f).** No treino original, sobreviveram 72.0% das mulheres
e 19.6% dos homens; as taxas por classe foram 61.8% (1ª),
42.3% (2ª) e 26.5% (3ª). As regras otimizadas detalham as associações
com idade, tarifa e composição familiar, conforme os caminhos e suportes apresentados na tabela de folhas.
São associações preditivas, não relações causais. Uma árvore permite seguir um caminho de decisões
até a previsão. No Random Forest, cada árvore ainda tem regras legíveis, mas a previsão agrega muitas
árvores treinadas com amostras bootstrap e subconjuntos de atributos; não existe um único caminho curto
que explique integralmente a decisão do conjunto. Essa diversidade tende a reduzir a variância e o
sobreajuste, favorecendo a predição, ao custo de interpretabilidade; a vantagem não é garantida em toda amostra.

**Limitações.** O experimento usa um único holdout aleatório por passageiro, que pode distribuir membros
da mesma família entre treino e teste. Não há avaliação externa nem validação agrupada por família/ticket.
As 16 linhas de teste compartilham passageiros e são comparações descritivas, não 16 amostras independentes.
MissForest é uma aproximação iterativa; a coluna `imputacao_convergiu` informa se o ajuste final atingiu
a tolerância antes do limite. Se falso, usou a última rodada, sem garantir convergência. O SMOTE gera
atributos categóricos/contagens fracionários. Mais sementes, maior orçamento e validação agrupada seriam
necessários para conclusões mais robustas.
