# Tentativa 2

## Modelo
- **Algoritmo:** Random Forest Classifier, XGBoost, SVG
- **Hiperparâmetros:** 

## Pré-processamento
- **Features numéricas:** Age, SibSp, Parch, Fare, FamilySize → StandardScaler
- **Features categóricas:** Sex, Embarked, Title → OneHotEncoder (drop='first')
- **Feature engineering:** Tratamento de nulos de Age usando título, feature FamilySize baseado na soma de SibSp + Parch + 1

## Resultados
- **Acurácia no treino:** 
- **Acurácia na validação:**
- **Score no Kaggle:**

## Observações
