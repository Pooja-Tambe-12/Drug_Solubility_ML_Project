# Drug Solubility Prediction Using Machine Learning

## About the project

Solubility is an important property in drug discovery because it can
affect how a compound behaves during formulation and how much of it is
available for further development.

In this project, I explored whether molecular structure-based
information can be used to predict aqueous solubility (`logS`) using
machine learning.

I used RDKit to convert SMILES structures into molecular descriptors and
then compared different machine learning approaches. I also wanted to
make the evaluation more realistic, so in addition to a conventional
random train-test split, I used a Bemis-Murcko scaffold split to test
how the model performs on compounds with unseen chemical scaffolds.

The main model used in the project is a Random Forest regression model.

## What I worked on

The project covers the following steps:

1.  Loaded and explored the solubility dataset
2.  Checked data types, missing values and basic statistics
3.  Validated SMILES structures using RDKit
4.  Canonicalized SMILES to identify duplicate structures
5.  Investigated duplicate compounds and conflicting solubility values
6.  Generated molecular descriptors using RDKit
7.  Performed exploratory data analysis
8.  Built baseline regression models
9.  Trained Random Forest and Gradient Boosting models
10. Used GridSearchCV for Random Forest hyperparameter tuning
11. Evaluated models using MAE, RMSE and R²
12. Compared random and Bemis-Murcko scaffold splits
13. Compared the machine learning model with a General Solubility
    Equation (GSE) baseline
14. Used permutation importance to understand feature contributions
15. Performed an applicability-domain analysis using Morgan fingerprints
    and Tanimoto similarity

## Dataset

The dataset contains:

-   **1,128 compounds** in the original dataset
-   Compound name
-   SMILES
-   Experimental `logS` solubility value

After SMILES validation and canonicalization, the dataset contained
**1,117 unique structures**.

During duplicate analysis, some structures were found to have different
reported `logS` values. These differences were kept as a data-quality
consideration because experimental solubility can depend on factors such
as measurement conditions, pH, temperature, solid form and experimental
protocol.

## Molecular descriptors

I used RDKit to calculate **18 molecular descriptors**, including:

-   Molecular weight (`MolWt`)
-   LogP (`MolLogP`)
-   Molar refractivity (`MolMR`)
-   Heavy atom count
-   H-bond acceptors
-   H-bond donors
-   Heteroatom count
-   Rotatable bonds
-   Valence electrons
-   Aromatic rings
-   Saturated rings
-   Aliphatic rings
-   Ring count
-   TPSA
-   Labute ASA
-   Balaban J
-   Bertz CT
-   Fraction Csp3

These descriptors were used as the input features for the machine
learning models.

## Machine learning

I compared different regression approaches, including baseline models,
Linear Regression, Random Forest and Gradient Boosting.

Random Forest was selected as the main model and its hyperparameters
were optimized using `GridSearchCV`.

The main evaluation metrics were:

-   **MAE** -- Mean Absolute Error
-   **RMSE** -- Root Mean Squared Error
-   **R²** -- Coefficient of determination

## Results

### Random split

Random Forest performance on the held-out random test set:

  Metric     Result
  -------- --------
  MAE         0.476
  RMSE        0.697
  R²          0.885

### Scaffold split

To make the evaluation more challenging, compounds were split according
to their Bemis-Murcko scaffolds so that the test set contained chemical
scaffolds not present in the training set.

  Metric     Result
  -------- --------
  MAE         0.686
  RMSE        0.955
  R²          0.852

The difference between the random and scaffold results shows why the
choice of train-test splitting strategy matters in molecular machine
learning. A random split can contain structurally similar compounds in
both training and test sets, while a scaffold split provides a more
demanding test of generalization to different chemical structures.

## Comparison with GSE

I also used the General Solubility Equation (GSE) as a simple
chemistry-based baseline.

The comparison was:

  Model / Split                   MAE    RMSE      R²
  --------------------------- ------- ------- -------
  GSE -- Random                 1.297   1.664   0.346
  Random Forest -- Random       0.476   0.697   0.885
  GSE -- Scaffold               1.592   1.864   0.437
  Random Forest -- Scaffold     0.686   0.955   0.852

The GSE provides a useful reference point for understanding how the
descriptor-based machine learning approach compares with a simple
empirical solubility relationship on this dataset.

## Model interpretation

I used permutation importance on the held-out test sets to examine which
molecular descriptors were most useful to the model.

Descriptors related to **lipophilicity, molecular size, polarity and
surface area** were among the more important features.

Feature importance should not be interpreted as proof of causality. It
only shows how much the model's predictive performance changes when a
feature is randomly permuted.

## Applicability domain

I also looked at the applicability domain of the model using Morgan
fingerprints and Tanimoto similarity.

For each test compound, the maximum fingerprint similarity to the
training set was calculated. This gives an indication of whether a
compound is chemically similar to structures the model has already seen.

This is useful because a good prediction for a compound that is very
different from the training data should be treated with more caution.

## What I learned

This project helped me understand the complete workflow of a small
cheminformatics/ML problem, rather than only training a model.

Some of the main things I worked with were:

-   SMILES validation and canonicalization
-   RDKit molecular descriptors
-   Molecular fingerprints and Tanimoto similarity
-   Regression models for QSPR
-   Cross-validation and hyperparameter tuning
-   Random vs scaffold splitting
-   Model evaluation using MAE, RMSE and R²
-   Permutation feature importance
-   Applicability-domain analysis
-   Connecting model results back to chemistry

## Limitations

There are several important limitations to keep in mind.

Solubility is affected by more than molecular structure alone. Factors
such as:

-   pH and ionization
-   pKa
-   Temperature
-   Solid-state properties
-   Crystal form and polymorphism
-   Experimental protocol
-   Differences between experimental datasets

can have a significant effect on measured solubility.

Therefore, this project should be viewed as a **descriptor-based QSPR
model for the `logS` values represented in this dataset**, rather than a
universal drug-solubility prediction model.

The duplicate/conflicting measurements are another data-quality
limitation and could be investigated further with more information about
the experimental conditions.

## Tools used

-   Python
-   Pandas
-   NumPy
-   Matplotlib
-   Seaborn
-   RDKit
-   scikit-learn
-   Google Colab

## Notebook

The main notebook for this project is:

`Drug_Solubility_ML.ipynb`

## Project status

This is a learning and portfolio project where I have tried to focus not
only on building a machine learning model, but also on understanding the
chemistry, data quality and limitations behind the predictions.
