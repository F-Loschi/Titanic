# Tentativa 2

## Modelo
- **Algoritmos testados:** Random Forest Classifier, XGBoost, SVM
- **Seleção de hiperparâmetros:** GridSearchCV

## Pré-processamento
- **Features numéricas:** Age, FarePerPerson → StandardScaler
- **Features categóricas:** Sex, Embarked, Title, FamilySizeLabel, GroupSizeLabel → OneHotEncoder (drop='first', handle_unknown='ignore')
- **Features passthrough:** Pclass, GroupSize, FamilySize (variáveis discretas ordenadas, não escalonadas)
- **Feature Engineering:**
  - `FamilySize` = SibSp + Parch + 1
  - `GroupSize` = número de passageiros com o mesmo ticket
  - `FarePerPerson` = Fare / GroupSize (valor real pago por pessoa)
  - `GroupSizeLabel` = categorização de GroupSize (solo, pequeno, grande)
  - `FamilySizeLabel` = categorização de FamilySize (solo, pequeno, grande)
  - Imputação de Age via mediana por título (aplicada apenas para `Master`; demais nulos imputados pela mediana geral)

## Resultados
| Modelo | Validação | Kaggle |
|--------|-----------|--------|
| Random Forest | 0.83 | 0.77272 |
| XGBoost | 0.85 | 0.71770 |
| SVM | 0.83 | 0.74641 |
| Stacking Model | 0.83 | 0.74880 |
| Voting Classifier Soft | 0.83 | 0.75837 |
| Voting Classifier Hard | 0.83 | 0.75598 |


## Observações
- **Overfitting do XGBoost:** o modelo apresentou acurácia de 0.85 na validação local mas apenas 0.71 no Kaggle, sugerindo forte overfitting. Na Attempt 3, vale explorar um `learning_rate` maior com menos `n_estimators`, ou adicionar regularização via parâmetros como `min_child_weight` e `subsample`.
- **Ausência de `random_state`:** os modelos foram treinados sem `random_state` fixo, o que pode causar variação nos resultados entre execuções e dificulta a reprodutibilidade. Corrigir na próxima tentativa.
- **Pré-processamento do `GroupSize` no teste:** o `GroupSize` foi calculado apenas com os passageiros do `df_test`, ignorando tickets compartilhados entre treino e teste. Passageiros do teste que compartilham ticket com alguém do treino terão `GroupSize` subestimado.
- **Melhoria na imputação de Age:** a imputação por mediana do título foi aplicada apenas para `Master`. Para os demais títulos, foi usada a mediana geral — uma abordagem mais refinada seria usar a mediana por título para todos eles.

## Links consultados
- [Gradient Boosting](https://www.geeksforgeeks.org/machine-learning/ml-gradient-boosting/)
- [OneHotEncoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html)
- [GridSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html)
- [RandomizedSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.RandomizedSearchCV.html)
- [XGBoost Documentação](https://xgboost.readthedocs.io/en/stable/python/python_api.html#xgboost.XGBClassifier)
- [XGBoost GeeksForGeeks](https://www.geeksforgeeks.org/machine-learning/xgboost/)
- [SVM Documentação](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html)
- [SVM GeeksForGeeks](https://www.geeksforgeeks.org/machine-learning/support-vector-machine-algorithm/)
- [Voting Classifier Documentação](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.VotingClassifier.html)
- [Voting Classifier GeeksForGeeks](https://www.geeksforgeeks.org/machine-learning/voting-classifier/)
- [Stacking Classifier Documentação](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.StackingClassifier.html)
- [Stacking Classifier GeeksForGeeks](https://www.geeksforgeeks.org/machine-learning/stacking-in-machine-learning/)